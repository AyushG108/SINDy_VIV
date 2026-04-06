# SINDy_VIV

Sparse Identification of Nonlinear Dynamics (SINDy) is used in this project to discover governing equations for vortex-induced vibration (VIV) systems. This repository combines **classical reduced-order physics-based models** with **data-driven system identification**.

---

## Overview

This work focuses on two canonical VIV reduced-order models:

* **Wake Oscillator Model** (Facchinetti et al., 2004)
* **Lift Oscillator Model** (Hartlen & Currie, 1970)

The pipeline is:

1. Solve governing equations using numerical integration
2. Generate time-series data (wake variable `q`, lift coefficient `C_l`)
3. Construct nonlinear feature libraries
4. Apply SINDy to discover governing equations

---

## ⚙️ Governing Equations

### 1. Wake Oscillator Model (Facchinetti et al.)

The coupled fluid–structure system is given by:

[
\ddot{y} + 2\zeta \omega_n \dot{y} + \omega_n^2 y = \frac{F}{m}
]

[
\ddot{q} + \epsilon (q^2 - 1) \dot{q} + \omega_s^2 q = A \ddot{y}
]

where:

* (y): structural displacement
* (q): wake variable
* (\zeta): damping ratio
* (\omega_n): natural frequency
* (\epsilon): nonlinear wake parameter

---

### 2. Lift Oscillator Model (Hartlen & Currie)

The system is:

[
\dot{x} = v
]

[
\dot{v} = 2 a \omega^2 C_l - x - 2\zeta v
]

[
\dot{C_l} = p
]

[
\dot{p} = f(C_l, p)
]

where (f(C_l, p)) is a nonlinear polynomial representation of lift dynamics.

---

## SINDy Formulation

The identified system is expressed as:

[
\dot{q} = u
]
[
\dot{u} = f(q, \dot{q})
]

A candidate library includes:

* Polynomial terms: (q, q^2, q^3)
* Cross terms: (q \dot{q})
* Higher-order derivatives: (\dot{q}^2, \dot{q}^3)

Sparse regression selects only the dominant terms.

---

## 🔍 Discovered Models (SINDy Results)

### 1. Wake Oscillator (from simulated data)

Identified dynamics:

[
\dot{q} = u
]

[
\dot{u} = 0.074

* 0.400 q
* 29.462 u

- 0.018 q^2

* 0.005 q u
* 0.017 u^2
* 0.002 q^3
* 0.089 q^2 u

- 0.397 q u^2

* 0.025 u^3
  ]

---

### 2. Lift Oscillator (from CFD data)

Identified lift dynamics:

[
\dot{C_l} = p
]

[
\dot{p} = 0.169 + 0.119 C_l + 1.536 p

* 0.088 C_l^2 - 0.021 C_l p - 0.064 p^2
* 0.180 C_l^3 - 0.769 C_l^2 p - 0.180 C_l p^2
* 0.766 p^3
  ]

---

## CFD-Based Modeling

* Lift coefficient (C_l) is obtained from CFD of a static cylinder
* Time-series data is used directly for system identification
* No explicit governing equations are assumed

This highlights:

➡️ Physics-based modeling (wake oscillator)
➡️ Data-driven discovery (CFD + SINDy)

---

## Physical Model

![1DOF elastically supported cylinder undergoing VIV](Model-of-1DOF-elastically-supported-rigid-structure-experiencing-VIV.png)

---

## Repository Structure

* `SINDy_wake_oscillator.ipynb` → Wake oscillator + SINDy identification
* `SINDy_lift_oscillator.ipynb` → Lift oscillator + SINDy identification
* `wake_oscillator_model.ipynb` → Base wake model
* `lift_oscillator.ipynb` → Base lift model
* `CFD_lift_drag_time_series_*` → CFD datasets

---

## Requirements

* Python 3.x
* NumPy
* SciPy
* Matplotlib
* PySINDy (optional but recommended)

```bash
pip install numpy scipy matplotlib pysindy
```

---

## How to Run

```bash
git clone https://github.com/<your-username>/SINDy_VIV.git
cd SINDy_VIV
jupyter notebook
```

Run:

* `SINDy_wake_oscillator.ipynb`
* `SINDy_lift_oscillator.ipynb`

---

## References

[1] Facchinetti, M.L., de Langre, E., Biolley, F. (2004)
*Coupling of structure and wake oscillators in vortex-induced vibrations*

[2] Hartlen, R.T., Currie, I.G. (1970)
*Lift-oscillator model of vortex-induced vibration*

---

## Future Work

* Physics-informed SINDy
* Extension to turbulent flows
* Coupling with PINNs
* Generative models for flow reconstruction

---

