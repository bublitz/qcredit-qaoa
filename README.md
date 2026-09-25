# QCredit — Credit Portfolio Optimization with QAOA

[![Open in Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bublitz/qcredit-qaoa/blob/main/QCredit_QAOA.ipynb)

A didactic implementation of the **Quantum Approximate Optimization Algorithm (QAOA)** applied to a constrained credit-portfolio selection problem using **Qiskit**.

## Overview

QCredit models the selection of customers for a credit portfolio as a binary combinatorial optimization problem.

For each customer:

- `1` means the customer is selected;
- `0` means the customer is not selected.

The objective combines:

- expected return;
- risk-adjusted return;
- a maximum portfolio credit budget;
- a maximum portfolio risk;
- quadratic penalties for constraint violations.

The complete workflow is:

**business problem → cost function → classical benchmark → Pauli-Z Hamiltonian → QAOA ansatz → variational optimization → shot-based sampling → business decision**

## Technical Approach

The notebook demonstrates:

- binary formulation of the portfolio-selection problem;
- cost-function design with risk and constraint penalties;
- exact classical enumeration of all `2^6 = 64` portfolios;
- conversion of the classical cost function into a diagonal Hamiltonian;
- Hamiltonian verification against the classical cost function;
- QAOA construction with `QAOAAnsatz`;
- hybrid quantum/classical parameter optimization;
- five optimization restarts;
- 4,000 simulated measurements;
- post-processing based only on states actually observed;
- comparison between the exact classical reference and QAOA measurements;
- sensitivity analysis under conservative, base, and aggressive credit-policy scenarios.

## Experimental Results

For the six-customer instance, the exact classical feasible reference is:

| Metric | Result |
|---|---:|
| Portfolio bitstring | `011100` |
| Total credit | 9,000 |
| Expected return | 2,550 |
| Expected risk | 1,090 |
| Net value | 2,114 |
| Feasible | Yes |

The QAOA experiment used `QAOAAnsatz` with `reps=2`. The optimization found a normalized expected cost of approximately `0.006705`, and the final measurement stage used 4,000 shots with `AerSimulator`.

The most frequent measured state was `001001`, with 123 occurrences (3.075%). The best feasible portfolio observed among the measurements was `011100`, with 81 occurrences (2.025%), matching the exact classical feasible reference. A total of 62 distinct states were observed, corresponding to 96.9% of the 64-state solution space.

These results should be interpreted as an experiment on a deliberately small simulated instance, not as evidence of quantum advantage.

## Why This Project Matters

The project demonstrates the full modeling chain required to apply a quantum optimization algorithm to a business problem:

1. Translate a business decision into binary variables.
2. Define an objective that incorporates return, risk, and constraints.
3. Build an exact classical benchmark.
4. Map the cost function to a quantum Hamiltonian.
5. Construct and optimize a QAOA circuit.
6. Sample the resulting distribution.
7. Translate measured bitstrings back into business metrics.
8. Analyze sensitivity to business-policy parameters.

## Limitations

This is a **proof of concept for education and experimentation**.

The problem intentionally contains only six customers, making exhaustive classical enumeration possible. The Hamiltonian is constructed through a full Pauli-Z expansion, which is practical for this small instance but is not presented as a scalable formulation for real-world credit portfolios.

Larger instances introduce additional challenges:

- exponential growth of the classical search space;
- increased qubit requirements;
- more difficult variational optimization;
- penalty calibration;
- noise and hardware limitations;
- the need for more economical QUBO/Ising formulations.

A real credit-decision system would also require appropriate governance, regulatory validation, explainability, risk controls, data quality procedures, and expert review.

## Repository Contents

```text
qcredit-qaoa/
├── QCredit_QAOA.ipynb
├── README.md
├── requirements.txt
├── .gitignore
└── images/
```

## Running Locally

```bash
python -m venv .venv
```

Activate the environment and install the dependencies:

```bash
pip install -r requirements.txt
```

Then launch Jupyter:

```bash
jupyter notebook
```

Open `QCredit_QAOA.ipynb` and run the notebook from top to bottom.

## Running in Google Colab

The notebook was originally developed for Google Colab. The repository includes a **Open in Google Colab** badge at the top of this README.

## References

1. Brazil Quantum Camp — Block 2, Lesson 4: Quantum Optimization II.
2. NordIQuEst Application Library — *Solving Flight Scheduling Optimization using QAOA*.
3. IBM Quantum Documentation — `QAOAAnsatz`.
4. IBM Quantum Documentation — `SparsePauliOp`.

## Disclaimer

This repository is an educational and experimental project. The data and results are synthetic/small-scale and must not be used for real-world credit decisions.
