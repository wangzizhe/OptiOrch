---
layout: default
title: Case Study
nav_order: 5
---

# Case Study

An orchestration workflow for a self-adaptive edge computing system with MOO4Modelica .

## Scenario

#### Objective

To develop a self-sufficient edge computing system powered by PV, capable of dynamically adjusting its **CPU cores** and **frequency** based on **available energy** and **user demand**. 

The system aims to meet **user demand** while **maximizing energy efficiency** for each time unit.

#### Input

* Energy available (hourly)

* User demand (hourly)

#### Goals:

  - **Meet User Demand:** Ensure the system provides the required **performance** to meet **user demand**.
  - **Maximize Energy Efficiency:** Optimize the use of energy consumption to prolong system operation and efficiency.

#### Outcome 

A dynamically self-adaptive edge computing system that efficiently manages its resources based on real-time **energy availability** and **user demand**, ensuring sustainable and efficient operation for autonomous vehicles requiring edge computing capabilities.

## File Structure

```shell
/src
  |-- MOO4Modelica
  	|-- config.json
  	|-- config.py
  	|-- optimize_main.py
  	|-- parallel_computing.py
  	|-- optimization_libraries.py
  |-- OrchestrationWorkflow
      |-- ITSystem.mo  # Modelica model
      |-- energy_available_and_user_demand_data.txt  # input data
      |-- orchestration_config.json  # config file of the orchestration workflow
      |-- orchestrator.py
      |-- orchestration_wrapper.py
      |-- orchestration_configurator.py
```

## Key Components

1. **Modelica Model**
	* Corresponding file: `ITSystem.mo`
	* Defines the IT system's parameters, such as `activeCores`, `cpuFrequency`, `availableEnergy`, and `userDemand`.
	* Simulates energy consumption, performance, and remaining energy.
2. **Orchestrator & Configurator**
	* Corresponding files: 
	  * `orchestration_config.json`,
	  * `orchestrator.py`, 
	  * `orchestration_wrapper.py`,
	  * `orchestration_configurator.py`
	* `orchestrator.py` as the main script to orchestrate the whole process.
	* Manages the interaction between the optimization framework and the Modelica model.
	* Executes the adaptive control loop, which adjusts the system parameters based on real-time data to meet user demand and optimize performance.

## Process

1. **Data Collection:**
   - Gather hourly data on energy available from PV and user demand.
2. **Optimization with MOO4Modelica Framework:**
   - Use the MOO4Modelica framework to identify the best CPU core and frequency configurations to meet user demand and optimize energy use.
3. **Configuration and Simulation:**
   - Send the optimized configurations to the Modelica model.
   - Simulate the model with these configurations to validate if user demand is met and energy is sufficient.
   - Iterate through configurations until an optimal set is found or determine if no configuration meets the goals.
4. **Adaptive Control Loop:**
   - For each hour:
     - Update the system with current energy and demand.
     - Optimize to find the best parameters.
     - Simulate the model to check if goals are met.
     - Try subsequent configurations if initial ones fail.
5. **Reporting:**
   - Generate a report for each hour indicating:
     - Whether user demand was met.
     - Parameter settings used.
     - Simulation time and actual run time before energy depletion.****

## Demo

* set up the configuration in `orchestration_config.json`

```json
{
    "DATA_FILE_PATH": "energy_available_and_user_demand_data.txt",
    "CONFIG_PATH": "config.json",
    "MODEL_FILE": "ITSystem.mo",
    "MODEL_NAME": "ITSystem",
    "SIMULATION_TIME": 100,
    "START_TIME": 8,
    "END_TIME": 13,
    "TIME_UNIT": "hour",
    "OPTIMIZATION_PARAMETERS": {
        "activeCores": {
            "bounds": [1, 4],
            "type": "int"
        },
        "cpuFrequency": {
            "bounds": [1.0, 3.0],
            "type": "float"
        }
    },
    "EVALUATION_PARAMETERS": {
        "performance": "performance",
        "remaining_energy": "remainingEnergy",
        "energy_consumption": "energyConsumption"
    },
    "INPUT_PARAMETERS": {
        "available_energy": "availableEnergy",
        "user_demand": "userDemand"
    },
    "CRITERIA": {
        "GOAL_EXPRESSION": 
        "evaluation_results['performance'] >= simulation_inputs['user_demand']"
    }
}
```

* run `python orchestrator.py`

```shell
Processing hour: 8  # 8 am
Parameters set: {'activeCores': 4, 'cpuFrequency': 2.47}
Performance: 988.34  # at 8 am user demand is high, the evaluated performance is 988.34 which satisfies user demand
User demand satisfied.

...

Processing hour: 12  # 12 am
Parameters set: {'activeCores': 2, 'cpuFrequency': 1.86}
Performance: 372.13  # # at 12 am user demand is medium, the evaluated performance is 372.13 which satisfies user demand
User demand satisfied.

Final Report:
Hour 8: User demand satisfied with configuration {'activeCores': 4, 'cpuFrequency': 2.47}.

...

Hour 12: User demand satisfied with configuration {'activeCores': 2, 'cpuFrequency': 1.86}.
```

## (Optional) Detailed Logout

Check more details if you are interested.

```shell
C:\Users\Administrator\Desktop\MOO4Modelica\src>python orchestrator.py

Processing hour: 8
Number of variables: 2
Lower bounds: [1. 1.]
Upper bounds: [4. 3.]

Parameters set: {'activeCores': 4, 'cpuFrequency': 1.4}
Parameters set: {'activeCores': 1, 'cpuFrequency': 2.46}
Parameters set: {'activeCores': 2, 'cpuFrequency': 2.41}
Parameters set: {'activeCores': 3, 'cpuFrequency': 1.79}
Parameters set: {'activeCores': 4, 'cpuFrequency': 1.55}
Value for remainingEnergy at 100: 5300.0
Value for remainingEnergy at 100: 6795.000000000001
Value for performance at 100: 560.0
Simulation results: {'remainingEnergy': 5300.0, 'performance': 560.0}
Value for performance at 100: 482.0
Simulation results: {'remainingEnergy': 6795.0, 'performance': 482.0}
Value for remainingEnergy at 100: 7769.999999999999
Value for performance at 100: 246.0
Simulation results: {'remainingEnergy': 7770.0, 'performance': 246.0}
Value for remainingEnergy at 100: 5225.0
Value for performance at 100: 620.0
Simulation results: {'remainingEnergy': 5225.0, 'performance': 620.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 6105.000000000001
Value for performance at 100: 537.0
Simulation results: {'remainingEnergy': 6105.0, 'performance': 537.0}
OMC session closed successfully.
Initial tasks: 5
Results: 5
Results content: [[7770.0, 246.0], [5225.0, 620.0], [6795.0, 482.0], [6105.0, 537.0], [5300.0, 560.0]]
Initial results: [[7770.0, 246.0], [5225.0, 620.0], [6795.0, 482.0], [6105.0, 537.0], [5300.0, 560.0]]
Number of parameter sets evaluated: 5
Expected shape of results: (5, 2)
Processed results: [[-7770.0, -246.0], [-5225.0, -620.0], [-6795.0, -482.0], [-6105.0, -537.0], [-5300.0, -560.0]]
Shape of results array: (5, 2)
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      5 |             - |             -
Elapsed time: 14.57 seconds
Optimization Results:
Solution 0: Remainingenergy = 7770.00, Performance = 246.00,
Solution 1: Remainingenergy = 5225.00, Performance = 620.00,
Solution 2: Remainingenergy = 6795.00, Performance = 482.00,
Solution 3: Remainingenergy = 6105.00, Performance = 537.00,
Solution 4: Remainingenergy = 5300.00, Performance = 560.00,
Results stored in: results\optimization_results.json
Set parameter activeCores to 1: Ok
Set parameter cpuFrequency to 2.4600106155237125: Ok

The IT system can run for 67.4 seconds until the remaining energy is depleted.
Performance: 246.0010615523712
Remaining energy at time 67.4: -3.023577431491276
Energy Consumption: 22.30005307761856
User demand not satisfied.
Set parameter activeCores to 4: Ok
Set parameter cpuFrequency to 1.5540265162269487: Ok

The IT system can run for 31.6 seconds until the remaining energy is depleted.
Performance: 621.6106064907797
Remaining energy at time 31.6: -9.536189563858116
Energy Consumption: 47.77013258113475
User demand not satisfied.
Set parameter activeCores to 2: Ok
Set parameter cpuFrequency to 2.408291071217383: Ok

The IT system can run for 47.0 seconds until the remaining energy is depleted.
Performance: 481.6582142434766
Remaining energy at time 47.0: -5.948401736084776
Energy Consumption: 32.04145535608691
User demand not satisfied.
Set parameter activeCores to 3: Ok
Set parameter cpuFrequency to 1.7863937137727008: Ok

The IT system can run for 38.6 seconds until the remaining energy is depleted.
Performance: 535.9181141318103
Remaining energy at time 38.6: -2.773986758131201
Energy Consumption: 38.9319685688635
User demand not satisfied.
Set parameter activeCores to 4: Ok
Set parameter cpuFrequency to 1.3955752467636957: Ok

The IT system can run for 32.0 seconds until the remaining energy is depleted.
Performance: 558.2300987054783
Remaining energy at time 32.0: -3.292039482191228
Energy Consumption: 46.97787623381848
User demand not satisfied.

Processing hour: 9
Number of variables: 2
Lower bounds: [1. 1.]
Upper bounds: [4. 3.]

Parameters set: {'activeCores': 1, 'cpuFrequency': 2.2}
Parameters set: {'activeCores': 1, 'cpuFrequency': 2.74}
Parameters set: {'activeCores': 2, 'cpuFrequency': 1.62}
Parameters set: {'activeCores': 1, 'cpuFrequency': 1.96}
Parameters set: {'activeCores': 3, 'cpuFrequency': 2.89}
Value for remainingEnergy at 100: 7900.000000000001
Value for performance at 100: 220.0
Simulation results: {'remainingEnergy': 7900.0, 'performance': 220.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7630.0
Value for performance at 100: 274.0
Simulation results: {'remainingEnergy': 7630.0, 'performance': 274.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 8019.999999999999
Value for performance at 100: 196.0
Simulation results: {'remainingEnergy': 8020.0, 'performance': 196.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7190.0
Value for performance at 100: 324.0
Simulation results: {'remainingEnergy': 7190.0, 'performance': 324.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 5555.000000000001
Value for performance at 100: 867.0
Simulation results: {'remainingEnergy': 5555.0, 'performance': 867.0}
OMC session closed successfully.
Initial tasks: 5
Results: 5
Results content: [[7190.0, 324.0], [8020.0, 196.0], [5555.0, 867.0], [7630.0, 274.0], [7900.0, 220.0]]
Initial results: [[7190.0, 324.0], [8020.0, 196.0], [5555.0, 867.0], [7630.0, 274.0], [7900.0, 220.0]]
Number of parameter sets evaluated: 5
Expected shape of results: (5, 2)
Processed results: [[-7190.0, -324.0], [-8020.0, -196.0], [-5555.0, -867.0], [-7630.0, -274.0], [-7900.0, -220.0]]
Shape of results array: (5, 2)
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      5 |             - |             -
Elapsed time: 13.81 seconds
Optimization Results:
Solution 0: Remainingenergy = 7190.00, Performance = 324.00,
Solution 1: Remainingenergy = 8020.00, Performance = 196.00,
Solution 2: Remainingenergy = 5555.00, Performance = 867.00,
Solution 3: Remainingenergy = 7630.00, Performance = 274.00,
Solution 4: Remainingenergy = 7900.00, Performance = 220.00,
Results stored in: results\optimization_results.json
Set parameter activeCores to 2: Ok
Set parameter cpuFrequency to 1.6161235925293533: Ok

The IT system can run for 71.4 seconds until the remaining energy is depleted.
Performance: 323.2247185058706
Remaining energy at time 71.4: -4.956122532978924
Energy Consumption: 28.08061796264676
User demand not satisfied.
Set parameter activeCores to 1: Ok
Set parameter cpuFrequency to 1.9637812018777485: Ok

The IT system ran until the end of the simulation time (100 seconds).
Performance: None
Remaining energy at time 100: None
Energy Consumption: None
User demand not satisfied.
Set parameter activeCores to 3: Ok
Set parameter cpuFrequency to 2.8935220325912816: Ok

The IT system ran until the end of the simulation time (100 seconds).
Performance: None
Remaining energy at time 100: None
Energy Consumption: None
User demand not satisfied.
Set parameter activeCores to 1: Ok
Set parameter cpuFrequency to 2.7447820408609926: Ok

The IT system can run for 84.4 seconds until the remaining energy is depleted.
Performance: 274.4782040860993
Remaining energy at time 84.4: -2.298021243338681
Energy Consumption: 23.72391020430496
User demand not satisfied.
Set parameter activeCores to 1: Ok
Set parameter cpuFrequency to 2.2037988786678286: Ok

The IT system can run for 95.2 seconds until the remaining energy is depleted.
Performance: 220.3798878667829
Remaining energy at time 95.2: -1.008266245886318
Energy Consumption: 21.01899439333915
User demand not satisfied.

Processing hour: 10
Number of variables: 2
Lower bounds: [1. 1.]
Upper bounds: [4. 3.]
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Parameters set: {'activeCores': 3, 'cpuFrequency': 1.43}
Parameters set: {'activeCores': 2, 'cpuFrequency': 1.49}
Parameters set: {'activeCores': 3, 'cpuFrequency': 1.62}
Parameters set: {'activeCores': 1, 'cpuFrequency': 2.03}
Parameters set: {'activeCores': 1, 'cpuFrequency': 1.33}
Value for remainingEnergy at 100: 6285.0
Value for performance at 100: 429.0
Simulation results: {'remainingEnergy': 6285.0, 'performance': 429.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7255.0
Value for performance at 100: 298.0
Simulation results: {'remainingEnergy': 7255.0, 'performance': 298.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 6189.999999999999
Value for performance at 100: 486.0
Simulation results: {'remainingEnergy': 6190.0, 'performance': 486.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7984.999999999999
Value for performance at 100: 203.0
Simulation results: {'remainingEnergy': 7985.0, 'performance': 203.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 8335.0
Value for performance at 100: 133.0
Simulation results: {'remainingEnergy': 8335.0, 'performance': 133.0}
OMC session closed successfully.
Initial tasks: 5
Results: 5
Results content: [[6285.0, 429.0], [7985.0, 203.0], [6190.0, 486.0], [8335.0, 133.0], [7255.0, 298.0]]
Initial results: [[6285.0, 429.0], [7985.0, 203.0], [6190.0, 486.0], [8335.0, 133.0], [7255.0, 298.0]]
Number of parameter sets evaluated: 5
Expected shape of results: (5, 2)
Processed results: [[-6285.0, -429.0], [-7985.0, -203.0], [-6190.0, -486.0], [-8335.0, -133.0], [-7255.0, -298.0]]
Shape of results array: (5, 2)
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      5 |             - |             -
Elapsed time: 13.57 seconds
Optimization Results:
Solution 0: Remainingenergy = 6285.00, Performance = 429.00,
Solution 1: Remainingenergy = 7985.00, Performance = 203.00,
Solution 2: Remainingenergy = 6190.00, Performance = 486.00,
Solution 3: Remainingenergy = 8335.00, Performance = 133.00,
Solution 4: Remainingenergy = 7255.00, Performance = 298.00,
Results stored in: results\optimization_results.json
Set parameter activeCores to 3: Ok
Set parameter cpuFrequency to 1.429426196112012: Ok

The IT system ran until the end of the simulation time (100 seconds).
Performance: 428.8278588336036
Remaining energy at time 100: 1285.286901943994
Energy Consumption: 37.14713098056006
User demand satisfied.

Processing hour: 11
Number of variables: 2
Lower bounds: [1. 1.]
Upper bounds: [4. 3.]
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Parameters set: {'activeCores': 1, 'cpuFrequency': 2.58}
Parameters set: {'activeCores': 4, 'cpuFrequency': 2.77}
Parameters set: {'activeCores': 2, 'cpuFrequency': 2.47}
Parameters set: {'activeCores': 3, 'cpuFrequency': 2.63}
Parameters set: {'activeCores': 1, 'cpuFrequency': 1.65}
Value for remainingEnergy at 100: 5685.000000000001
Value for performance at 100: 789.0
Value for remainingEnergy at 100: 6764.999999999999
Simulation results: {'remainingEnergy': 5685.0, 'performance': 789.0}
Value for performance at 100: 494.0000000000001
Simulation results: {'remainingEnergy': 6765.0, 'performance': 494.0}
Value for remainingEnergy at 100: 8175.000000000001
Value for remainingEnergy at 100: 4615.0
Value for performance at 100: 165.0
Simulation results: {'remainingEnergy': 8175.0, 'performance': 165.0}
Value for performance at 100: 1108.0
Simulation results: {'remainingEnergy': 4615.0, 'performance': 1108.0}
OMC session closed successfully.
OMC session closed successfully.
OMC session closed successfully.
Value for remainingEnergy at 100: 7710.000000000001
OMC session closed successfully.
Value for performance at 100: 258.0
Simulation results: {'remainingEnergy': 7710.0, 'performance': 258.0}
OMC session closed successfully.
Initial tasks: 5
Results: 5
Results content: [[4615.0, 1108.0], [6765.0, 494.0], [7710.0, 258.0], [8175.0, 165.0], [5685.0, 789.0]]
Initial results: [[4615.0, 1108.0], [6765.0, 494.0], [7710.0, 258.0], [8175.0, 165.0], [5685.0, 789.0]]
Number of parameter sets evaluated: 5
Expected shape of results: (5, 2)
Processed results: [[-4615.0, -1108.0], [-6765.0, -494.0], [-7710.0, -258.0], [-8175.0, -165.0], [-5685.0, -789.0]]
Shape of results array: (5, 2)
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      5 |             - |             -
Elapsed time: 159.27 seconds
Optimization Results:
Solution 0: Remainingenergy = 4615.00, Performance = 1108.00,
Solution 1: Remainingenergy = 6765.00, Performance = 494.00,
Solution 2: Remainingenergy = 7710.00, Performance = 258.00,
Solution 3: Remainingenergy = 8175.00, Performance = 165.00,
Solution 4: Remainingenergy = 5685.00, Performance = 789.00,
Results stored in: results\optimization_results.json
Set parameter activeCores to 4: Ok
Set parameter cpuFrequency to 2.7706749141785: Ok

The IT system can run for 93.0 seconds until the remaining energy is depleted.
Performance: 1108.2699656714
Remaining energy at time 93.0: -8.36383509300228
Energy Consumption: 53.8533745708925
User demand satisfied.

Processing hour: 12
Number of variables: 2
Lower bounds: [1. 1.]
Upper bounds: [4. 3.]
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Parameters set: {'activeCores': 2, 'cpuFrequency': 2.0}
Parameters set: {'activeCores': 2, 'cpuFrequency': 1.42}
Parameters set: {'activeCores': 2, 'cpuFrequency': 1.98}
Parameters set: {'activeCores': 3, 'cpuFrequency': 1.77}
Parameters set: {'activeCores': 4, 'cpuFrequency': 2.87}
Value for remainingEnergy at 100: 7000.0
Value for performance at 100: 400.0
Simulation results: {'remainingEnergy': 7000.0, 'performance': 400.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7290.0
Value for performance at 100: 284.0
Simulation results: {'remainingEnergy': 7290.0, 'performance': 284.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 7010.000000000002
Value for performance at 100: 396.0
Simulation results: {'remainingEnergy': 7010.0, 'performance': 396.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 6114.999999999999
Value for performance at 100: 531.0
Simulation results: {'remainingEnergy': 6115.0, 'performance': 531.0}
OMC session closed successfully.
Value for remainingEnergy at 100: 4564.999999999998
Value for performance at 100: 1148.0
Simulation results: {'remainingEnergy': 4565.0, 'performance': 1148.0}
OMC session closed successfully.
Initial tasks: 5
Results: 5
Results content: [[7010.0, 396.0], [4565.0, 1148.0], [7000.0, 400.0], [6115.0, 531.0], [7290.0, 284.0]]
Initial results: [[7010.0, 396.0], [4565.0, 1148.0], [7000.0, 400.0], [6115.0, 531.0], [7290.0, 284.0]]
Number of parameter sets evaluated: 5
Expected shape of results: (5, 2)
Processed results: [[-7010.0, -396.0], [-4565.0, -1148.0], [-7000.0, -400.0], [-6115.0, -531.0], [-7290.0, -284.0]]
Shape of results array: (5, 2)
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      5 |             - |             -
Elapsed time: 13.66 seconds
Optimization Results:
Solution 0: Remainingenergy = 7010.00, Performance = 396.00,
Solution 1: Remainingenergy = 4565.00, Performance = 1148.00,
Solution 2: Remainingenergy = 7000.00, Performance = 400.00,
Solution 3: Remainingenergy = 6115.00, Performance = 531.00,
Solution 4: Remainingenergy = 7290.00, Performance = 284.00,
Results stored in: results\optimization_results.json
Set parameter activeCores to 2: Ok
Set parameter cpuFrequency to 1.9778515120327718: Ok

The IT system ran until the end of the simulation time (100 seconds).
Performance: None
Remaining energy at time 100: None
Energy Consumption: None
User demand not satisfied.
Set parameter activeCores to 4: Ok
Set parameter cpuFrequency to 2.872662699566768: Ok

The IT system can run for 92.0 seconds until the remaining energy is depleted.
Performance: 1149.065079826707
Remaining energy at time 92.0: -1.424841800712812
Energy Consumption: 54.36331349783384
User demand satisfied.

Final Report:
Hour 8: No sufficient configuration found. Simulation ran for 32.0 seconds out of 100 seconds.
Hour 9: No sufficient configuration found. Simulation ran for 95.2 seconds out of 100 seconds.
Hour 10: User demand satisfied with configuration {'activeCores': 3, 'cpuFrequency': 1.429426196112012}. Simulation ran for 100 seconds out of 100 seconds.
Hour 11: User demand satisfied with configuration {'activeCores': 4, 'cpuFrequency': 2.7706749141785}. Simulation ran for 93.0 seconds out of 100 seconds.
Hour 12: User demand satisfied with configuration {'activeCores': 4, 'cpuFrequency': 2.872662699566768}. Simulation ran for 92.0 seconds out of 100 seconds.
```

