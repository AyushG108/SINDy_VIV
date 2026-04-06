===================================================
SINDy_VIV
===================================================

Sparse Identification of Nonlinear Dynamics (SINDy) is used in this project to discover governing equations for vortex-induced vibration (VIV) systems. This repository combines classical reduced-order physics-based models with data-driven system identification.

===================================================
Overview
===================================================

This work focuses on two canonical VIV reduced-order models:

Wake Oscillator Model (Facchinetti et al., 2004)

Lift Oscillator Model (Hartlen & Currie, 1970)

The pipeline is:

Solve governing equations using numerical integration

Generate time-series data (wake variable q, lift coefficient C_l)

Construct nonlinear feature libraries

Apply SINDy to discover governing equations

===================================================
Governing Equations
===================================================

Wake Oscillator Model (Facchinetti et al.)

The coupled fluid-structure system is given by:

y_ddot + 2zetaomega_n*y_dot + omega_n^2*y = F/m

q_ddot + epsilon*(q^2 - 1)*q_dot + omega_s^2q = Ay_ddot

where:

y = structural displacement

q = wake variable

zeta = damping ratio

omega_n = natural frequency

epsilon = nonlinear wake parameter

Lift Oscillator Model (Hartlen & Currie)

The system is:

x_dot = v

v_dot = 2*a*omega^2*C_l - x - 2*zeta*v

C_l_dot = p

p_dot = f(C_l, p)

where f(C_l, p) is a nonlinear polynomial representation of lift dynamics.

===================================================
SINDy Formulation
===================================================

The identified system is expressed as:

q_dot = u

u_dot = f(q, q_dot)

A candidate library includes:

Polynomial terms: q, q^2, q^3

Cross terms: q*q_dot

Higher-order derivatives: q_dot^2, q_dot^3

Sparse regression selects only the dominant terms.

===================================================
Discovered Models (SINDy Results)
===================================================

Wake Oscillator (from simulated data)

Identified dynamics:

q_dot = u

u_dot = 0.074 - 0.400*q - 29.462*u - 0.018*q^2 + 0.005*q*u + 0.017*u^2 + 0.002*q^3 + 0.089*q^2*u - 0.397*q*u^2 + 0.025*u^3

Lift Oscillator (from CFD data)

Identified lift dynamics:

C_l_dot = p

p_dot = 0.169 + 0.119*C_l + 1.536*p - 0.088*C_l^2 - 0.021*C_l*p - 0.064*p^2 + 0.180*C_l^3 - 0.769*C_l^2*p - 0.180*C_l*p^2 + 0.766*p^3

===================================================
CFD-Based Modeling
===================================================

Lift coefficient (C_l) is obtained from CFD of a static cylinder

Time-series data is used directly for system identification

No explicit governing equations are assumed

This highlights:
-> Physics-based modeling (wake oscillator)
-> Data-driven discovery (CFD + SINDy)

===================================================
Physical Model
===================================================

Model-of-1DOF-elastically-supported-rigid-structure-experiencing-VIV.png

===================================================
Repository Structure
===================================================

SINDy_wake_oscillator.ipynb -> Wake oscillator + SINDy identification

SINDy_lift_oscillator.ipynb -> Lift oscillator + SINDy identification

wake_oscillator_model.ipynb -> Base wake model

lift_oscillator.ipynb -> Base lift model

Additional_files.ipynb -> Additional utilities/experiments

CFD_lift_drag_time_series_stable... -> CFD datasets

Doc.pdf -> Documentation

miscellaneous_training_lift_oscillator... -> Misc training scripts

===================================================
Requirements
===================================================

Python 3.x
NumPy
SciPy
Matplotlib
PySINDy (optional but recommended)

pip install numpy scipy matplotlib pysindy

===================================================
How to Run
===================================================

git clone https://github.com/<your-username>/SINDy_VIV.git
cd SINDy_VIV
jupyter notebook

Run:

SINDy_wake_oscillator.ipynb

SINDy_lift_oscillator.ipynb

===================================================
References
===================================================

[1] Facchinetti, M.L., de Langre, E., Biolley, F. (2004)
Coupling of structure and wake oscillators in vortex-induced vibrations

[2] Hartlen, R.T., Currie, I.G. (1970)
Lift-oscillator model of vortex-induced vibration

===================================================
Future Work
===================================================

Physics-informed SINDy

Extension to turbulent flows

Coupling with PINNs

Generative models for flow reconstruction
