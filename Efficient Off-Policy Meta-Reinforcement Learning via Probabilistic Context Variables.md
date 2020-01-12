# Efficient Off-Policy Meta-Reinforcement Learning via Probabilistic Context Variables

## Key ideas:

- Off-policy to increase Sample efficiency (since on-policy is much worse). But, using off-policy method to solve meta-problems is challenging. (note: A3C, TRPO and PPO is on-policy algorithms):
   - "... modern meta-learning is predicated on the assumption that the distribution of data used for adaptation will
match across meta-training and meta-test. In RL, this implies that since at meta-test time on-policy data will be used
to adapt, on-policy data should be used during meta-training as well. Furthermore, meta-RL requires the policy to reason about distributions, so as to learn effective stochastic exploration strategies. This problem inherently cannot be solved by off-policy RL methods that minimize temporaldifference error, as they do not have the ability to directly optimize for distributions of states visited. In contrast, policy gradient methods have direct control over the actions taken by the policy. Given these two challenges, a naive combination of meta-learning and value-based RL could be ineffective. In practice, we were unable to optimize such a method." (**=> Basically what we did with DQN wouldn't easily transfer to other problems.**)
   - Figure 6 below shows that data sampling strategy (from the replay buffer) can affect performance.
   - Figure 7 and 4 shows that a probabilistic context is important to solve sparse rewards problems with off-policy method. (I guess for the problem in figure 4, when received a (failed) reward, a deterministic latent estimator only tell that this location is wrong, explore elsewhere, while a probabilistic latent estimator would hypothesize that this whole surrounding are have smaller probability to contain goal than the other explored are, so we should go there.)
   ![off-policy is hard](https://github.com/duongnhatthang/MetaRL-Literature-Review/blob/master/images/off_policy_is_hard.png)
   ![sparse reward task](https://github.com/duongnhatthang/MetaRL-Literature-Review/blob/master/images/sparse_reward.png)
- Disentangles Task inference and Control: separate the process of "understanding" the current task and making policies (also used in Imitation Learning: example > encoded > behavior cloning (cited in PEARL paper)).
   - PEARL encodes the MDPs to probabilistic latent (context) variables Z. This (VAE inspired) way of encode has been used for (supervised) meta-learning before by [Rusu et al.](https://arxiv.org/abs/1807.05960), [Gordon et al.](https://arxiv.org/abs/1805.09921) and [Finn et al.](https://arxiv.org/abs/1806.02817) by using probabilistic latent task variables inferred bia amortized approximate inference. This paper extend to off-policy Meta-RL.
    ![PEARL encoder](https://github.com/duongnhatthang/MetaRL-Literature-Review/blob/master/images/PEARL_encoder.png)
   - Similar approaches to encode the MDPs include [SNAIL](https://arxiv.org/pdf/1707.03141.pdf), [RL2](https://arxiv.org/pdf/1611.02779.pdf) and [Recurrent DDPG](https://arxiv.org/abs/1512.04455). The image below used the RNN encoder from DDPG paper as baseline (haven't read, I guess it's just vanilla RNN like RL2 paper).
 
     + **=> PEARL encoding approach (of using probabilistic latent variable) helps optimizes faster, NOT improves performance.**
     + **=> Decorrelating the data(break the sequence order) for MDPs encoding is VERY important.**
  
     - Compare encoder effect (from PEARL paper): ![image compare encoder effect](https://github.com/duongnhatthang/MetaRL-Literature-Review/blob/master/images/encoder.png)

## Notes:
- Approaches that disentangles Task inference and Control (PEARL, RL2 and SNAIL) achieved better result than those that don't.
- The RL objective is optimized with [Soft Actor Critic](https://arxiv.org/pdf/1801.01290.pdf) (where they added maximizing the entropy as part of the objective to increase stability, perfomance (?) and sample efficiency (because off-policy ?)).

- Temporally-extended exploration/ deep exploration (what I call meta-exploration): exploration for the task structure, not for the current objective. 
   - "... acting optimally according to a random MDP allows for temporally extended (or deep) exploration, meaning that the agent can act to test hypotheses even when the results of actions are not immediately informative of the task. In the single-task deep RL setting, posterior sampling and the benefits of deep exploration has been explored by [Osband et al. (2016)](https://arxiv.org/abs/1306.0940), which maintains an approximate posterior over value functions via bootstraps." (PEARL paper). Osband et al. and [Strens](https://arxiv.org/pdf/1802.07245.pdf) "... make use of posterior sampling for efficient exploration at meta-test time" "... executes the optimal policy (using the posterior) for a sampled MDP for the duration of an episode".
   - [MAESN](https://arxiv.org/pdf/1802.07245.pdf) is very similar to this paper: using probabilistic latent (context) variables Z to solve meta-problem (it seems to follow the original idea from VAE more). The different with PEARL are:
     + This paper focus more on Meta-Exploration. 
     + MAESN method want to find the optimal initialzation of Muy and Sigma for all tasks, then use gradient descent for quick adaptation of these values (thus, uses these updated values across different tasks to meta-update the initial values). However, the PEARL method predicts these value as a product of multiple Gaussian factors, each predicted by a NN (Reparameterize trick ?) from 1 timestep's data.
     + MAESN showed that with a latent context vector Z, the agent do meta-exploration better (Figure 7 below). Their experiments is only for their method alone, but it can be presumable to be the same for all previous method that encode MDPs since the encoder effect is similar according to the PEARL paper (image above)).
     + MAESN also showed (in figure 8) that the choice of prior as Unit Gaussian is ok, since the latents can still adapted effectively to different region.
     + Additionally, MAESN showed that the structured noise DOES helps meta-exploration better than using a fixed latent context vector (Figure 9). Since the encoder effect is similar (to RNN) according to the PEARL paper, probabilistic latent extraction's only advantage is improving Sample efficiency for dense reward problem, but significantly improve performance for sparse reward problem (PEARL experiment above).
     ![meta-exploration](https://github.com/duongnhatthang/MetaRL-Literature-Review/blob/master/images/meta_exploration.png)

     
