---
layout: default
title: Usage
parent: MOO4Modelica
nav_order: 1
---

# Usage

### I. Generate Feature Tree from A Modelica Model (Optional)

*This step is optional, the idea is mainly for analyzing and understanding variables and parameters in large-scale models because they are normally nested models.*

**Step 1:** Generate the parser code using ANTLR4

```
antlr4 -Dlanguage=Python3 modelica.g4
```

**Step 2:** Configuration

configure `parse_modelica.py`

```python
# Configuration
file_name = 'SimpleHeatingSystem.mo'
```

configure `feature_model.py`

```python
# Configuration
model_name = 'SimpleHeatingSystem.mo'
output_file = 'feature_model.json'
include_equations = True  # set to "False" if equations should be excluded
```

**Step 3:** Parse the Modelica model and generate the feature tree

```
python feature_model.py
```

### II. Run Optimization

The framework has abstracted all the setup parameters into the file `config.py`. 

**Step 1:** Configure global settings in `config.py` 

```shell
# Basic settings
"MODEL_NAME": "SimpleHeatingSystem",
"MODEL_FILE": "SimpleHeatingSystem.mo",
"SIMULATION_STOP_TIME": 3000,  # in seconds

# Results precision
"PRECISION": 2,  # decimal places

"PARAMETERS": ["param1", "param2"]
"OBJECTIVES": [
    {"name": "object1", "maximize": false},
    {"name": "object2", "maximize": true}
],

"PARAM_BOUNDS": {
    "param1": {
        "bounds": [1000, 5000],
        "type": "float"
    },
    "param2": {
        "bounds": [280, 310],
        "type": "int"
    }
},

"OPTIMIZATION_CONFIG": {
    "USE_SINGLE_OBJECTIVE": false,
    "ALGORITHM_NAME": "nsga2",
    "POP_SIZE": 100,
    "N_GEN": 100
},

"PLOT_CONFIG": {
    "PLOT_X": "",
    "PLOT_Y": "",
    "PLOT_TITLE": "",
    "ENABLE_PLOT": false
},

# import libraries that the model needs
"LIBRARY_CONFIG": {
    "LOAD_LIBRARIES": false,
    "LIBRARIES": [
        {"name": "", "path": ""}
    ]
},

# Parallel computing
"N_JOBS": -1  # Options: '-1', '1', 'n', 'None'
```

**Step 2:** run `python optimize_main.py`

Now, take a cup of tee and wait for the results xD