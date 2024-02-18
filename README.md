# Bio2Py 
Bio2Py (BioWin to Python) is a Python package that runs BioWin simulations using Python. 
This API is based on the usage of PyAutoGUI Python package to automate loading influent data, running steady-state and dynamic simulations, and saving simulation results without manual intervention. 

Bio2Py, developed in Python 3.12.0, operates with BioWin versions 6.2 and 6.0. 


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

The 'Bio2Py Test' folder contains a file titled 'Bio2Py Test and Usage video' with a video demonstrating these steps.

Functions summary
---------------
**Main Functions**
- steady_state_simulations: runs multiple steady state simulations for constant/variable influent and saves simulation results with a single command.
- dynamic_simulations: runs multiple dynamic simulations for a constant/variable influent and saves simulation results with a single command. 

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
The main functions were built based on the auxiliary functions.
More information regarding the functions is available in the file 'Bio2Py Functions'. 

Test
--------------
The 'Bio2Py' test folder contains a notebook designed to test the main functions of Bio2Py. Additionally, it includes a BioWin file that can be utilized for testing purposes. Since the main functions are built using the auxiliary functions, if the test of the main functions is successful, the user should not have problems using the auxiliary functions.

The folder also includes a video titled 'Bio2Py Test and Usage video', which demonstrates the testing of Bio2Py. This video provides users with a better understanding of how Bio2Py works and its usage. It indicates when the user is active and when Bio2Py performs actions.


Implementation examples 
---------------
**Implementation example 1: Evaluation of Phosphorus Removal Capacity of an EBPR System**

The folder ´Implementation Example 1´ contains a notebook with a simple example of Bio2Py implementation to illustrate it's use and applicability. 

In this example, the maximum capacity of phosphorus removal of a proposed wastewater treatment plant is evaluated. In order to do this, multiple simulations must be performed by changing the influent TP until the effluent TP is larger than the maximum value accepted by the regulatory constraints. This task is automated using the Bio2Py API.

The corresponding BioWin simulation file can also be found on the folder. A video is also included, allowing the user to see Bio2Py in action.

**Implementation example 2: Evaluation of Chemical Consumption for Physicochemical Phosphorus Removal**

The folder implementation example 2 contains a notebook with a simple example of how the user can combine Bio2Py auxiliary functions to automate diffrent actions in BioWin that are not defined on first hand as main functions in Bio2Py. It also demonstrates how Bio2Py can enhance data acquisition processes. 

In this example, we explore a physicochemical phosphorus removal system using FeCl3, within a specified influent. NaOH is used for adjusting pH.
The goal was to generate response surfaces for effluent TP and pH in relation to FeCl3 and NaOH consumption. This analysis could assist in determining the optimal combination of chemicals for influents with particular characteristics. 

***Steps***
1. Define a range of FeCl3 and NaOH flow rates to test.
2. Randomly select N (FeCl3, NaOH) pairs within these ranges.
3. Run N simulations in BioWin for the correspondig (FeCl3, NaOH) pais. --> This can be automated using Bio2Py auxiliary functions!
4. Save simulation results for each (FeCl3, NaOH) pair. --> This can be automated using Bio2Py auxiliary functions!
5. Visualize the results to understand how P and pH vary with FeCl3 and NaOH.

***Benefits of Bio2Py***

Bio2Py simplifies the execution of numerous simulations. In this example, 370 simulations were conducted by the API, enabling the generation of response surfaces for effluent TP and pH regarding FeCl3 and NaOH consumption. Analysis of these surfaces shows that various combinations of FeCl3 and NaOH achieve comparable levels of P removal.  other criteria such as minimizing costs, could be considered to determine the most suitable chemical consumption for the effluent plant.
Manually performing these simulations would be laborious and prone to data transfer errors.
