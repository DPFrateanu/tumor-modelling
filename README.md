# Tumor Modelling

This repository contains exploratory Jupyter notebooks for lattice-based tumor growth modelling, with oxygen-driven dynamics in the baseline model and a treatment-aware extension using tamoxifen.

## Table of Contents

- [Project Overview](#project-overview)
- [Repository Structure](#repository-structure)
- [Modelling Approach](#modelling-approach)
- [Notebook Guide](#notebook-guide)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
- [Reproducibility Notes](#reproducibility-notes)
- [Limitations](#limitations)
- [Next Improvements](#next-improvements)

## Project Overview

The project focuses on simulating tumor cell population behavior on a 2D grid where:

- oxygen availability modulates cell fate (proliferation, quiescence, necrosis),
- chemical transport processes evolve faster than biological decision timescales,
- treatment effects can be introduced and observed in the same lattice framework.

The implementation is notebook-centric, intended for iterative modelling, parameter exploration, and visual analysis.

## Repository Structure

```text
tumor-modelling/
├── digital-twin.ipynb  # Baseline oxygen-coupled tumor growth model
├── tamoxifen.ipynb     # Treatment-aware extension with tamoxifen effects
├── TumorModeling.txt   # Modelling notes and assumptions
└── README.md
```

## Modelling Approach

At a high level, the model combines:

1. **Discrete spatial tumour representation** on a 2D lattice.
2. **Oxygen diffusion update** using finite-difference style numerical evolution.
3. **Cell-state updates** driven by local oxygen thresholds and probabilistic rules.
4. **Separated timescales** by running many diffusion sub-iterations per biological step.

Documented assumptions in `TumorModeling.txt` include:

- division rate motivation for MCF-7-like behavior,
- use of diffusion parameter choices constrained by numerical stability,
- decoupling fast chemical dynamics from slower biological transitions.

## Notebook Guide

### `digital-twin.ipynb`

- Implements the baseline 2D tumor model (oxygen + cell dynamics).
- Uses repeated diffusion sub-iterations per biological time update.
- Applies oxygen-based criteria for quiescence/necrosis and proliferation behavior.
- Includes analysis-oriented utilities such as growth-curve style inspection.

### `tamoxifen.ipynb`

- Builds on the baseline structure by introducing treatment-related dynamics.
- Adds tamoxifen-related diffusion/effect logic and treatment timing behavior.
- Supports comparative analysis between untreated and treated progression patterns.

## Requirements

- Python 3.x
- [Jupyter](https://jupyter.org/)
- [NumPy](https://numpy.org/)
- [Matplotlib](https://matplotlib.org/)

Install dependencies:

```bash
pip install jupyter numpy matplotlib
```

## Getting Started

Launch either notebook:

```bash
jupyter notebook digital-twin.ipynb
jupyter notebook tamoxifen.ipynb
```

Recommended execution order:

1. Open a notebook.
2. Run cells top-to-bottom without skipping initialization cells.
3. Review generated plots and summary outputs after each simulation block.
4. Change one parameter group at a time for controlled experiments.

## Reproducibility Notes

- Keep Python package versions fixed during comparative runs.
- Use consistent initial conditions when benchmarking parameter changes.
- Record key simulation settings (grid size, thresholds, diffusion parameters, treatment timing) for each run.
- Prefer multiple runs per setting when using probabilistic update rules.

## Limitations

- Current implementation is notebook-first (not packaged as a reusable Python module).
- Parameter calibration and validation against experimental data are not formalized in this repository.
- Numerical and biological assumptions are exploratory and should be interpreted as research prototypes.

## Next Improvements

- Extract shared simulation logic into reusable Python modules.
- Add scripted experiment runners for batch parameter sweeps.
- Add a lightweight results schema for run metadata and output comparison.
- Add validation notebooks/tests for stability and sensitivity analysis.
