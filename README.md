# Multimode Fibers: Mode Analysis and Optical Fiber Simulation

A comprehensive research project investigating mode propagation in multimode optical fibers using numerical simulation and machine learning techniques.

## 📋 Project Overview

This repository contains research on **multimode optical fiber (MMF) mode analysis and propagation** using the pyMMF simulation package. The project explores:

- **Mode behavior analysis** in step-index and graded-index fibers
- **Modal superposition** and field distributions
- **Statistical modal weighting** using uniform and normal distributions
- **Numerical mode solving** using multiple solvers (SI, Radial, eig2D, WKB)
- **Fiber characterization** for various wavelengths and parameters
- **Machine learning applications** for mode prediction and fiber classification using complex-valued neural networks
- **Fidelity and correlation analysis** of predicted vs actual mode patterns
- **Deep learning reconstruction** of multimode fiber outputs

### Research Objectives

1. Develop and validate efficient numerical methods for mode solving in multimode fibers
2. Analyze modal field patterns and their propagation characteristics
3. Create tools for fiber design and optimization
4. Investigate neural network approaches for mode prediction

### Key Achievements

✅ Implemented four different mode solving algorithms with varying computational complexity and accuracy  
✅ Generated comprehensive modal field visualizations and superposition studies  
✅ Analyzed modal excitation patterns using uniform and normal distribution weighting  
✅ Developed correlation and fidelity metrics for modal ensemble comparisons  
✅ **Developed complex-valued neural network for mode prediction** with PyTorch  
✅ Created reproducible analysis notebooks with systematic parameter variation  
✅ Compiled 90+ experimental results and visualizations  
✅ Developed deployment-ready prediction interface with Cog  

---

## 🗂️ Repository Structure

```
.
├── README.md                          # This file
├── Research_Project.pdf               # Complete research report
│
├── notebooks/                         # Jupyter notebooks for research and analysis
│   ├── examples/                      # Example notebooks (from pyMMF-master)
│   │   ├── Benchmark_SI.ipynb        # Step-index solver benchmarks
│   │   └── RadialSolverGRIN.ipynb    # Radial solver for GRIN profiles
│   │
│   └── research/                      # Research analysis notebooks
│       ├── Research_Project_Main.ipynb           # Primary research analysis
│       ├── Research_Project_Archive.ipynb        # Archive version with additional analysis
│       └── Distribution_Analysis.ipynb           # Statistical distribution analysis
│
├── pyMMF/                             # Core pyMMF simulation package
│   ├── pyMMF/                         # Package source code
│   │   ├── __init__.py
│   │   ├── core.py                   # Core functionality
│   │   ├── modes.py                  # Mode solving classes
│   │   ├── index_profile.py           # Fiber profile definitions
│   │   ├── functions.py               # Utility functions
│   │   ├── logger.py                  # Logging utilities
│   │   └── solvers/                   # Numerical solvers
│   │       ├── SI.py                  # Step-index solver
│   │       ├── Radial.py              # Radial/GRIN solver
│   │       ├── eig2D.py               # 2D eigenvalue solver
│   │       └── WKB.py                 # WKB approximation solver
│   │
│   ├── setup.py                       # Package installation setup
│   ├── LICENSE                        # Package license
│   ├── predict.py                     # Deployment interface (Cog)
│   ├── cog.yaml                       # Cog deployment configuration
│   └── example/                       # Example usage scripts
│
├── results/                           # Experimental results and outputs
│   ├── visualizations/                # Figure outputs (~90 images)
│   │   ├── mode_profiles/             # Modal field visualizations
│   │   ├── experiments/               # Experimental results
│   │   └── analysis/                  # Analysis plots
│   │
│   └── logs/                          # Execution logs
│       └── pyMMF.log                  # Runtime log
│
└── .gitignore                         # Git ignore rules
```

---

## ⚡ Quick Start

### 1. Clone and Install

```bash
# Clone the repository
git clone <repository-url>
cd Multimode_Fibers

# Install pyMMF in editable mode
cd pyMMF
pip install -e .
cd ..
```

### 2. Run Examples

```bash
# Using Jupyter
jupyter notebook notebooks/examples/Benchmark_SI.ipynb

# Or open the research notebooks
jupyter notebook notebooks/research/Research_Project_Main.ipynb
```

### 3. Basic Usage

```python
import pyMMF
import numpy as np
import matplotlib.pyplot as plt

# Define fiber parameters
n1 = 1.45                    # core refractive index
radius = 2.0                 # core radius in microns
NA = 0.275                   # numerical aperture
wavelength = 0.6328          # wavelength in microns

# Create index profile
profile = pyMMF.IndexProfile(npoints=128, areaSize=5*radius)
profile.initStepIndex(n1=n1, a=radius, NA=NA)

# Solve modes
solver = pyMMF.propagationModeSolver()
solver.setIndexProfile(profile)
solver.setWL(wavelength)
modes = solver.solve(mode='SI')

# Create modal superposition with uniform distribution
weights = np.random.uniform(0, 1, size=modes.number_of_modes)
superposition = np.sum(weights[i] * modes.profiles[i] 
                       for i in range(modes.number_of_modes))

# Visualize
plt.imshow(np.abs(superposition))
plt.colorbar()
plt.show()
```

---

## 📊 Statistical Modal Weighting Analysis

A key aspect of this research involves analyzing **modal excitation patterns** using different probability distributions:

### Uniform Distribution
- **Principle**: Each mode has equal probability of excitation
- **Application**: Creates balanced superpositions across all modes
- **Use Case**: Baseline comparison, studying ensemble average behavior
- **Mathematical Form**: $w_i \sim \text{Uniform}(0, 1)$

```python
# Example: Uniform modal weighting
amplitudes = np.random.uniform(0, 1, size=modes.number_of_modes)
phases = np.random.uniform(0, 2*np.pi, size=modes.number_of_modes)
weights = amplitudes * np.exp(1j * phases)

superposition = np.sum(weights[i] * modes.profiles[i] 
                       for i in range(modes.number_of_modes))
```

### Normal Distribution
- **Principle**: Mode excitation follows Gaussian distribution (most energy in lower modes)
- **Application**: Models realistic input where fundamental modes dominate
- **Use Case**: Studying energy concentration, practical fiber communication scenarios
- **Mathematical Form**: $w_i \sim \mathcal{N}(\mu, \sigma^2)$

```python
# Example: Normal distribution modal weighting
amplitudes = np.random.normal(0.5, 0.15, size=modes.number_of_modes)
amplitudes = np.abs(amplitudes)  # Ensure positive
phases = np.random.uniform(0, 2*np.pi, size=modes.number_of_modes)
weights = amplitudes * np.exp(1j * phases)

superposition = np.sum(weights[i] * modes.profiles[i] 
                       for i in range(modes.number_of_modes))
```

### Analysis Metrics
The research examines:
- **Correlation**: How similar field patterns are across different weighting schemes
- **Fidelity**: How well machine learning models predict weighted superpositions
- **Mode Power Distribution**: Energy concentration in low vs high-order modes

### Research Notebooks
- **Distribution_Analysis.ipynb**: Detailed statistical analysis of uniform vs normal distributions
- **Research_Project_Main.ipynb**: Comparative study of modal superpositions and ML predictions

---

## 🔧 Available Solvers

| Solver | Best For | Complexity | Speed |
|--------|----------|-----------|-------|
| **SI** | Step-index fibers | Semi-analytical | Fast ⚡ |
| **Radial** | Axisymmetric GRIN profiles | Numerical (1D) | Very Fast ⚡⚡ |
| **eig2D** | Arbitrary profiles, bent fibers | Full 2D numerical | Slower 🐢 |
| **WKB** | Parabolic GRIN approximation | Approximation | Fastest ⚡⚡⚡ |

---

## 📦 Dependencies

### Required
- Python 3.7+
- numpy
- scipy
- matplotlib
- numba (for fast computation)
- joblib (for parallel processing)

### Optional
- jupyter (for notebook support)
- tensorflow or pytorch (for ML experiments)

### Installation

```bash
# Install required dependencies
pip install numpy scipy matplotlib numba joblib

# For notebook support
pip install jupyter

# For ML experiments (optional)
pip install tensorflow  # or pytorch
```

---

## 📊 Project Contents

### Notebooks

- **Research_Project_Main.ipynb**: Primary research analysis including mode solving, visualization, and superposition studies
- **Research_Project_Archive.ipynb**: Extended analysis with additional experiments and baseline comparisons
- **Distribution_Analysis.ipynb**: Statistical analysis of uniform and normal distributions for modal weighting
- **Benchmark_SI.ipynb**: Performance benchmarks for step-index solvers
- **RadialSolverGRIN.ipynb**: Detailed radial/GRIN solver demonstrations

### Results

- **visualizations/mode_profiles/**: Modal field distributions for different parameters
- **visualizations/experiments/**: Experimental results and comparisons
- **visualizations/analysis/**: Analysis plots and statistical visualizations
- **logs/**: Execution logs and computational benchmarks

---

## 🎯 Research Workflow

1. **Setup**: Define fiber parameters (radius, NA, wavelength, profile type)
2. **Profiling**: Create refractive index profile
3. **Solving**: Choose appropriate solver and compute modes
4. **Analysis**: Extract modal properties (effective indices, field profiles)
5. **Visualization**: Generate plots and analysis figures
6. **Superposition**: Create custom mode combinations
7. **Validation**: Compare with theoretical predictions

---

## 🧠 Machine Learning Component

The project includes a **complex-valued neural network (ComplexNet)** for predicting multimode fiber modes from field data using PyTorch.

### What It Does
- **Predicts** modal weight composition from complex-valued field data
- **Reconstructs** MMF output field distributions
- **Learns** the relationship between input fields and modal content
- **Evaluates** using correlation and fidelity metrics

### Architecture
```
Input: 128×128 Complex Field
           │
    ComplexLinear Layer
    (16,384 → 5 complex values)
           │
Output: 5 Mode Weights
    ↓ (with pyMMF modes)
Reconstructed Field
    │
    ├─→ Amplitude
    ├─→ Phase  
    └─→ Intensity
```

### Key Features
- ✅ **Complex-Valued**: Handles complex numbers natively via complexPyTorch
- ✅ **PyTorch-Based**: GPU-compatible training with Adam optimizer
- ✅ **Multi-Metric Evaluation**: Correlation and fidelity-based assessment
- ✅ **Integrated with pyMMF**: Uses actual modes as basis functions

### Implementation Details
- **Framework**: PyTorch + complexPyTorch
- **Training**: 200 epochs, Adam optimizer (lr=0.01), batch size 32
- **Loss Function**: MSE on complex numbers (real + imaginary components)
- **Data Split**: 72% train, 20% test, 8% validation
- **Outputs**: 5 complex-valued mode weights

### Training Metrics
```python
# Correlation: Amplitude-phase matched similarity
correlation = np.corrcoef(predicted_field, ground_truth_field)

# Fidelity: Quantum-inspired inner product measure
fidelity = |⟨ψ_pred|ψ_gt⟩|² / (||ψ_pred|| × ||ψ_gt||)

# Loss: Mean squared error across real and imaginary parts
mse = mean((pred_real - gt_real)² + (pred_imag - gt_imag)²)
```

### Example Usage

```python
import torch
from source.copy_of_nn import ComplexNet

# Initialize model
model = ComplexNet(output_modes=5)
model = model.to(device)

# Load trained weights (if available)
# model.load_state_dict(torch.load('trained_model.pth'))

# Predict modes from field data
model.eval()
with torch.no_grad():
    field_input = torch.tensor(input_field, dtype=torch.complex64)
    mode_weights = model(field_input)  # 5 complex coefficients

# Reconstruct using pyMMF modes
superposition = sum(
    mode_weights[i] * modes.profiles[i] 
    for i in range(5)
)

# Visualize
intensity = np.abs(superposition)**2
plt.imshow(intensity.reshape(128, 128))
plt.show()
```

### Files
- **source/copy_of_nn.py** (594 lines): Complete neural network implementation
  - Model definition and training loop
  - Evaluation and correlation analysis
  - Fidelity calculations
  - Integration with pyMMF

### Dependencies (for ML)
```bash
pip install torch                    # PyTorch
pip install complexPyTorch           # Complex-valued operations
pip install seaborn                  # Distribution visualization
```

### Performance Metrics
The model is evaluated using:
- **Correlation coefficient** ($-1$ to $+1$): Field similarity
- **Fidelity** ($0$ to $1$): Quantum-inspired exact match measure
- **Test loss**: Mean squared error on held-out test set

### Results
Training produces:
- Loss curves (training vs validation)
- Correlation/fidelity distributions
- Mode prediction comparisons
- 90+ evaluation visualizations

---

## 🚀 Deployment

The `predict.py` file provides a deployment interface compatible with Cog for cloud deployment:

```bash
# With Cog installed
cog predict -i parameters="..." -i wavelength=1550e-9
```

Configure deployment via `cog.yaml`.

---

## 📖 Documentation

- **Research_Project.pdf**: Comprehensive research report with methodology, results, and analysis
- **pyMMF documentation**: See `pyMMF/` folder and inline docstrings
- **Example notebooks**: Start with `notebooks/examples/`

---

## 🔗 References

This project uses the **pyMMF** package:
- Source: `pyMMF/pyMMF/` directory
- Core modules: `core.py`, `modes.py`, `index_profile.py`
- Solvers: `solvers/SI.py`, `solvers/Radial.py`, `solvers/eig2D.py`, `solvers/WKB.py`

---

## 📝 Reproducibility

To reproduce results:

1. Install dependencies as described above
2. Run notebooks in order:
   - Start with `notebooks/examples/Benchmark_SI.ipynb`
   - Then `notebooks/research/Research_Project_Main.ipynb`
3. Outputs will be saved to `results/`
4. Compare generated visualizations with `results/visualizations/`

---

## 💡 Getting Help

- Check the research notebooks in `notebooks/research/` for detailed examples
- Review docstrings in `pyMMF/` source files
- See `Research_Project.pdf` for theoretical background
- Check `results/logs/pyMMF.log` for execution details

---
