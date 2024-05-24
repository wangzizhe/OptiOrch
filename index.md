---
layout: default
title: Introduction
nav_order: 1
description: "Multi-objective Optimization framework/pipeline for Modelica."
permalink: /
---

# MOO4Modelica

Multi-objective Optimization framework for Modelica.
https://git-st.inf.tu-dresden.de/wang/MOO4Modelica

![framework](./assets/MOO4Modelica.png)

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
