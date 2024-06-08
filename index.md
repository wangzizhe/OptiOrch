---
layout: default
title: Introduction
nav_order: 1
description: "Multi-objective Optimization framework/pipeline for Modelica."
permalink: /
---

# MOO4Modelica

A multi-objective Optimization framework and workflow for Modelica.

GitLab Repo: [https://git-st.inf.tu-dresden.de/wang/MOO4Modelica](https://git-st.inf.tu-dresden.de/wang/MOO4Modelica)

<img src="./assets/MOO4Modelica_framework.png" alt="framework" style="zoom:80%;" />

#### Highlights:

1. **Easy to configure:** All settings and configurations can be defined in `config.json`.
2. **SoTA algorithms for MOO:** Dynamic import of algorithms from *pymoo*.
3. **Enable use of** **parallel computing**: For accelerated process. 
4. **Enable use of adaptive instance selection:** Automated search space reduction.
5. **Support transformation into feature models**: To better analyze and understand large-scale models.
6. **Comprehensive debugging system**: Debugging functions for all critical steps.

#### Structure:

```
./src/ 
(Feature Model Transformation)
	|-- modelica.g4
	|-- parse_modelica.py
	|-- feature_model.py
(Optimization Operation)
	|-- config.json
	|-- config.py
	|-- optimize_main.py 
	|-- parallel_computing.py
	|-- adaptive_instance_selection.py 
	|-- optimization_libraries.py 
	|-- evaluate.py
```

* (Feature Model Transformation)
  * `modelica.g4`: an ANTLR4 grammar for Modelica files
  * `parse_modelica.py`: parse a Modelica model to extract it components and their parameters
  * `feature_model.py`: create a feature model and add the extracted components
* (Optimization Operation)
  * `config.json` & `config.py`: global settings and configurations
  * `optimize_main.py`: main optimization script
  * `parallel_computing.py`: parallel computing
  * `adaptive_instance_selection.py`: automated search space reduction
  * `optimization_libraries.py`: dynamic import of algorithms from *pymoo*
  * `evaluate.py`: performance evaluation (time efficiency, optimization accuracy, additional statistical analysis)