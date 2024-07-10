---
layout: default
title: Workflow
parent: ModelicaOrch
nav_order: 1
---

# Dynamic Adaptation and Orchestration of Systems with Modelica and MOO4Modelica

An orchestration workflow for simulating and optimizing self-adaptive systems with MOO4Modelica.  

The system dynamically adjusts its operational parameters based on available resources and specific goals, aiming to meet defined objectives.

## Components

The orchestration workflow consists of four main components, besides the Modelica model, input data, and the MOO4Modelica optimization framework:

* `orchestration_config.json`: configuration file for the entire orchestration workflow.
  * It also dynamically adapts the MOO4Modelica config file (`config.py`).
* `orchestrator.py`: Coordinates the overall workflow.
  * Initializes components, reads data, runs the optimization, and manages the simulation and evaluation loop.
* `orchestration_wrapper.py`:  Intermediary between MOO4Modelica and the orchestration workflow. 
  * Manages the optimization process using MOO4Modelica, handles optimization results and provides parameter sets for simulation.
* `orchestration_configurator.py`: Updates configuration based on current status.
  * Prepares and sets parameters for simulation.

## Orchestration Workflow (Flowchart)
```mermaid
graph TD
    A[Start] --> B[Read Input Data]
    B --> C[Iterate Over Time Units]
    C --> D[Update Configuration: Based on the read data] --> E1[Orchestrator: Call Configurator and Wrapper]
    E1 --> E2[Configurator: Update Configuration for Optimization]
    E2 --> E[Run Optimization: Using MOO4Modelica to find the best parameters]
    E --> F[Get Parameter Sets: Retrieve optimized parameter sets from results] --> F1[Wrapper: Manage Optimization Results]
    F1 --> G[Simulate and Evaluate Parameters: Model simulation with parameter sets to check whether goals are satisfied]
    G --> H{Goal Satisfied?}
    H --> |Yes| I[Goal Satisfied with Corresponding Parameters]
    H --> |No| J[Try Next Parameter Set: Repeat Simulation and Evaluation]
    J --> K{Any Parameter Sets Left?}
    K --> |Yes| G
    K --> |No| L[Goal Not Satisfied After trying all parameter sets]
    I --> M[Add to Final Report]
    L --> M
    M --> N{All Time Units Iterated?}
    N --> |No| D
    N --> |Yes| O[End]
```