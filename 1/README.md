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
│   ├── task1_energy_phase_space.ipynb       # Classical HO energy & phase-space plots
│   └── task2_dynamics_trajectories.ipynb    # Classical HO simulation & trajectories
│
├── component2_quantum/
│   ├── task1_operators_spectrum.ipynb       # Quantum operators, Hamiltonian, eigenvalues
│   └── task2_state_dynamics_wigner.ipynb    # Schrödinger evolution & Wigner functions
│
├── component3_ml/                           # To be introduced after Components 1 & 2
│   └── .gitkeep
│
├── figures/                                 # Exported publication-quality figures
│
└── data/                                    # Generated datasets (classical & quantum)
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

---

### Component 2 — Quantum Data Generation

Calculate quantum properties analytically and numerically with QuTiP.

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

---

### Component 3 — ML Training *(Coming Soon)*

Tasks for Component 3 will be introduced after Components 1 and 2 are complete. The goal is to choose, train, and validate an ML model that maps classical data to quantum processor properties.

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
pip install qutip numpy scipy matplotlib jupyter
```

**Python version:** 3.9+

---

## Running the Notebooks

Launch Jupyter and open any notebook from the relevant component folder:

```bash
jupyter notebook
```

All notebooks are self-contained. Run cells top-to-bottom. Generated figures are saved to `figures/` and datasets to `data/`.

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
| Quantum Mechanics lecture notes (Essler, Oxford) | PDF provided by PI |
| Superconducting quantum processors overview | [PennyLane tutorial](https://pennylane.ai/qml/demos/) |

---
