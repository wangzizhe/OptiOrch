# MOO4Modelica

**Introduction:** Multi-objective Optimization framework/pipeline for Modelica.

**Motivation:** Since the introduction of the paper "*A Multi-objective Optimization Algorithm and Process for Modelica Model*" stated that the functions for multi-objective optimization are limited in Modelica. 

It worth to research in this direction, e.g. **Python has some good frameworks for MOO.**

**I want to design a general pipeline/framework to couple Python's MOO frameworks to Modelica using OMPython.**

Ideas for next steps (after finishing this framework): MOO takes a lot of time! Thus

* Implementation of BRISE into the framework to reduce the computing complexity, **or**
* Implementation of parallel computing

## Framework

#### Highlights:

1. **Easy to configure:** All settings and configurations can be defined in `config.py`.
2. **SoTA libraries and algorithms for MOO:** Support different libraries and algorithms.
3. **Enable using of** **parallel computing**: For accelerated process. 

#### Structure:

```
./src/ 
|-- config.py 
|-- main_optimize.py 
|-- optimization_libraries.py 
|-- parallel_computing.py
```

* `config.py`: global settings and configurations
* `optimize_main.py`: main optimization script
* `optimization_libraries`: initialization libraries and algorithms
* `parallel_computing.py`: parallel_computing

(*) This is another one-script Python file provided, which corresponds to the 4 scripts. 

#### Example

Case study with a simple heating system [SimpleHeatingSystem.mo](./SimpleHeatingSystem.mo) built in Modelica.

The idea is to heat a room, when the room heats faster, human beings feels more comfortable, but the energy consumption and cost become higher. This system should be optimized to find the **trade-off of energy consumption and comfort**.

hyperparameters (these are the parameters to be varied): 

```
  parameter Real Q_max = 5000 "Maximum heating power in watts";
  parameter Real T_set = 293.15 "Setpoint temperature in Kelvin";
```

A Python script [Optimize_SimpleHeatingSystem.py](./Optimize_EnergySystem.py) to perform hyperparameter optimization.

**Procedure:**

<u>1. Configuration of global setting for the optimization</u>

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

<u>2. Run Python script: `python optimize_main.py`</u>

You will see 

* the models have been loaded in different workers for parallel computing
* parameters have been set for different models
* simulated results of each model

like this:

```shell
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True
Load model result in worker: True

Parameters set: {'Q_max': 1889.86, 'T_set': 307.42}
Parameters set: {'Q_max': 4459.37, 'T_set': 282.12}
Parameters set: {'Q_max': 3851.85, 'T_set': 298.54}
Parameters set: {'Q_max': 2924.46, 'T_set': 280.58}
Parameters set: {'Q_max': 1375.1, 'T_set': 282.02}

Simulation results: {'energy': 1513678.09, 'cost': 151367.81, 'comfort': 1255.39}
Simulation results: {'energy': 1054423.49, 'cost': 105442.35, 'comfort': 2203.89}
Simulation results: {'energy': 220875.05, 'cost': 22087.51, 'comfort': 664.14}
Simulation results: {'energy': 1054423.49, 'cost': 105442.35, 'comfort': 2203.89}
Simulation results: {'energy': 281742.21, 'cost': 28174.22, 'comfort': 1317.24}
```

The simulation is runing:

```shell
==========================================================
n_gen  |  n_eval  | n_nds  |      eps      |   indicator
==========================================================
     1 |        5 |      2 |             - |             -
     2 |       10 |      2 |  5.7876254181 |         ideal
     3 |       15 |      3 |  0.9710601290 |         ideal
     4 |       20 |      2 |  1.1370485530 |         ideal
     5 |       25 |      2 |  0.1184009363 |         ideal
```

You will see the results at the end of simulation like this:

```shell
Optimization Results:
Solution 0: Energy = 140592.49, Cost = 14059.25, Comfort = 517.23,
Solution 1: Energy = 119249.50, Cost = 11924.95, Comfort = 555.79,
```

After that you will see the result of Pareto Front. The top left results is what we want.

![Pareto Front](./diagrams/SimpleHeatingSystem_Pareto_Front.png)

## Related Work

### I. Publications

### 1. A Multi-objective Optimization Algorithm and Process for Modelica Model

Zhang, Congcong, et al. "A Multi-objective Optimization Algorithm and Process for Modelica Model." *2022 4th International Conference on Artificial Intelligence and Advanced Manufacturing (AIAM)*. IEEE, 2022.

**Problem:** In the current commercial software based on Modelica model, there are few functions for multi-objective optimization, and the current multi-objective optimization algorithm has the problems of insufficient approximation of the optimal solution set and uneven distribution of the solution set.

**Objective:** In order to solve this problem, Combined with the characteristics of Modelica model and NSGA-II algorithm, this paper proposes a multi-objective optimization algorithm and process for Modelica model.

**Solution:** This paper provides a multi-objective optimization design process for the current modeling and simulation platform. **The process analyzes the model variables according to ANTLR4 and transform the tree structure**.

**Future Work:** MOO takes huge computing resource, so it is slow, the future work would be implement parallel computing to solve this problem.

![Overall Process](./diagrams/overall_process.JPG)

**Zizhe's thoughts:** 

* This paper only provides the idea of the overall process, I can't find it anywhere or reproduce it. Also the ANTLR method seems complicated.
* The future work part in this paper is interesting for me and the authors are not doing it. So I could think about implementing parallel computing.

#### 2. DESA - Optimization of variable structure Modelica models using custom annotations

Bender, Daniel. "DESA: Optimization of variable structure modelica models using custom annotations." *Proceedings of the 7th International Workshop on Equation-Based Object-Oriented Modeling Languages and Tools*. 2016.

**Contribution:** The library DESA uses custom annotations to implement the optimization task to the model. Further the model is exported including these meta-information. The DESA optimization tool then allows to set up the optimization task in a Matlab environment and operates the optimization run. In this way the optimization of variable structure models is achieved.

![DESA workflow](./diagrams/workflow.JPG)

**Zizhe's thoughts:** 

* This only works in Dymola...

### II. Frameworks

#### 0. pymoo: Multi-objective Optimization in Python

https://pymoo.org

The framework offers state of the art single- and multi-objective optimization algorithms and many more features related to multi-objective optimization such as visualization and decision making.

#### 1. **DEAP (Distributed Evolutionary Algorithms in Python)**

DEAP is a flexible framework for evolutionary algorithms. It provides tools for single and multi-objective optimization, making it suitable for a wide range of optimization problems.

- **Documentation**: [https://deap.readthedocs.io](https://deap.readthedocs.io/)
- **GitHub**: https://github.com/DEAP/deap

#### 2. **Platypus**

Platypus is a framework for evolutionary computing with a focus on multi-objective optimization. It supports a variety of algorithms and is designed to be user-friendly.

- **Documentation**: [https://platypus.readthedocs.io](https://platypus.readthedocs.io/)
- **GitHub**: https://github.com/Project-Platypus/Platypus

#### 3. **PyGMO (Python Global Multiobjective Optimizer)**

PyGMO is a scientific library for massively parallel optimization. It provides a wide range of optimization algorithms, including those for multi-objective optimization.

- **Documentation**: https://esa.github.io/pygmo2/
- **GitHub**: https://github.com/esa/pygmo2

#### 4. **SciPy**

SciPy is a fundamental library for scientific computing in Python, which includes several optimization routines. While it focuses more on classical optimization algorithms, it is still quite powerful for certain types of optimization problems.

- **Documentation**: https://docs.scipy.org/doc/scipy/reference/optimize.html

#### 5. **Nevergrad**

Nevergrad is a gradient-free optimization platform by Facebook AI Research. It provides a variety of algorithms suitable for optimization tasks where gradients are not available.

- **Documentation**: https://facebookresearch.github.io/nevergrad/
- **GitHub**: https://github.com/facebookresearch/nevergrad

#### 6. **Optuna**

Optuna is an automatic hyperparameter optimization software framework, particularly for machine learning, but it can be used for general optimization tasks. It supports both single-objective and multi-objective optimization.

- **Documentation**: https://optuna.readthedocs.io/en/stable/
- **GitHub**: https://github.com/optuna/optuna

#### 7. **NSGA-II (Non-dominated Sorting Genetic Algorithm II) Implementations**

While NSGA-II is available in `pymoo`, other libraries also provide implementations of this popular algorithm for multi-objective optimization:

- **jMetalPy**: https://jmetalpy.readthedocs.io/en/latest/ (focused on multi-objective optimization)
- **Inspyred**: http://aarongarrett.inspyred.github.io/ (another evolutionary computing framework)

#### 8. **CMA-ES (Covariance Matrix Adaptation Evolution Strategy)**

CMA-ES is a robust optimization algorithm suitable for difficult optimization problems. Libraries such as `pycma` provide implementations of this algorithm.

- **Documentation**: https://pypi.org/project/cma/
- **GitHub**: https://github.com/CMA-ES/pycma
