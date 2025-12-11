Zero-Noise Extrapolation (ZNE) · Measurement Error Mitigation · Depolarizing Noise Scaling

This repository contains my full research workflow, experiments, data, and analysis on quantum noise mitigation techniques using Qiskit Aer.
The goal of this project is to evaluate how different types of noise affect quantum circuits and how classical post-processing techniques—like Zero-Noise Extrapolation (ZNE) and measurement error mitigation—can improve accuracy on noisy quantum hardware.

This project uses the Bell state (|00⟩ + |11⟩)/√2 as a benchmark circuit.

Repository Structure
Quantum-noise-mitigation/
│
├── paper/
│   ├── draft.docx                # Research paper draft
│   └── literature/               # References & notes
│
├── notebooks/
│   ├── exp1_baseline.ipynb       # Experiment 1: Baseline fidelity under noise
│   ├── exp2_noise_scaling.ipynb  # Experiment 2: Zero-Noise Extrapolation (ZNE)
│   ├── exp3_IBM_backend.ipynb    # Experiment 3: Measurement error mitigation
│
├── src/
│   ├── noise_model.py            # Custom depolarizing + readout error models
│   └── scaling_algorithm.py      # ZNE polynomial + linear extrapolation
│
├── results/
│   ├── raw_data/                 # CSV files from all experiments
│   └── graphs/                   # Plots generated during analysis
│
└── README.md                     # Project documentation







Experiment 1 — Baseline Noise Fidelity Test

Goal: Understand how depolarizing noise alone affects state fidelity.

We sweep depolarizing probabilities from 0 → 0.05 and compute fidelity:

Ideal statevector length = 4

Ideal fidelity with itself = ~1.0

Fidelity decreases linearly as noise increases

Results stored in:
results/raw_data/exp1_noise_sweep.csv

Graph:
results/graphs/exp1_fidelity_vs_noise.png

Key Insight:
Even small noise levels (1–5%) reduce fidelity significantly for entangled states.







xperiment 2 — Zero-Noise Extrapolation (ZNE)

Goal: Improve accuracy by simulating artificially increased noise and extrapolating back to zero noise.

We scale noise by factors: 1×, 2×, 3×
Then compute state fidelity and apply:

Degree-2 polynomial fit

Richardson linear fit

Outputs:

ZNE-extrapolated fidelity ≈ 0.99999999

Restores nearly perfect fidelity

Data:
results/raw_data/exp2_zne_summary.csv

Graph:
results/graphs/exp2_zne_curve.png

Key Insight:
ZNE accurately predicts the zero-noise value even when raw fidelities drop to 0.96–0.98.







Experiment 3 — Measurement-Based ZNE + Calibration

Goal: Test measurement error mitigation and compare it with statevector results.

Steps:

Build measurement circuits

Generate 4 calibration circuits (00, 01, 10, 11)

Build calibration matrix

Run noisy measurement sampling

Apply classical mitigation via matrix inversion

Sweep noise scale again

Outputs include:

Raw fidelity (measurement)

Mitigated fidelity

Comparison vs statevector-based fidelity

Data:
results/raw_data/exp3_measurement_zne.csv

Graph:
results/graphs/exp3_measurement_zne.png

Key Insight:
Mitigation improves accuracy but cannot reach statevector ZNE performance due to sampling noise and readout errors.
However, trends remain consistent.






Depolarizing error
Applied to:
Single-qubit gates (h)
Two-qubit gates (cx)

2. Readout error
Defined via confusion matrices and applied to each qubit.

3. Scalable noise
For ZNE:
effective_noise = base_noise × scale_factor




Technologies & Tools

Qiskit Aer (statevector simulator, noise models)
Qiskit IBM Runtime (optional)
NumPy, Pandas, Matplotlib
Polynomial and Richardson extrapolation
Google Colab / Jupyter Notebook





summary of Results

Experiment	Technique	Key Result
Exp 1	Baseline noise sweep	Fidelity decreases linearly from 1.0 → ~0.95
Exp 2	Zero-Noise Extrapolation	Extrapolated fidelity ≈ 1.0
Exp 3	Measurement mitigation	Raw ≈ 0.49 → Mitigated ≈ 0.49 (stable), trend matches theory




Conclusion

This project demonstrates that:
Quantum circuits degrade significantly even under small depolarizing noise.
Classical ZNE techniques can recover near-ideal results without modifying hardware.
Measurement error mitigation is essential for realistic experiments.
Combining statevector ZNE with measurement mitigation provides the most complete noise-characterization pipeline.
This serves as a foundation for more advanced techniques like dynamical decoupling, Clifford data regression, and machine-learning-based noise suppression.




Contributions

Contributions, suggestions, and improvements are welcome!
Feel free to open a Pull Request or Issue.
