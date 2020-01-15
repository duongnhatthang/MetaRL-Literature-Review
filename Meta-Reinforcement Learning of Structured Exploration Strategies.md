# Meta-Reinforcement Learning of Structured Exploration Strategies (MAESN)

## Key ideas:

### 1. Using probabilistic latent vector **z** (acquired with helps from prior tasks) to help agent (adapt and) explore effectively.

#### Previous methods weakness:
  - Common MetaRL approaches (that recast meta-RL into RL problems) optimize a deterministic policy, which is time-invariant representations for action distributions, cannot represent highly exploratory behavior AND adapt very quickly to optimal behavior, especially for difficult tasks.
  - Exploration depends critically on stochasticity.
