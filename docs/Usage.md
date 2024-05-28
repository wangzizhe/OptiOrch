---
layout: default
title: Usage
nav_order: 3
---

# Usage

## I. Generate Feature Tree/Model from A Modelica File (Optional)

*This step is optional, the idea is mainly for analyzing and understanding variables and parameters in large-scale models because they are normally nested models.*

1. Generate the parser code using ANTLR4

```
antlr4 -Dlanguage=Python3 modelica.g4
```

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

## II. Run Optimization

The framework has abstracted all the setup parameters into the file `config.py`. 

1. Configure global settings in `config.py` 

```python
# Basic settings
MODEL_NAME = "SimpleHeatingSystem"
MODEL_FILE = f"{MODEL_NAME}.mo"
MODEL_PATH = os.path.join(os.getcwd(), MODEL_FILE)
SIMULATION_STOP_TIME = 2000  # in seconds

# Parameters and result variables
PARAMETERS = ["Q_max", "T_set"]
RESULTS = ["energy", "comfort"]

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
POP_SIZE = 5  # Population size for NSGA2
N_GEN = 5     # Number of generations

# Parallel processing
N_JOBS = -1  # Options: '-1', '1', 'n', 'None'
```

2. run `python optimize_main.py`