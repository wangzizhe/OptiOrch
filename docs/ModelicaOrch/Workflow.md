---
layout: default
title: Workflow
parent: ModelicaOrch
nav_order: 1
---

# Dynamic Adaptation and Orchestration of Modelica Simulation

An orchestration workflow for simulating and optimizing self-adaptive systems with MOO4Modelica.  

The system dynamically adjusts its operational parameters based on available resources and specific goals, aiming to meet defined objectives.

## Components

The orchestration workflow consists of four main components, besides the Modelica model, input data, and the MOO4Modelica optimization framework:

* `orchestration_config.json`: configuration file for the entire orchestration workflow.
  * It also dynamically adapts the MOO4Modelica config file (`config.py`).
* `orchestrator.py`: Coordinates the overall workflow.
  * Initializes components, reads data, runs the optimization, and manages the simulation and evaluation loop.
* `orchestration_wrapper.py`:  Intermediary between MOO4Modelica and the orchestration workflow. 
  * Manages the optimization process using MOO4Modelica, handles optimization results, and provides parameter sets for simulation.
* `orchestration_configurator.py`: Updates configuration based on current status.
  * Prepares and sets parameters for simulation.

## Orchestration Workflow (Flowchart)
```mermaid
graph TD
    A[OptiOrch] --> B[Read Input Data]
    B --> C[Adaptive Control Loop: Iteration Over Time Units] --> D1[Orchestrator: Call Configurator and Wrapper]
    D1 --> D2[Configurator: Update Configuration for Optimization]
    D2 --> D[MOO4Modelica: Find the Best Parameter Sets]
    D --> E[Wrapper: Retrieve Optimized Parameter Sets] --> E1[Wrapper: Assign Optimized Parameter Set to the Model]
    E1 --> F[Configurator: Simulate and Evaluate Parameter Sets]
    F --> G{Goal Satisfied?}
    G --> |Yes| H[Goal Satisfied with Current Optimized Parameter Set]
    G --> |No| I[Try Next Optimized Parameter Set]
    I --> J{Any Parameter Sets Left?}
    J --> |Yes| F
    J --> |No| K[Goal Not Satisfied After Trying All Parameter Sets]
    H --> L[Add to Final Report]
    K --> L
    L --> M{All Time Units Iterated?}
    M --> |No| C
    M --> |Yes| N[Final Report]
```