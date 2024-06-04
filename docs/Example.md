---
layout: default
title: Example
nav_order: 4
---

# Example

Case study with a simple heating system [SimpleHeatingSystem.mo](./SimpleHeatingSystem.mo) built in Modelica.

The idea is to heat a room, when the room heats faster, human beings feels more comfortable, but the energy consumption and cost become higher. This system should be optimized to find the **trade-off of energy consumption and comfort**.

hyperparameters (these are the parameters to be varied): 

```
  parameter Real Q_max = 5000 "Maximum heating power in watts";
  parameter Real T_set = 293.15 "Setpoint temperature in Kelvin";
```

### I. Generate Feature Tree/Model from A Modelica File (Optional)

1. Generate the parser code using ANTLR4

```
antlr4 -Dlanguage=Python3 modelica.g4
```

There are files which haven been generated, e.g.:
* `modelicaLexer.py`
* `modelicaParser.py`
* `modelicaListener.py`

2. Configuration

configurate `parse_modelica.py`

```python
# Configuration
file_name = 'SimpleHeatingSystem.mo'
```

configurate `feature_model.py`

```python
# Configuration
model_name = 'SimpleHeatingSystem.mo'
output_file = 'feature_model.json'
include_equations = True  # set to "False" if equations should be excluded
```

3. Parse the Modelica model and generate the feature tree

```
python feature_model.py
```

You will see the result in the terminal:

```
SimpleHeatingSystem
├── parameter Real (Q_max)
│   └── value=5000
├── parameter Real (T_set)
│   └── value=293.15
├── parameter Real (efficiency)
│   └── value=0.9
├── parameter Real (hysteresis)
│   └── value=0.5
├── parameter Real (T_comfort)
│   └── value=293.15
├── Real (Q_heat)
├── Real (T_room)
│   └── value=()
├── Real (cost)
│   └── value=()
├── Real (energy)
│   └── value=()
├── Real (comfort)
│   └── value=()
├── Boolean (heater_on)
│   └── value=()
├── constant Real (c_p)
│   └── value=1005
├── constant Real (m_air)
│   └── value=50
├── Real (accumulated_discomfort)
│   └── value=()
├── Equation 1: der(T_room)=(Q_heat*efficiency-(T_room-273.15))/(m_air*c_p)
├── Equation 2: heater_on=ifT_room<T_set-hysteresisthentrueelseifT_room>T_set+hysteresisthenfalseelsepre(heater_on)
├── Equation 3: Q_heat=ifheater_onthenQ_maxelse0
├── Equation 4: der(energy)=Q_heat*efficiency
├── Equation 5: der(cost)=0.1*Q_heat*efficiency
├── Equation 6: der(accumulated_discomfort)=abs(T_room-T_comfort)
└── Equation 7: der(comfort)=ifheater_onthenefficiency*Q_heat/(accumulated_discomfort+1)else0
```

The feature model will be then saved as `feature_model.json`:

```json
{
    "name": "SimpleHeatingSystem",
    "children": [
        {
            "name": "parameter Real (Q_max)",
            "children": [
                {
                    "name": "value=5000",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (T_set)",
            "children": [
                {
                    "name": "value=293.15",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (efficiency)",
            "children": [
                {
                    "name": "value=0.9",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (hysteresis)",
            "children": [
                {
                    "name": "value=0.5",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (T_comfort)",
            "children": [
                {
                    "name": "value=293.15",
                    "children": []
                }
            ]
        },
        {
            "name": "Real (Q_heat)",
            "children": []
        },
        {
            "name": "Real (T_room)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "Real (cost)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "Real (energy)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "Real (comfort)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "Boolean (heater_on)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "constant Real (c_p)",
            "children": [
                {
                    "name": "value=1005",
                    "children": []
                }
            ]
        },
        {
            "name": "constant Real (m_air)",
            "children": [
                {
                    "name": "value=50",
                    "children": []
                }
            ]
        },
        {
            "name": "Real (accumulated_discomfort)",
            "children": [
                {
                    "name": "value=()",
                    "children": []
                }
            ]
        },
        {
            "name": "Equation 1: der(T_room)=(Q_heat*efficiency-(T_room-273.15))/(m_air*c_p)",
            "children": []
        },
        {
            "name": "Equation 2: heater_on=ifT_room<T_set-hysteresisthentrueelseifT_room>T_set+hysteresisthenfalseelsepre(heater_on)",
            "children": []
        },
        {
            "name": "Equation 3: Q_heat=ifheater_onthenQ_maxelse0",
            "children": []
        },
        {
            "name": "Equation 4: der(energy)=Q_heat*efficiency",
            "children": []
        },
        {
            "name": "Equation 5: der(cost)=0.1*Q_heat*efficiency",
            "children": []
        },
        {
            "name": "Equation 6: der(accumulated_discomfort)=abs(T_room-T_comfort)",
            "children": []
        },
        {
            "name": "Equation 7: der(comfort)=ifheater_onthenefficiency*Q_heat/(accumulated_discomfort+1)else0",
            "children": []
        }
    ]
}
```

### II. Run Optimization

1. Configurate global settings for the optimization

```python
# Basic settings
MODEL_NAME = "SimpleHeatingSystem"
MODEL_FILE = f"{MODEL_NAME}.mo"
MODEL_PATH = os.path.join(os.getcwd(), MODEL_FILE)
SIMULATION_STOP_TIME = 3000  # in seconds

# Parameters and result variables
PARAMETERS = ["Q_max", "T_set"]
RESULTS = ["energy", "comfort"]
MAXIMIZE = ["comfort"]  # List of objectives to maximize

# Parameter range
PARAM_BOUNDS = {
    "Q_max": (1000, 5000),
    "T_set": (280, 310),
}

# Results precision
PRECISION = 2  # decimal places

# Plot configurations
PLOT_CONFIG = {
    "PLOT_X": "Energy Consumption",
    "PLOT_Y": "Comfort",
    "PLOT_TITLE": "Pareto Front of Energy Consumption vs Comfort"
}

# Algorithm selection
# Options: 'pymoo.NSGA2', 'pymoo.NSGA3', 'pymoo.CMAES', 'scipy.de', 'scipy.minimize'
OPTIMIZATION_LIBRARY = 'pymoo'
ALGORITHM_NAME = 'NSGA2'

# Optimization settings
POP_SIZE = 50  # Population size for NSGA2
N_GEN = 100     # Number of generations

# Parallel processing
N_JOBS = -1  # Options: '-1', '1', 'n', 'None'
```

2. Run Python script: `python optimize_main.py`

You will see 

* the models have been loaded in different workers for parallel computing
* parameters have been set for different models
* simulated results of each model

like this:

```shell
Number of variables: 2
Lower bounds: [1000  280]
Upper bounds: [5000  310]

Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True

Parameters set: {'Q_max': 4173.26, 'T_set': 284.49}
Parameters set: {'Q_max': 1220.76, 'T_set': 305.78}
Parameters set: {'Q_max': 1951.81, 'T_set': 299.48}
Parameters set: {'Q_max': 4675.92, 'T_set': 289.72}
Parameters set: {'Q_max': 2044.53, 'T_set': 305.47}

...
```

The simulation is runing:

```shell
Simulation results: {'energy': 344482.35, 'comfort': 1873.97}
OMC session closed successfully.
Simulation results: {'energy': 1490363.54, 'comfort': 836.97}
OMC session closed successfully.
Simulation results: {'energy': 608112.5, 'comfort': 2298.91}
OMC session closed successfully.
Simulation results: {'energy': 1158015.95, 'comfort': 1213.1}
OMC session closed successfully.
Simulation results: {'energy': 778781.79, 'comfort': 1521.05}
OMC session closed successfully.
...
```

You will see the results at the end of simulation like this:

```shell
...
   100 |     5000 |     50 |  0.0031202395 |             f
Optimization Results:
Solution 0: Energy = 1683944.96, Comfort = 3074.81,
Solution 1: Energy = 118249.77, Comfort = 1817.60,
Solution 2: Energy = 1683944.96, Comfort = 3074.81,
Solution 3: Energy = 118249.77, Comfort = 1817.60,
Solution 4: Energy = 990666.54, Comfort = 2706.51,
Solution 5: Energy = 911860.57, Comfort = 2648.50,
Solution 6: Energy = 515215.82, Comfort = 2359.04,
Solution 7: Energy = 835602.55, Comfort = 2606.62,
Solution 8: Energy = 745744.39, Comfort = 2517.19,
Solution 9: Energy = 1157466.41, Comfort = 2821.38,
Solution 10: Energy = 454719.13, Comfort = 2303.91,
Solution 11: Energy = 1296560.30, Comfort = 2904.91,
Solution 12: Energy = 374082.06, Comfort = 2223.35,
Solution 13: Energy = 283399.71, Comfort = 2117.14,
Solution 14: Energy = 716472.49, Comfort = 2491.76,
Solution 15: Energy = 248647.78, Comfort = 2069.58,
Solution 16: Energy = 1366392.43, Comfort = 2942.05,
Solution 17: Energy = 1021489.97, Comfort = 2728.62,
Solution 18: Energy = 1517262.96, Comfort = 3011.96,
Solution 19: Energy = 1097303.16, Comfort = 2781.52,
Solution 20: Energy = 136370.09, Comfort = 1864.18,
Solution 21: Energy = 158015.71, Comfort = 1913.03,
Solution 22: Energy = 1422577.37, Comfort = 2969.70,
Solution 23: Energy = 1586150.01, Comfort = 3039.63,
Solution 24: Energy = 787135.29, Comfort = 2555.69,
Solution 25: Energy = 1547145.24, Comfort = 3024.26,
Solution 26: Energy = 1473206.51, Comfort = 2992.92,
Solution 27: Energy = 404821.56, Comfort = 2255.21,
Solution 28: Energy = 558581.73, Comfort = 2396.62,
Solution 29: Energy = 787135.29, Comfort = 2555.69,
Solution 30: Energy = 1217645.63, Comfort = 2859.06,
Solution 31: Energy = 864384.88, Comfort = 2620.60,
Solution 32: Energy = 660473.92, Comfort = 2480.77,
Solution 33: Energy = 530846.76, Comfort = 2372.74,
Solution 34: Energy = 347377.37, Comfort = 2194.18,
Solution 35: Energy = 1256593.65, Comfort = 2882.21,
Solution 36: Energy = 152981.66, Comfort = 1902.23,
Solution 37: Energy = 649879.03, Comfort = 2472.21,
Solution 38: Energy = 892160.79, Comfort = 2636.94,
Solution 39: Energy = 206851.82, Comfort = 2004.69,
Solution 40: Energy = 1453971.43, Comfort = 2982.01,
Solution 41: Energy = 238575.76, Comfort = 2054.81,
Solution 42: Energy = 184698.05, Comfort = 1965.72,
Solution 43: Energy = 125296.39, Comfort = 1836.44,
Solution 44: Energy = 448166.30, Comfort = 2297.71,
Solution 45: Energy = 1387654.91, Comfort = 2950.49,
Solution 46: Energy = 1051306.87, Comfort = 2749.67,
Solution 47: Energy = 298007.19, Comfort = 2135.79,
Solution 48: Energy = 1064447.73, Comfort = 2758.86,
Solution 49: Energy = 319668.29, Comfort = 2162.22,
```

After that you will see the Pareto front:

![Pareto Front of the Simple Heating System](../assets/SimpleHeatingSystem_Pareto_Front.png) 
