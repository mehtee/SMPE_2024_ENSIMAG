
### What are Bandits?

Multi-Armed Bandits (MABs) is a class of online optimization problems in which a decision maker must choose one of multiple arms at each moment in time. Each arm returns a probabilistic reward; the goal is to maximize the total number of rewards accumulated over time. The problem lies in the trade-off between exploration (the attempt to diversify over arms to gain information about their rewards) and exploitation (the selection of the best performing arm according to data available). For instance, in a simple model arms yield rewards from a certain fixed distribution-for example Bernoulli distributions having probabilities $\mu_i$ for each arm$i$.

### Mathematical Formulation

The main goal in bandit problems is to reduce **regret**, which is the difference between the rewards you could have gotten by always picking the best option and the rewards you actually get from the choices you make over time. The **regret** $R(t)$ at time $t$ is given by:

$$
\text{Regret}(t) = \max_{i} \left(\sum_{t=1}^T R_{t,i} \right) - \sum_{t=1}^T R_{t,I_t}
$$

Where:
-$I_t$ is the arm selected at time$t$,
-$R_{t,i}$ represents the reward obtained from arm $i$.

To achieve sub-linear regret (i.e., regret growing slower than $T$), strategies like **UCB** (Upper Confidence Bound) and **Thompson Sampling** are used to balance exploration and exploitation efficiently.

### Possible Strategy
For each arm $i$, let $\hat{\mu}_i(t)$ be its sample mean reward after $n_i(t)$ pulls at time $t$. At each time step, choose the arm that maximizes

$$
I_i(t) = \hat{\mu}_i(t) + \frac{\sin\big(\omega t + \phi_i\big)}{n_i(t) + 1},
$$

where:

- $\omega > 0$ is a constant that sets the oscillation frequency.
- $\phi_i$ is a phase offset unique to arm $i$ (e.g., drawn randomly from $\text{Uniform}(0,2\pi)$).
- $n_i(t)$ is the number of times arm $i$ has been played up to time $t$.

**How it works:**

- **Exploration:** The sine term, $\sin(\omega t + \phi_i)$, moves between $-1$ and $1$ that gives each arm a periodic bonus for exploration.
- **Decaying bonus:** Dividing by $n_i(t) + 1$ makes sure that as an arm is played more often, so its exploration bonus gets smaller.
- **Long-run behavior:** Since the sine term moves around with an average of zero, the index $I_i(t)$ will eventually be mostly influenced by $\hat{\mu}_i(t)$ which causes the algorithm to focus more on choosing the best arms as time goes on.