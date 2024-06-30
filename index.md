---
layout: default
title: Introduction
nav_order: 1
description: "Multi-objective Optimization framework/pipeline for Modelica."
permalink: /
---

# MOO4Modelica

An optimization framework and workflow for Modelica which supports both single- and multi-objective optimization.

GitLab Repo: [https://git-st.inf.tu-dresden.de/wang/MOO4Modelica](https://git-st.inf.tu-dresden.de/wang/MOO4Modelica)

<img src="./assets/MOO4Modelica_framework.png" alt="framework" style="zoom:80%;" />

#### Highlights:

1. **Easy to configure:** All configurations can be defined in `config.json`.
2. **SoTA algorithms:** Dynamic import of algorithms from *pymoo*.
3. **Enable use of** **parallel computing**: For accelerated process. 
5. **Support transformation into feature models**: To better analyze and understand large-scale models.

#### Structure:

```
./src/ 
(Feature Model Transformation)
	|-- feature_model
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
```

* (Feature Model Transformation)
  * `modelica.g4`: an ANTLR4 grammar for Modelica files
  * `parse_modelica.py`: parse a Modelica model to extract it components and their parameters
  * `feature_model.py`: create a feature model and add the extracted components
* (Optimization Operation)
  * `config.json` & `config.py`: global settings and configurations
  * `optimize_main.py`: main optimization script
  * `parallel_computing.py`: parallel computing
  * `optimization_libraries.py`: dynamic import of algorithms from *pymoo*