# Multimode Fibers Repository

Optical fiber simulation and reservoir computing research using the pyMMF package.

## 📁 Directory Overview

| Folder | Purpose |
|--------|---------|
| `pyMMF-master/pyMMF-master/` | Core simulation package with 4 solvers (SI, Radial, eig2D, WKB) |
| `source/` | Research notebooks on ML/deep learning applications |
| `Images/` | Experimental results & visualizations (~60 images) |
| `Trial/` | Mode analysis & neural network experiments (~30 images) |

---

## ⚡ Quick Start

### Install pyMMF
```bash
cd pyMMF-master/pyMMF-master
pip install -e .
```

# Multimode Fibers — Quick Reference

Lightweight overview and quick start for the multimode fiber simulation and research workspace.

## What's new

- Added project artifacts and deployment helpers: `predict.py`, `cog.yaml`, `build/`, `dist/`, and `pyMMF.log` files.
- Top-level Research_Project.pdf is included for offline reading of the research report.

## 📁 Directory snapshot

| Path | Notes |
|------|-------|
| `pyMMF-master/pyMMF-master/` | Core package (pyMMF), examples, build artifacts (`build/`, `dist/`) and `predict.py` (Cog interface) |
| `source/` | Research Jupyter notebooks and a project log (`pyMMF.log`) |
| `Images/` | Visualizations and experimental outputs (~60 images) |
| `Trial/` | Experimental mode analyses and NN experiment figures (~30 images) |
| `Research_Project.pdf` | Research write-up / report |

## ⚡ Quick start

Install the package in editable mode and run the notebook examples:

```bash
cd pyMMF-master/pyMMF-master
pip install -e .
# then open the example notebooks in `example/`
```

Basic interactive use (python):

```python
import pyMMF
profile = pyMMF.IndexProfile(n1=1.46, a=2.5e-6, alpha=2.0)
solver = pyMMF.propagationModeSolver()
modes = solver.solve(profile=profile, mode='radial', wavelength=1550e-9)
TM = modes.newTM(length=2.0)
```

## ⚙️ Notable files and folders

- `pyMMF-master/pyMMF-master/predict.py` — prediction/deployment interface (Cog)
- `pyMMF-master/pyMMF-master/cog.yaml` — deployment metadata for `predict.py`
- `pyMMF-master/pyMMF-master/build/`, `dist/`, `pyMMF.egg-info/` — build/distribution artifacts
- `pyMMF-master/pyMMF-master/pyMMF.log` and `source/pyMMF.log` — runtime/log files
- `pyMMF-master/pyMMF-master/setup.py`, `LICENSE` — packaging and license

## 📂 Source files (new)

The `source/` directory now contains the following files:

- `source/Uniform_Distribution.ipynb` — notebook demonstrating uniform distribution sampling and analysis
- `source/Research Project (1).ipynb` — primary research notebook for experiments and results
- `source/20240115_Research Project (1)_DP.ipynb` — dated project draft with additional analysis
- `source/pyMMF.log` — runtime log capturing execution details and warnings

## Available solvers (high level)

| Solver | Best for |
|--------|----------|
| SI     | Ideal step-index fibers (fast, semi-analytical) |
| Radial | Axisymmetric / GRIN profiles (fast numeric) |
| eig2D  | Arbitrary index profiles and bent fibers (flexible) |
| WKB    | Approximate estimates for parabolic GRIN |

## Dependencies

Required: numpy, scipy, matplotlib, numba, joblib

Optional: jupyter, tensorflow or pytorch for ML experiments

## Where to look next

- Examples: `pyMMF-master/pyMMF-master/example/` (open the notebooks)
- Core code: `pyMMF-master/pyMMF-master/pyMMF/` (core.py, index_profile.py, solvers/)
- Experiments and figures: `Images/` and `Trial/`
- Research document: `Research_Project.pdf`

---

**Last updated**: February 2026

