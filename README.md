# Tumor Modelling – Digital Twin Simulation

A 2D cellular automaton simulation of tumor growth, modelling oxygen diffusion and cell fate decisions (proliferation, hypoxia, necrosis) on a discrete lattice.

## Overview

This project implements a **digital twin** of a solid tumor, specifically calibrated to the **MCF-7 breast cancer cell line** (division rate ≈ 25 hours). The simulation couples two processes that operate on very different time scales:

- **Chemical (fast):** Oxygen diffuses through the tissue.
- **Biological (slow):** Cells divide or die based on local oxygen levels.

To bridge the gap between these time scales, each discrete hourly time step runs **1,000 oxygen-diffusion sub-iterations**, allowing the oxygen field to reach quasi-equilibrium before cells make division or necrosis decisions.

## Simulation Details

### Grid & Initialization

- A **150 × 150** lattice represents the tissue.
- Each cell site holds one of three states:
  - `0` – Empty
  - `1` – Alive / proliferating
  - `2` – Necrotic
- The simulation starts with a small 3 × 3 cluster of alive cells at the centre of the grid.
- Oxygen is initialized uniformly at the boundary concentration (220 μM) across the whole grid.

### Oxygen Diffusion

Oxygen transport is solved with the **finite-difference method** on the 2D discrete lattice:

```
O_new[i,j] = O[i,j] + D * Laplacian(O)[i,j] - λ * cell[i,j]
```

- The diffusion coefficient `D = 0.1` is chosen to satisfy the **Von Neumann stability criterion** for a 2D explicit finite-difference scheme.
- Fixed Dirichlet boundary conditions maintain the oxygen level at 220 μM on all edges, representing a well-oxygenated surrounding.
- Living cells consume oxygen at rate `λ = 0.044` μM per sub-iteration.

### Biological Rules (per hourly step)

For each alive cell, the following rules are applied in order:

| Condition | Outcome |
|-----------|---------|
| O₂ < 1.0 μM (severe hypoxia) | Cell becomes **necrotic** |
| O₂ < 10.0 μM (mild hypoxia) | Cell is **quiescent** (no division) |
| O₂ ≥ 10.0 μM | Cell **divides** with probability 0.04 into a random free neighbour (von Neumann neighbourhood) |

### Simulation Duration

- **45 days** (1,080 hourly steps) are simulated.
- Progress is printed to stdout at the end of each simulated day.

## Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `GRID_SIZE` | 150 | Lattice size (150 × 150) |
| `D_O2` | 0.1 | Scaled diffusion coefficient (Von Neumann stability) |
| `LAMBDA_O2` | 0.044 | O₂ consumption rate per alive cell per sub-iteration |
| `O2_BOUNDARY` | 220.0 μM | Boundary (and initial) O₂ concentration |
| `O2_HYPOXIA` | 10.0 μM | O₂ threshold below which cells become quiescent |
| `O2_NECROSIS` | 1.0 μM | O₂ threshold below which cells undergo necrosis |
| `PROB_DIV_BASE` | 0.04 | Base probability of division per hour (≈ 25 h doubling time) |
| `DIFUSION_STEP` | 1000 | Oxygen diffusion sub-iterations per hourly biological step |

## Repository Structure

```
tumor-modelling/
├── digital-twin.ipynb   # Main simulation notebook
├── TumorModeling.txt    # Modelling notes and design decisions
└── README.md
```

## Requirements

- Python 3.x
- [NumPy](https://numpy.org/)
- [Matplotlib](https://matplotlib.org/)

Install dependencies with:

```bash
pip install numpy matplotlib
```

## Usage

Open and run the Jupyter notebook:

```bash
jupyter notebook digital-twin.ipynb
```

Execute all cells in order. The final cell renders side-by-side plots of the **cell-state map** and the **oxygen concentration map** at the end of the simulation.

## Output

After the simulation completes you will see:
- A **cell map** colour-coded by state (empty / alive / necrotic).
- An **oxygen map** (in μM) showing the characteristic depletion zone at the tumour core.
