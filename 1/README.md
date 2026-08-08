## Project Overview

The overarching goal of this research is to **predict properties of quantum processors using classical data**. The project bridges classical mechanics, quantum physics, and machine learning across three sequential research components.

```
Classical World ──► Neural Network ──► Quantum World
  (Phase Space)                        (Processor Properties)
```

---

## Repository Structure

```
ai-quantum-processors/
│
├── README.md
│
├── component1_classical/
│   ├── task1_energy_phase_space.ipynb            # Classical HO energy & phase-space plots
│   ├── task2_dynamics_trajectories.ipynb         # Classical HO simulation & trajectories
│   ├── task3_cosine_potential.ipynb              # Nonlinear oscillator: cosine potential
│   └── task4_coupled_oscillators.ipynb           # Two coupled oscillators & Poincaré maps
│
├── component2_quantum/
│   ├── task1_operators_spectrum.ipynb            # Quantum operators, Hamiltonian, eigenvalues
│   ├── task2_state_dynamics_wigner.ipynb         # Schrödinger evolution & Wigner functions
│   └── task3_fluxonium_dynamics.ipynb            # Fluxonium wave packets & phase-charge dynamics
│
├── component3_ml/
│   └── task1_classical_to_quantum_regression.ipynb   # Fluxonium classical→quantum MLP regression
│
├── figures/                                      # Exported publication-quality figures
│
└── data/                                         # Generated datasets (classical & quantum)
```

---

## Research Components

### Component 1 — Classical Data Generation

Simulate classical mechanical systems using Newton's equations of motion.

#### Task 1: Classical Harmonic Oscillator — Energy and Phase Space
- Derive the classical energy function `E(x, p)` for a 1D harmonic oscillator
- Derive equations of motion from Hamilton's equations
- Produce a contour/density plot of `E(x, p)` over a 2D grid; explain constant-energy curves

#### Task 2: Classical Harmonic Oscillator — Dynamics and Trajectories
- Solve the equations of motion analytically; plot a single phase-space trajectory `(x(t), p(t))`
- Simulate an ensemble of random initial conditions; overlay all trajectories and discuss energy dependence

#### Task 3: Nonlinear Oscillator — Cosine Potential and Phase Space
- Add a cosine term to the harmonic potential; derive the modified equations of motion
- Simulate trajectories across several initial energies; overlay bands of trajectories at fixed energy, encoded by color

#### Task 4: Two Coupled Oscillators — Momentum Coupling and Poincaré Maps
- Derive equations of motion for two momentum-coupled cosine oscillators
- Plot the four natural 2D phase-space projections across a range of energies
- Construct Poincaré maps and discuss regular vs. chaotic structure

---

### Component 2 — Quantum Data Generation

Calculate quantum properties analytically and numerically with QuTiP and scqubits.

#### Task 1: Quantum Harmonic Oscillator — Operators and Energy Spectrum
- Construct truncated matrix representations of `x̂`, `p̂`, and `Ĥ` using QuTiP ladder operators
- Visualize matrix elements with `imshow` plots
- Compute eigenvalues numerically; compare to analytic result `Eₙ = ℏω(n + ½)`
- Reflect on what changes (and what stays the same) moving from classical `E(x,p)` to quantum `Ĥ`

#### Task 2: Quantum Harmonic Oscillator — State Dynamics and Phase Space
- Use `sesolve` to evolve three initial states:
  - Energy eigenstate `|n⟩` (e.g., `|0⟩` or `|1⟩`)
  - Normalized superposition `(|0⟩ + |1⟩) / √2`
  - Displaced Gaussian / coherent state `|α⟩`
- Compute and plot the **Wigner function** `W(x, p)` at `t = 0` for each state
- Animate Wigner function dynamics (fixed color scale across frames)
- Plot expectation-value trajectories `⟨p̂⟩` vs `⟨x̂⟩`; overlay classical solution with matching initial conditions
- Write a short reflection (≤ 1 page) comparing all representations: energy contours, matrix elements, eigenvalues, Wigner functions, and expectation-value plots

#### Task 3: Fluxonium — Wave Packets and Phase–Charge Dynamics
- Build the fluxonium Hamiltonian in `scqubits` (charging, inductive, Josephson terms)
- Construct a localized wave packet centered at a chosen phase `φ₀`; evolve with `sesolve`
- Compare expectation-value trajectories `⟨φ̂⟩(t)`, `⟨n̂⟩(t)` against the mapped classical trajectory
- Repeat across several `φ₀` and summarize as grid plots

---

### Component 3 — ML Training

Choose, train, and validate an ML model that maps classical fluxonium data to quantum trajectory data.

#### Task 1: Fluxonium — Classical-to-Quantum Regression
- **Shared parameter module:** single source of truth for `E_J, E_C, E_L, φ_ext`, deriving the mapped classical constants (`m, ω, V₀, k`) used by both simulators
- **Paired sampling:** `N_s` random wave-packet centers `(φ₀, n₀)`, mapped to classical `(x₀, p₀) = (φ₀ − φ_ext, n₀)`
- **Time grid:** `t_final` derived from the classical period `T = 2π/ω` (used `t_final = 2T`) so both trajectories complete at least one full cycle
- **Quantum dataset:** fluxonium Hamiltonian + localized wave packet (built via a phi-operator discrete-variable-representation) evolved with `sesolve`; flattened to target vectors `B ∈ ℝ^(2Nₜ)`
- **Classical dataset:** mapped nonlinear-oscillator equations of motion integrated on the same initial conditions and time grid; flattened to input vectors `A ∈ ℝ^(2Nₜ)`
- **Model:** 2-hidden-layer MLP (PyTorch), ReLU activations, MSE loss, Adam optimizer, 80/20 train/val split, train-set-only normalization
- **Hyperparameter sweep:** compared hidden-layer widths, learning rate, and batch size across 8 configurations to select the best-generalizing setup
- **Results (Nₛ = 300):** validation MSE ≈ 0.01 for the best configuration (down from ≈0.15–0.19 at Nₛ = 60 without a sweep); component-wise breakdown shows the `φ` (phase) trajectory is harder to predict than `n` (charge)
- **Open questions:** whether validation MSE keeps improving past Nₛ = 300, whether the wave-packet construction's approximate charge centering biases the dataset, and whether the `φ`/`n` error asymmetry warrants a weighted loss

---

## Setup & Installation

```bash
# Clone the repository
git clone https://github.com/<org>/ai-quantum-processors.git
cd ai-quantum-processors

# Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate        # Linux/macOS
# venv\Scripts\activate         # Windows

# Install dependencies
pip install qutip scqubits torch numpy scipy matplotlib jupyter
```

**Python version:** 3.9+

---

## Running the Notebooks

Launch Jupyter and open any notebook from the relevant component folder:

```bash
jupyter notebook
```

All notebooks are self-contained. Run cells top-to-bottom (a full "Restart & Run All" is verified clean before every push). Generated figures are saved to `figures/` and datasets to `data/`.

> **Note:** Component 3's notebook trains a neural network and runs a hyperparameter sweep — expect a runtime of a couple of minutes on a full run, versus a few seconds for Components 1 and 2.

---

## Coding Standards

- **Readable code:** descriptive variable names, short focused functions, one-line comment per non-obvious step
- **Notebooks only:** all code lives in `.ipynb` files
- **GitHub hygiene:** push regularly; keep the PI (@mondragon-shem) added as a collaborator

## Plot Standards

- Every axis labeled with units (or explicitly marked dimensionless)
- Every curve has a legend entry; every figure has a caption with a key takeaway
- Perceptually uniform colormaps (`viridis`, `plasma`) throughout
- Font sizes legible at slide and print scale

## Meeting Slide Structure

Each weekly update slide deck should follow:
1. **Context** — why was this calculation done and what question does it answer?
2. **Results** — findings supported by figures with annotations
3. **Open Questions** — what remains unclear and what comes next?

*(One idea per slide; main takeaway stated as a complete sentence in the slide title.)*

---

## References & Resources

| Resource | Link |
|---|---|
| QuTiP documentation | [qutip.org](https://qutip.org) |
| scqubits documentation | [scqubits.readthedocs.io](https://scqubits.readthedocs.io) |
| PyTorch documentation | [pytorch.org](https://pytorch.org) |
| Quantum Mechanics lecture notes (Essler, Oxford) | PDF provided by PI |
| Superconducting quantum processors overview | [PennyLane tutorial](https://pennylane.ai/qml/demos/) |

---
