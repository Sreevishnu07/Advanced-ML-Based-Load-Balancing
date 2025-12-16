## Advanced ML-Based Load Balancing
This repository explores advanced machine learning techniques for
dynamic load balancing in multiprocessor systems.

The project builds upon classical scheduling algorithms and extends
them using reinforcement learning, drift-aware ML, and graph-based models.

# So yea added my first module(a big learning curve in all fairness)

## Module 1: Reinforcement Learning–Based Scheduler (DQN)

### Problem Formulation
Task scheduling in a heterogeneous multiprocessor system is modeled as a Markov Decision Process (MDP):
- **State**: Task attributes (size, priority) and per-CPU system metrics (queue length, load, speed, wait time)
- **Action**: Assign the incoming task to one of the available processors
- **Reward**: Negative task completion time with penalties for load imbalance and SLA violations

The goal is to learn a scheduling policy that minimizes latency and SLA violations under dynamic system conditions.

### Approach
A Deep Q-Network (DQN) is trained on a simulated workload to learn state-dependent scheduling decisions. Experience replay and target networks are used to stabilize learning. The learned policy is evaluated against classical baselines.

### Key Results
- The RL agent demonstrates clear learning behavior, improving average reward by over **40%** during training.
- It significantly outperforms **Round Robin** scheduling across all metrics.
- While **Least-Work-Remaining (LWR)** remains a strong greedy baseline, the RL policy achieves **comparable SLA violation rates** under limited training time.
- The learned policy adapts decisions based on system state rather than relying on static heuristics.

### Insights & Limitations
- Reinforcement learning requires sufficient training time to surpass strong greedy heuristics such as LWR.
- Under short training horizons, RL trades off oracle accuracy for adaptability.
- These results highlight the importance of reward shaping and long-horizon training for RL-based schedulers.

### Future Extensions
- Longer training horizons and improved exploration schedules
- Incorporating predictive state features (e.g., workload trends)
- Extending the approach to multi-task and multi-objective scheduling


