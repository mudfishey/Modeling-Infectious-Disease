# Disease Compartment Modeling

This repository contains my work on compartmental disease modeling using ordinary differential equations (ODEs).

Compartmental models are a mathematical framework used to describe how populations transition between different states, or *compartments*, over time. These models are widely applied across many domains, but they are particularly important in infectious disease epidemiology.

A population is divided into compartments that represent disease states. The most common framework is the SIR model:

- **S** – Susceptible individuals
- **I** – Infectious individuals
- **R** – Recovered (or Removed) individuals

By defining transition rates between compartments, these models can be used to study disease transmission dynamics, evaluate intervention strategies, and understand epidemic behavior.

## Projects

| Project | Description |
|----------|-------------|
| [Basic ODE model](./Basic%20ODE%20model/BasicODE.md) | A foundational implementation of compartmental disease modeling using ordinary differential equations (ODEs). This project introduces the mathematical framework behind disease transmission dynamics and numerical simulation methods. |
| [Disease X](./Disease%20X/DiseaseX.md) | An application of compartmental disease modeling to a COVID-19-like outbreak scenario. This project explores disease spread dynamics, model assumptions, and simulation results under various conditions. |
