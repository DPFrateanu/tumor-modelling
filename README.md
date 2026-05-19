# Tumor Modelling

This repository contains Jupyter notebooks for lattice-based tumor growth simulations driven by oxygen diffusion, plus a treatment extension with tamoxifen.

## Current Project Composition

```
tumor-modelling/
├── digital-twin.ipynb  # Baseline digital-twin tumor model (oxygen + cell dynamics)
├── tamoxifen.ipynb     # Treatment-aware model with tamoxifen diffusion/effect
├── TumorModeling.txt   # Modelling notes and assumptions
└── README.md
```

## Notebooks

### `digital-twin.ipynb`
- 2D cellular automaton tumor model on a `150 x 150` grid.
- Couples fast oxygen diffusion with slower biological updates.
- Uses oxygen thresholds for quiescence/necrosis and probabilistic proliferation.
- Includes additional analysis helpers (e.g., 2D-to-3D conversion and Gompertz-style fitting).

### `tamoxifen.ipynb`
- Extends the baseline approach with tamoxifen diffusion and treatment timing.
- Simulates treatment-driven modulation of proliferation dynamics.

## Requirements

- Python 3.x
- [Jupyter](https://jupyter.org/)
- [NumPy](https://numpy.org/)
- [Matplotlib](https://matplotlib.org/)

Install dependencies:

```bash
pip install jupyter numpy matplotlib
```

## Usage

Run either notebook in Jupyter:

```bash
jupyter notebook digital-twin.ipynb
jupyter notebook tamoxifen.ipynb
```

Execute cells in order to reproduce each simulation workflow and plots.
