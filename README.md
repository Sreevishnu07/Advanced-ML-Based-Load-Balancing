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

# Module 3: GNN-Based Global Load Balancing

This module introduces a **Graph Neural Network (GNN)–inspired scheduler** for dynamic load balancing in heterogeneous multiprocessor systems. Unlike local greedy heuristics or oracle imitation, the scheduler models **global inter-processor context** and learns to directly predict task completion cost, enabling informed and robust scheduling decisions.

---

## Motivation

Earlier modules revealed important limitations:
- Greedy schedulers (e.g., Least-Work-Remaining) are strong under stable, queue-dominated workloads but lack global awareness.
- Predictive and reinforcement learning approaches are sensitive to training horizon, noise, and objective alignment.
- Oracle imitation via classification can fail due to score miscalibration during inference.

This module addresses these limitations by **aligning learning objectives with scheduling physics** and incorporating **global relational reasoning**.

---

## Key Idea

Processors are modeled as **nodes in a fully connected graph**, where each node represents a CPU with local state features. A shared neural network performs **message passing** by combining local CPU features with a global system context, enabling each CPU to reason about the overall system state.

Instead of predicting *which CPU to choose*, the model learns to **regress task completion time per CPU**, and scheduling decisions are made by selecting the CPU with the minimum predicted completion cost.

---

## Graph Formulation

- **Nodes**: CPUs
- **Node Features**:
  - Queue length
  - Current load
  - Waiting time
  - CPU speed
  - Task size
  - Task priority
  - System-wide average load
- **Message Passing**:
  - One-hop global aggregation via mean pooling
- **Output**:
  - Predicted completion time for each CPU

---

## Learning Objective

- **Training**: Supervised regression using true task completion time
- **Loss Function**: Mean Squared Error (MSE)
- **Decision Rule**: Select CPU with minimum predicted completion time

This formulation ensures:
- Alignment between training and inference objectives
- Stable ranking across CPUs
- Direct optimization of scheduling-relevant cost

---

## Experimental Results

| Scheduler | Oracle Accuracy | SLA Violation Rate |
|---------|----------------|--------------------|
| Round Robin | ~25% | ~64% |
| Least-Work-Remaining | ~71% | ~43% |
| **GNN-Based (Regression)** | **~96%** | **~39%** |

The GNN-based scheduler consistently **outperforms Least-Work-Remaining** in SLA violations by leveraging global system context.

---

## Key Insights

- Oracle imitation via classification is insufficient for scheduling due to misaligned objectives.
- Cost-based learning (completion-time regression) is critical for reliable ML-driven schedulers.
- Global relational reasoning enables performance gains beyond local greedy heuristics.
- GNNs are effective for scheduling **only when trained to model cost, not action**.

---

## Limitations & Future Work

- Current graph connectivity is fully connected; adaptive or sparse graphs could improve scalability.
- Temporal message passing could capture longer-term workload evolution.
- Integration with reinforcement learning could enable online adaptation.

---

## Conclusion

This module demonstrates that **GNN-based global reasoning**, when correctly formulated as a cost regression problem, provides a robust and effective approach to multiprocessor load balancing. The results highlight the importance of objective alignment and relational modeling in ML-driven scheduling systems.

# Thank you for listening!


