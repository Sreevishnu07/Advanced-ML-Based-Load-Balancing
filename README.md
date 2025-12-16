# Advanced ML-Based Load Balancing
This repository explores advanced machine learning techniques for
dynamic load balancing in multiprocessor systems.

The project builds upon classical scheduling algorithms and extends
them using reinforcement learning, drift-aware ML, and graph-based models.

# So yea added my first module(a big learning curve in all fairness)
This work explores reinforcement learning for dynamic load balancing by modeling task scheduling as a Markov Decision Process and training a Deep Q-Network on a simulated multiprocessor workload. The results demonstrate clear learning trends, with the RL agent improving reward by over 40% during training and substantially outperforming Round Robin scheduling. Although Least-Work-Remaining remains a strong greedy baseline, the RL agent achieves comparable SLA violation rates under limited training time while exhibiting adaptive, state-dependent scheduling decisions. These findings suggest that reinforcement learning holds promise for anticipatory scheduling in complex, dynamic systems, particularly when extended with longer training horizons or predictive state features.

