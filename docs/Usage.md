---
layout: default
title: Usage
nav_order: 3
---

# Usage

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