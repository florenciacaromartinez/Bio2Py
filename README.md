# Bio2Py 
Bio2Py (BioWin to Python) is a Python package that runs BioWin simulations using Python. 
This API is based on the usage of PyAutoGUI Python package to automate loading influent data, running steady-state and dynamic simulations, and saving simulation results without manual intervention. 


Instalation
---------------
Bio2Py can be locally instaled with 'pip'. 
In the root directory containing the file 'Bio2Py-1.1.0.tar.gz', execute the following command:
    
    pip install Bio2Py-1.1.0.tar.gz

The file 'Bio2Py-Env.txt' contains the necessary information to set up an environment for using Bio2Py. 

Usage
---------------
Bio2Py automates the process of loading influent data, running simulations and saving simulation results in BioWin. It is necessary to set the process layout, project options, unit system, report options, etc before using Bio2Py. 

In order to use Bio2Py, BioWin simulation window must be **fully** visible. Since Bio2Py relies on image recognition in order to work, any pop-up message or warning window that covers BioWin program window will cause the API's actions interruption. 

**Before using Bio2Py**
- Open BioWin simulation file.
- Set the zoom to 100% on BioWin window.
- Manually run a single flow balance and steady state/dynamic simulation. Check for any warning messages that could interrupt Bio2Py process. If necessary, the user may consider disabling BioWin alarms to prevent Bio2Py interruption.  
- Choose locations (if applicable): 
    - For saving BioWin generated reports with simulation results (File -> Report to Excel (TM) -> Choose directory).
    - From where variable influent data will be loaded (Influent icon -> Open file -> Choose directory)(***). Only necessary if running simulations with variable influent. Choose the same filepath arg .
- Place BioWin simulation window on the left side of the screen. 
- Make sure influent icon is visible. 

**Setting the environment**
- Import Bio2Py using:


      import Bio2Py
  
- Define the variables:


      simulation_window,influent_window=Bio2Py.setting_the_environment()
- Use Bio2Py funcions to automate simulation process in BioWin. 

Functions summary
---------------
**Main Functions**
- steady_state_simulations
- dynamic_simulations

**Auxiliary functions**

***For loading influent data***
- load_constant_influent
- load_variable_influet

***For running single simulations***
- steady_state
- dynamic

***For saving results***
- save_results

***Other useful functions***
- setting_the_environment
- check_cpu_usage
- maximize
- minimize

Auxiliary functions can be combined to create new functions and streamline the simulation of processes in BioWin. 
The main functions were built based on these auxiliary functions.


Main Functions
----------------
**Steady state simulations**

The 'steady_state_simulations' function allows the user to run **multiple** steady state simulations for constant influent type or variable influent type, with a single command. 

The 'steady_state_simulations' function has the option of running a flow balance before running the steady state simulation, a good practice when working with complex systems. 

Args:
- influent_type: Specify whether the influent is constant ('constant') or variable ('variable') over time.

- influent:
    - If the influent type is constant (DataFrame):
        Each row corresponds to different influent scenarios to be tested. Columns represent influent characteristics in the same order as in BioWin: ['Flow', 'COD (mg-COD/L)', 'TKN (mg-N/L)', 'TP (mg-P/L)', 'TS (mg-S/L)', 'Nitrate (mg-N/L)', 'pH', 'Alkalinity (mmol/L)', 'ISS total (mg-ISS/L)', 'Metal soluble - Ca (mg/L)', 'Metal soluble - Mg (mg/L)', 'Gas - Dissolved O2 (mg/L)'].
    - If the influent type is variable (Dictionary):
        Each Dictionary key corresponds to a different scenario.
            - Each Dictionary key column corresponds to the influent's parameters in the same order of appearance in BioWin: column_names = ['Time','Flow', 'COD (mg-COD/L)', 'TKN (mg-N/L)', 'TP (mg-P/L)', 'TS (mg-S/L)', 'Nitrate (mg-N/L)', 'pH', 'Alkalinity (mmol/L)', 'ISS total (mg-ISS/L)', 'Metal soluble - Ca (mg/L)', 'Metal soluble - Mg (mg/L)', 'Gas - Dissolved O2 (mg/L)'].
            - Each Dictionary key row corresponds to the influent's parameters at the time specified in the time grid (minutes, days, hours).
            - Dictionary entries should be ordered from the one with the least number of rows to the one with the most.

- seed_mass: sets seed values for flow mass balance. Choose between the program options: 'seed' (seed), 'current' (current values), 'last' (last steady state), 'complex' (complex seed). 'None' for not running flow balance.

- seed_SS: sets seed values for the steady state simulation. Choose between the program options: 'seed' (seed), 'current' (current values), 'last' (last steady state), 'complex' (complex seed).

- max_simulation_time: sets the maximum time (in seconds) for finding the steady-state solution of each scenario. If the time for finding the solution exceeds the maximum time, the simulation will stop.

- result_type: 'table' (csv file with results specified in Album. The API copies the results present in the Page 1 of the Album and saves them in a csv file), 'report' (excel report generated by BioWin) , 'complete' (csv file and excel report) ,'none' (does not save simulation results).

- filepath: specifies where the generated files are saved. Use de form r"filepath".
    - For simulations with constant influent, and result type 'table': filepath for saving csv file.
    - For simulations with constant influent, and result type 'report': filepath does not has to be specified. The excel report generated by BioWin will be saved in the specified filepath in BioWin (File->Report to Excel->File path).
    - For simulations with variable influent: a .txt file is generated by the function to ease influent data loading in BioWin. Filepath specifies where to save the .txt file with influent data, The chosen filepath must be the same as the filepath selected in BioWin influent window to load the data (***). Choosing the same filepath where BioWin file is located is recommended.

    
Returns:

Excel report generated by the program or/and a csv file with results specified on the first page of the Album.


**Dynamic simulations** 

The 'dynamic_simulations' function allows the user to run multiple dynamic simulations for a constant or variable influent. 

Runs dynamic simulations for the given data. The data can correspond to a constant or variable influent.

Args:
- influent_type: Specify whether the influent is constant ('constant') or variable ('variable') over time.
- influent:
    - If the influent type is constant (DataFrame):
            Each row corresponds to different influent scenarios to be tested. Columns represent influent characteristics in the same order as in BioWin: ['Flow', 'COD (mg-COD/L)', 'TKN (mg-N/L)', 'TP (mg-P/L)', 'TS (mg-S/L)', 'Nitrate (mg-N/L)', 'pH', 'Alkalinity (mmol/L)', 'ISS total (mg-ISS/L)', 'Metal soluble - Ca (mg/L)', 'Metal soluble - Mg (mg/L)', 'Gas - Dissolved O2 (mg/L)'].
    - If the influent type is variable (Dictionary):
            Each Dictionary key corresponds to a different scenario.
            - Each Dictionary key column corresponds to the influent's parameters in the same order of appearance in BioWin: column_names = ['Time','Flow', 'COD (mg-COD/L)', 'TKN (mg-N/L)', 'TP (mg-P/L)', 'TS (mg-S/L)', 'Nitrate (mg-N/L)', 'pH', 'Alkalinity (mmol/L)', 'ISS total (mg-ISS/L)', 'Metal soluble - Ca (mg/L)', 'Metal soluble - Mg (mg/L)', 'Gas - Dissolved O2 (mg/L)'].
            - Each Dictionary key row corresponds to the influent's parameters at the time specified in the time grid (minutes, days, hours).
            - Dictionary entries should be ordered from the one with the least number of rows to the one with the most.

- start_conditions (str): start conditions for the dynamic simulation. Choose between current values ('current') or last steady state values ('last').

- days (int): length of the dynamic simulation in days.

- max_simulation_time (int): sets the maximum time for running the dynamic simulation. If the time for finding the solution is exceeds the maximum time, the simulation will stop.
  
- filepath (optional): only if influent_type='variable'. When working with variable influen data .txt files are generated for loading influent data to BioWin. Filepath specifies where the .txt files are saved. The chosen filepath must be the same as the filepath selected in BioWin influent window to load the data. Choosing the same filepath where BioWin file is located is recommended. 
    
Returns:

Excel report generated by BioWin.

Implementation example
---------------
The folder ´implementation example´ contains a notebook with a simple example of Bio2Py implementation to illustrate it´s use and applicability. 

In this example the maximum capacity of phosphorus removal of a proposed wastewater treatment plant is evaluated. In order to do this, multiple simulations must be performed by changing the influent TP until the effluent TP is larger than the maximum value accepted by the regulatory constraints. This task is automated using the Bio2Py API.

The corresponding BioWin simulation file can also be found on the folder. 

