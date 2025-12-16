# MDP Formulation for RL-Based Load Balancing

This document defines the Markov Decision Process (MDP) used to model
dynamic load balancing in a heterogeneous multiprocessor system.

The objective is to learn a scheduling policy that minimizes task
latency while maintaining balanced utilization across processors.

---

## 1. Environment

The environment consists of:
- P heterogeneous processors
- A stream of incoming tasks arriving over time

A decision is made at **each task arrival**.

---

## 2. State Space (S)

The state represents the system snapshot at task arrival.


This state captures:
- processor heterogeneity
- current imbalance
- task-specific requirements

---

## 3. Action Space (A)

An action assigns the incoming task to exactly one processor.


The action space is discrete and scales linearly with the number of processors.

---

## 4. Transition Function (T)

After action `a`:
- load of selected processor increases
- waiting times are updated
- other processors continue execution
- next task arrival occurs stochastically

Transitions are partially stochastic due to unknown execution times
and arrival patterns.

---

## 5. Reward Function (R)

The reward encourages low latency and balanced load.

Example formulation:


Where:
- `task_wait_time` = estimated delay for the assigned task
- `load_variance` = variance of processor loads
- `overload_penalty` = penalty if any processor exceeds a threshold
- α, β, γ are tunable weights

---

## 6. Episode Definition

An episode may be defined as:
- a fixed number of task assignments, or
- a fixed simulation time window

---

## 7. Objective

Learn a policy π(s) that maximizes expected cumulative reward:


This enables adaptive and self-optimizing scheduling under dynamic workloads.
