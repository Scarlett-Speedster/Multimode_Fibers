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

### Basic Usage
```python
import pyMMF

# Create fiber profile
profile = pyMMF.IndexProfile(n1=1.46, a=2.5e-6, alpha=2.0)

# Compute modes
solver = pyMMF.propagationModeSolver()
modes = solver.solve(profile=profile, mode='radial', wavelength=1550e-9)

# Get transmission matrix
TM = modes.newTM(length=2.0)
```

### Run Examples
- `pyMMF-master/pyMMF-master/example/RadialSolverGRIN.ipynb` - GRIN modes
- `pyMMF-master/pyMMF-master/example/Benchmark_SI.ipynb` - SI benchmarks

---

## 🔧 Available Solvers

| Solver | Type | Best For | Speed |
|--------|------|----------|-------|
| **SI** | Semi-analytical | Step-index fibers | ⚡⚡⚡ |
| **Radial** | Numerical | GRIN profiles | ⚡⚡ |
| **eig2D** | Eigenvalue | Arbitrary profiles | ⚡ |
| **WKB** | Approximation | Parabolic GRIN | ⚡⚡⚡ |

---

## 📦 Dependencies

```
numpy, scipy, matplotlib, numba, joblib
```

Optional: `jupyter`, `tensorflow`, `pytorch`

---

## 📂 pyMMF Package Structure

```
pyMMF/
├── core.py              # Main solver interface
├── modes.py             # Mode utilities
├── index_profile.py     # Fiber profiles
├── functions.py         # Helper functions
├── logger.py            # Logging
└── solvers/
    ├── SI.py            # Step-index solver
    ├── radial.py        # Radial solver
    ├── eig2D.py         # 2D eigenvalue solver
    └── WKB.py           # WKB approximation
```

