# Euler–Maruyama Discretisation

## Stochastic Differential Equation

A **Stochastic Differential Equation (SDE)** describes how a quantity evolves over time when its evolution has both a predictable part and a random part. It generalises an ordinary differential equation (ODE) by adding noise.

The general form of an SDE is:

$$
dX_t = \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t
$$

where:

- $X_t$ — the quantity being modelled (e.g. a stock price, a particle's position)
- $\mu(X_t, t)$ — the **drift** term: the deterministic, "average" direction the system moves in
- $\sigma(X_t, t)$ — the **diffusion** term: how much randomness affects the system
- $dW_t$ — an increment of a **Brownian motion**, the source of randomness

If $\sigma = 0$, the equation collapses to an ordinary ODE. The $\sigma\,dW_t$ term is what makes solutions *random paths* rather than a single deterministic curve — every time the equation is solved, a different trajectory is produced, governed by a probability distribution rather than one fixed answer.



## The Euler–Maruyama Method

Euler–Maruyama is the simplest numerical method for simulating an SDE — it is the direct stochastic analogue of Euler's method for ODEs.

It splits time into small steps of size $\Delta t$, and at each step, approximate the drift and diffusion as locally constant:

$$
X_{n+1} = X_n + \mu(X_n, t_n)\,\Delta t + \sigma(X_n, t_n)\,\Delta W_n
$$

where the random increment is:

$$
\Delta W_n = \sqrt{\Delta t}\cdot Z_n, \qquad Z_n \sim \mathcal{N}(0,1)
$$



