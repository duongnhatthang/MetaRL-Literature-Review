# Meta-Reinforcement Learning of Structured Exploration Strategies (MAESN)

## Key ideas: Using probabilistic latent vector **z** (acquired with helps from prior tasks) to help agent (adapt and) explore effectively.

#### Previous methods weakness:
  - Common MetaRL approaches (that recast meta-RL into RL problems) optimize a deterministic policy, which is time-invariant representations for action distributions, cannot represent highly exploratory behavior AND adapt very quickly to optimal behavior, especially for difficult tasks.
  - Exploration depends critically on stochasticity.
  - Previous methods' exploration policy don't have both stochastic and structured. The only source of stochastic is time-invariant in action space (time-invariant stochastic = the same amount of randomness at every time step. A better strategy would be small randomness at familiar place (i.e., at the start, since it's mostly the same across episode) then increasing the randdomness at uncertain place to gather as much info as possible). In theory, the previous methods could capture time-correlated stochasticity as well, but their expirical results suggest otherwise.
  - Many research (cited in Related Work) proposed methods to improve exploration, but they are all "task agnostic" (?) => they don't make use of prior knowledge of the world (meta information).
  - Previous methods only experiment on tasks "where either a few trials are sufficient to identify the goals of the task" , or the policy should acquire a consistent “search” strategy, for example to find the exit in new mazes. For more difficult problem that required structured exploration, previous methods: time-invariant noise is limited for exploration and RNN based method can't adapt well enough with just the behavior from forward pass. MAML will just revert back to normal policy gradient (still only have time-invariant stochasticity).
  
#### MAESN:
  - MAESN injects temporally correlated noise (sample from a distribution informed by meta-learning from past experience) to "randomize" exploration strategies.
  - Build up from MAML: the agent is trained to quickly adapt to new task by one (or a few) gradient update(s).
  - They claim that **since they adapt new task with gradient update, it enable asymptotic performance (?) as learning from scratch => method that doesn't do so are limited in their performance, might not even go to the converge point of training from scratch.**
  - Introduce structured stochasticity exploration policy (temporary extended exploration/ deep exploration/ meta-exploration).
