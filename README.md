# MetaRL-Literature-Review
- Review related papers for the topic: Meta Reinforcement Learning for sequential decision problems.
- 1-5 score how confident I am in understanding the paper (5: I think I got all the key concepts. 1: I just skim through the paper)


## Problem statement and notation:
Add later.

**Note:** At the moment, I focus more on MetaRL for Bandit problem.

## Paper list:

### Context based:
#### Probabilistic latent context:
- [x] [Efficient Off-Policy Meta-Reinforcement Learning via Probabilistic Context Variables](https://arxiv.org/pdf/1903.08254.pdf) (PEARL) (4.5 Read Soft Actor-Critic paper to fully understand final details)
- [x] [Meta-Reinforcement Learning of Structured Exploration Strategies](https://arxiv.org/pdf/1802.07245.pdf) (MAESN) (3)
#### Deterministic latent context:
- [x] [A Simple Neural Attentive Meta-Learner](https://arxiv.org/pdf/1707.03141.pdf)(5) (SNAIL)
- [x] [RL2: Fast Reinforcement Learning via Slow Reinforcement Learning](https://arxiv.org/pdf/1611.02779.pdf)(5) (RL2)
<br>

### Gradient based:
- [x] [Learning to Reinforcement Learn](http://www.gatsby.ucl.ac.uk/~ucgtcbl/papers/WangEtAl2016.pdf)(4)
- [ ] [Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/pdf/1703.03400.pdf) (MAML)
- [x] [Learning to Learn without Gradient Descent by Gradient Descent](http://proceedings.mlr.press/v70/chen17e/chen17e.pdf) (3.5)
- [x] [Meta-learning of Sequential Strategies](https://arxiv.org/pdf/1905.03030.pdf)(1) (Review stuff)
<br>

### Others:
- [ ] [Soft Actor-Critic: Off-Policy Maximum Entropy Deep Reinforcement Learning with a Stochastic Actor](https://arxiv.org/pdf/1801.01290.pdf) (SAC)
<br>

### Less relevant:
- [x] [Meta-Learning for Contextual Bandit Exploration](https://arxiv.org/pdf/1901.08159.pdf)(3)
- [ ] [A Bayesian Approach to Unsupervised One-Shot Learning of Object Categories](http://vision.stanford.edu/documents/Fei-Fei_ICCV03.pdf) (Hierarchical Bayesian approach)
- [x] [Prediction, Consistency, Curvature: Representation Learning for Locally-Linear Control](https://arxiv.org/pdf/1909.01506.pdf) (3) (PCC)

## Result comparison:

| Setup (N, K) | Gittins (optimal as N → ∞) | Random | RL2        | MAML     | SNAIL     | TS    | OTS   | Tuned-UCB | Eps-Greedy | Greedy |
|--------------|----------------------------|--------|------------|----------|-----------|-------|-------|-----------|------------|--------|
| 10,5         | **6.6**                    | 5.0    | **6.7**    | 6.5      | **6.6**   | 5.7   | 6.5   | 6.7       | 6.6        | 6.6    |
| 10,10        | **6.6**                    | 5.0    | **6.7**    | **6.6**  | **6.7**   | 5.5   | 6.2   | 6.7       | 6.6        | 6.6    |
| 10,50        | 6.5                        | 5.1    | **6.8**    | **6.6**  | **6.7**   | 5.2   | 5.5   | 6.6       | 6.5        | 6.5    |
| 100,5        | **78.3**                   | 49.9   | **78.7**   | 67.1     | **79.1**  | 74.7  | 77.9  | 78.0      | 75.4       | 74.8   |
| 100,10       | **82.8**                   | 49.9   | **83.5**   | 70.1     | **83.5**  | 76.7  | 81.4  | 82.4      | 77.4       | 77.1   |
| 100,50       | **85.2**                   | 49.8   | **84.9**   | 70.3     | **85.1**  | 64.5  | 67.7  | 84.3      | 78.3       | 78.0   |
| 500,5        | **405.8**                  | 249.8  |   401.5    | -        | **408.1** | 402.0 | 406.7 | 405.8     | 388.2      | 380.6  |
| 500,10       | **437.8**                  | 249.0  | **432.5**  | -        | **432.4** | 429.5 | 438.9 | 437.1     | 408.0      | 395.0  |
| 500,50       | **463.7**                  | 249.6  |   438.9    | -        | 442.6     | 427.2 | 437.6 | 457.6     | 413.6      | 402.8  |
| 1000,50      | **944.1**                  | 499.8  |   847.43   | -        | 889.8     | -     | -     | -         | -          | -      |

Table 1:  Results on **multi-arm bandit** problems. OTS: Optimistic Thompson Sampling. Greedy: with the best empirical mean. Horizon = N, Number of arms = K

<br>
<br>

| N   | Random | Eps-Greedy | PSRL  | OPSRL     | UCRL2 | RL2   | MAML  | SNAIL |
|-----|--------|------------|-------|-----------|-------|-------|-------|-------|
| 10  | 0.482  | 0.640      | 0.665 | 0.694     | 0.706 | 0.752 | 0.563 | **0.766** |
| 25  | 0.482  | 0.727      | 0.788 | 0.819     | 0.817 | 0.859 | 0.591 | **0.862** |
| 50  | 0.481  | 0.793      | 0.871 | 0.897     | 0.885 | 0.902 | -     | **0.908** |
| 75  | 0.482  | 0.831      | 0.910 | **0.931** | 0.917 | 0.918 | -     | **0.930** |
| 100 | 0.481  | 0.857      | 0.934 | **0.951** | 0.936 | 0.922 | -     | 0.941 |

Table 2:  Results on **tabular MDPs**. Check SNAIL paper for original source.

## My experiment results:

| Methods                                   | Regret              |
|-------------------------------------------|---------------------|
| Random                                    | 50.1715 +/- 36.0777 |
| Thompson Sampling                         | 3.5319 +/- 8.0465   |
| Un-tuned UCB                              | 10.2620 +/- 8.2752  |
| Finite Difference                         | ~ random            |
| A2C                                       | ~ random            |
| DQN (eps=0)                               | ~ random            |
| DQN (eps=0.1) replay-memory: >100 trajectories | ~14-16              |
| DQN (eps=0.1) replay-memory: ~12 trajectories | 7.6187 +/- 9.9622   |
| Augmented DQN (eps=0.1)                   | 8.5549 +/- 11.5184  |
| Augmented DQN (eps=0)                     | 9.8793 +/- 28.7358  |

Table 3: Results on multi-arm bandit problems. Horizon = 300, number of arms = 2, gamma = 0.9.

**NOTE**: 
- There is some bias in the number: DQN (12 trajectories) method received more trials than others.
- Augmented DQN: generate training samples with known good latent features (average reward, number_of_chosen**-0.5, current timestep).
- Augmented DQN on average required ~7 times less data to converge than vanilla DQN.

### Open questions:
- [ ] Augmented DQN basically teach the model to do Meta-Exploration => How do we do this to extend to other problems and method.
- [ ] Why does eps = 0.1 help improve the performance ? (training with eps is understanable, but it helps even when inference)
- [ ] Understanding PEARL: investigating the prior that they estimate. Does it overlap with the (bandit) environment actual prior? Can we improve upon this method?
