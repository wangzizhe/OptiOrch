---
layout: default
title: Usage
nav_order: 3
---

# Usage

## Generate Feature Tree/Model from A Modelica File (Optional)

*This step is optional, the idea is mainly for analyzing and understanding variables and parameters in large-scale models because they are normally nested models.*

#### 1. Generate the parser code using ANTLR4

```
antlr4 -Dlanguage=Python3 modelica.g4
```

#### 2. Parse the Modelica model and generate the feature tree

```
python feature_model.py
```

You will see the result in the terminal, it looks like this:

```
SimpleHeatingSystem
├── parameter Real (Q_max)
│   └── Q_max=5000
├── parameter Real (T_set)
│   └── T_set=293.15
├── parameter Real (efficiency)
│   └── efficiency=0.9
├── parameter Real (hysteresis)
│   └── hysteresis=0.5
├── parameter Real (T_comfort)
│   └── T_comfort=293.15
├──  Real (Q_heat)
├──  Real (T_room)
│   └── T_room=()
├──  Real (cost)
│   └── cost=()
├──  Real (energy)
│   └── energy=()
├──  Real (comfort)
│   └── comfort=()
├──  Boolean (heater_on)
│   └── heater_on=()
├── constant Real (c_p)
│   └── c_p=1005
├── constant Real (m_air)
│   └── m_air=50
└──  Real (accumulated_discomfort)
    └── accumulated_discomfort=()
```

The feature model will be then saved as `feature_model.json`. It looks like this:

```json
{
    "name": "SimpleHeatingSystem",
    "children": [
        {
            "name": "parameter Real (Q_max)",
            "children": [
                {
                    "name": "Q_max=5000",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (T_set)",
            "children": [
                {
                    "name": "T_set=293.15",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (efficiency)",
            "children": [
                {
                    "name": "efficiency=0.9",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (hysteresis)",
            "children": [
                {
                    "name": "hysteresis=0.5",
                    "children": []
                }
            ]
        },
        {
            "name": "parameter Real (T_comfort)",
            "children": [
                {
                    "name": "T_comfort=293.15",
                    "children": []
                }
            ]
        },
        {
            "name": " Real (Q_heat)",
            "children": []
        },
        {
            "name": " Real (T_room)",
            "children": [
                {
                    "name": "T_room=()",
                    "children": []
                }
            ]
        },
        {
            "name": " Real (cost)",
            "children": [
                {
                    "name": "cost=()",
                    "children": []
                }
            ]
        },
        {
            "name": " Real (energy)",
            "children": [
                {
                    "name": "energy=()",
                    "children": []
                }
            ]
        },
        {
            "name": " Real (comfort)",
            "children": [
                {
                    "name": "comfort=()",
                    "children": []
                }
            ]
        },
        {
            "name": " Boolean (heater_on)",
            "children": [
                {
                    "name": "heater_on=()",
                    "children": []
                }
            ]
        },
        {
            "name": "constant Real (c_p)",
            "children": [
                {
                    "name": "c_p=1005",
                    "children": []
                }
            ]
        },
        {
            "name": "constant Real (m_air)",
            "children": [
                {
                    "name": "m_air=50",
                    "children": []
                }
            ]
        },
        {
            "name": " Real (accumulated_discomfort)",
            "children": [
                {
                    "name": "accumulated_discomfort=()",
                    "children": []
                }
            ]
        }
    ]
}
```

## Run Optimization

The framework has abstracted all the setup parameters into the file `config.py`. Configure global settings then run `python optimize_main.py`

```python
# Basic settings
SIMULATION_STOP_TIME = 2000  # in seconds
MODEL_NAME = "SimpleHeatingSystem"
MODEL_FILE = f"{MODEL_NAME}.mo"

current_directory = os.getcwd()
model_path = os.path.join(current_directory, MODEL_FILE)

# Parameters and result variables
PARAMETERS = ["Q_max", "T_set"]
RESULTS = ["energy", "cost", "comfort"]

# Parameter bounds
PARAM_BOUNDS = {
    "Q_max": (1000, 5000),
    "T_set": (280, 310),
}

# Algorithm selection
# Options: 'pymoo.NSGA2', 'pymoo.NSGA3', 'pymoo.CMAES', 'scipy.de', 'scipy.minimize'
OPTIMIZATION_LIBRARY = 'pymoo'
ALGORITHM_NAME = 'NSGA2'

# Optimization settings
POP_SIZE = 5  # Population size for NSGA2
N_GEN = 5     # Number of generations

# Parallel processing
N_JOBS = -1  # Options: '-1', '1', 'n', 'None'
```