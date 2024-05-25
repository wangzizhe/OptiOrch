---
layout: default
title: Introduction
nav_order: 1
description: "Multi-objective Optimization framework/pipeline for Modelica."
permalink: /
---

# MOO4Modelica

Multi-objective Optimization framework for Modelica.

GitLab Repo: [https://git-st.inf.tu-dresden.de/wang/MOO4Modelica](https://git-st.inf.tu-dresden.de/wang/MOO4Modelica)

<img src="./assets/MOO4Modelica_pipeline.png" alt="framework" style="zoom:80%;" />

#### Highlights:

1. **Easy to configure:** All settings and configurations can be defined in `config.py`.
2. **SoTA libraries and algorithms for MOO:** Support different libraries and algorithms.
3. **Enable using of** **parallel computing**: For accelerated process. 
4. **Comprehensive debugging system**: Debugging functions for all critical steps.

#### Structure:

```
./src/ 
|-- config.py 
|-- main_optimize.py 
|-- parallel_computing.py
|-- optimization_libraries.py 
```

* `config.py`: global settings and configurations
* `optimize_main.py`: main optimization script
* `parallel_computing.py`: parallel_computing
* `optimization_libraries`: initialization libraries and algorithms







