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

# There we go,this journey only gets damn interesting as ML has always been for me!!

## Module 2: Drift-Aware Predictive Load Balancing

### Motivation
While reinforcement learning enables adaptive scheduling, it often requires long training horizons and careful reward shaping. This module explores whether **lightweight predictive modeling** can improve scheduling decisions by anticipating near-future system load, particularly under workload drift or transient spikes.

### Approach
The scheduler augments the classical **Least-Work-Remaining (LWR)** heuristic with short-horizon load prediction:
- Simple Moving Average (SMA) and Exponential Weighted Moving Average (EWMA) are used to track recent CPU load trends.
- A lightweight linear regression model predicts near-future CPU load.
- A drift detection mechanism activates predictive scheduling only when predicted load deviates significantly from current load; otherwise, the scheduler falls back to standard LWR.

This hybrid design aims to balance robustness and stability while avoiding unnecessary prediction noise.

### Experimental Setup
- Heterogeneous multiprocessor system with dynamic task arrivals
- Decision-dependent SLA modeling based on task completion time vs deadline
- Baselines: Round Robin (RR), Least-Work-Remaining (LWR)
- Metrics: average reward, oracle agreement, SLA violation rate

### Key Results
- The predictive scheduler consistently outperforms **Round Robin**, confirming the benefit of informed scheduling.
- **Least-Work-Remaining remains the strongest baseline** under queue-dominated and smoothly evolving workloads.
- Predictive scheduling does not universally outperform greedy heuristics; estimation noise can outweigh benefits when system dynamics are stable.

### Insights & Lessons Learned
- Scheduling effectiveness is highly **workload-dependent**.
- Predictive models are most beneficial under abrupt load shifts; in stable regimes, greedy heuristics remain near-optimal.
- These results highlight an important limitation of prediction-based scheduling and reinforce the need for adaptive strategy selection rather than a single universal policy.

### Future Directions
- Adaptive drift thresholds based on workload volatility
- Non-linear or probabilistic predictors for highly dynamic workloads
- Integration with learning-based or graph-based schedulers for global context



