# Grover's Algorithm Implementation (Target State: 010)

This repository contains a Python implementation of **Grover's Algorithm** using IBM's **Qiskit** framework. The project demonstrates a quantum search for a specific 3-bit target state (`010`) within an unstructured database.

## Project Overview
Grover's algorithm is a quantum algorithm that provides a quadratic speedup for searching unstructured data. While a classical search would take $O(N)$ operations, Grover's algorithm finds the target in $O(\sqrt{N})$ steps.

In this implementation:
* **Target State**: `010`.
* **Total Qubits**: 4 (3 data qubits + 1 auxiliary qubit for phase kickback).
* **Iterations**: The code executes two full iterations of the Grover operator (Oracle + Diffuser) to show the amplification of the target state's probability.

## Features
* **Custom Oracle**: Implements a phase oracle designed to flip the sign of the `010` state using `X` gates and a multi-controlled X gate (`C3XGate`).
* **Diffuser (Inversion about the mean)**: An implementation of the diffuser operator to amplify the probability amplitude of the marked state.
* **Visualizations**: Uses `matplotlib` and `seaborn` to plot probability distributions after each iteration, visualizing how the quantum state converges on the target.

## Prerequisites
To run the Jupyter notebook, you will need the following Python libraries installed:
```bash
pip install qiskit matplotlib seaborn
