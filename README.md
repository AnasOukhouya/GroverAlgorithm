# Grover's Algorithm Implementation (Target State: 010)

This repository contains a Python implementation of **Grover's Algorithm** using IBM's **Qiskit** framework. The project demonstrates a quantum search for a specific 3-bit target state (`010`) within an unstructured database.

## Project Overview
Grover's algorithm is a quantum algorithm that provides a quadratic speedup for searching unstructured data. While a classical search would take $O(N)$ operations, Grover's algorithm finds the target in $O(\sqrt{N})$ steps.



In this implementation:
* **Target State**: `010`.
* **Total Qubits**: 4 (3 data qubits + 1 auxiliary qubit for phase kickback).
* **Iterations**: The code executes two full iterations of the Grover operator (Oracle + Diffuser) to show the amplification of the target state's probability.

## How It Works

### The Oracle & Phase Kickback
The oracle identifies the target state by flipping its phase from +1 to -1, leaving all other states unchanged. To do this without measuring (which would collapse the superposition), the implementation uses a 4th auxiliary qubit initialized in the |−⟩ state. When the multi-controlled X gate (`C3XGate`) fires on the target state `010`, it flips the auxiliary qubit — but because that qubit is in superposition, the effect "kicks back" as a global phase flip onto the target state itself. This is phase kickback: the auxiliary qubit absorbs the operation and transfers its effect as a phase change to the control qubits, leaving the auxiliary qubit unchanged afterward.

### Why 2 Iterations?
The number of Grover iterations that maximizes the probability of measuring the target state is approximately:

$$k = \left\lfloor \frac{\pi}{4} \sqrt{N} \right\rfloor$$

For a 3-qubit search space of N = 8 states, this gives k ≈ 2.2, meaning **2 iterations is theoretically optimal**. Running a third iteration would actually start *decreasing* the probability — a counterintuitive but important property of amplitude amplification.

## Features
* **Custom Oracle**: Implements a phase oracle designed to flip the sign of the `010` state using `X` gates and a multi-controlled X gate (`C3XGate`).
* **Diffuser (Inversion about the mean)**: An implementation of the diffuser operator to amplify the probability amplitude of the marked state.
* **Visualizations**: Uses `matplotlib` and `seaborn` to plot probability distributions after each iteration, visualizing how the quantum state converges on the target.

## Results & Interpretation

The two probability distribution plots illustrate amplitude amplification in action:

- **After iteration 1:** The target state `0010` (Qiskit's bit ordering places the auxiliary qubit first) reaches a probability of **0.781**, while all 7 non-target states drop to ~0.031 each.
- **After iteration 2:** The target state reaches **0.945**, and non-target states collapse further to ~0.008 each.

This convergence is not random — it is the deterministic result of the oracle and diffuser working together. The oracle marks the target by inverting its phase; the diffuser (inversion about the mean) then amplifies marked amplitudes and suppresses unmarked ones. Each iteration sharpens the distribution further, which is why a classical O(N) search is outperformed by Grover's O(√N) approach.

Note that the states `1xxx` all show probability 0 — these correspond to states where the auxiliary qubit is |1⟩, which are unreachable from the initialized state and correctly filtered out.

## Limitations & Next Steps

This implementation has a few intentional simplifications worth noting:

- **Hardcoded target:** The oracle is built specifically for `010`. A generalized version would construct the oracle dynamically for any target state.
- **Simulator only:** Results are computed via statevector simulation, which is exact. On real quantum hardware, noise would reduce the peak probability and require error mitigation techniques.
- **Single marked item:** Grover's algorithm generalizes to multiple marked states, where the optimal iteration count adjusts to k ≈ (π/4) × √(N/M) for M solutions — a natural extension to explore.

A logical next step would be implementing a parameterized oracle that accepts any target bitstring, and testing the circuit on IBM's real quantum backends via the Qiskit Runtime API.

## Prerequisites
To run the Jupyter notebook, you will need the following Python libraries installed:
```bash
pip install qiskit matplotlib seaborn
