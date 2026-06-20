# Euler–Maruyama Discretisation

## What is a Stochastic Differential Equation?

A **Stochastic Differential Equation (SDE)** describes how a quantity evolves over time when its evolution has both a predictable part and a random part. It generalises an ordinary differential equation (ODE) by adding noise.

The general form of an SDE is:

$$
dX_t = \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t
$$

where:

- $X_t$ — the quantity being modelled (e.g. a stock price, a particle's position)
- $\mu(X_t, t)$ — the **drift** term: the deterministic, "average" direction the system moves in
- $\sigma(X_t, t)$ — the **diffusion** term: how much randomness affects the system
- $dW_t$ — an increment of a **Wiener process** (Brownian motion), the source of randomness

If $\sigma = 0$, the equation collapses to an ordinary ODE. The $\sigma\,dW_t$ term is what makes solutions *random paths* rather than a single deterministic curve — every time the equation is "solved," a different trajectory is produced, governed by a probability distribution rather than one fixed answer.

**Why SDEs are harder than ODEs:** Brownian motion is nowhere differentiable, so $dW_t$ is not a derivative in the usual sense. This is handled by **Itô calculus**, which has its own chain rule (Itô's lemma), because ordinary calculus does not directly apply to stochastic processes. One consequence: most SDEs cannot be solved in closed form, so they are usually **simulated numerically** instead.

## The Euler–Maruyama Method

Euler–Maruyama is the simplest numerical method for simulating an SDE — it is the direct stochastic analogue of Euler's method for ODEs.

**The idea:** split time into small steps of size $\Delta t$, and at each step, approximate the drift and diffusion as locally constant:

$$
X_{n+1} = X_n + \mu(X_n, t_n)\,\Delta t + \sigma(X_n, t_n)\,\Delta W_n
$$

where the random increment is:

$$
\Delta W_n = \sqrt{\Delta t}\cdot Z_n, \qquad Z_n \sim \mathcal{N}(0,1)
$$

In words: at every step, move forward deterministically by the drift, then add a random kick whose size depends on $\sigma$ and on $\sqrt{\Delta t}$.

> **Why $\sqrt{\Delta t}$ and not $\Delta t$?**
> Brownian increments have variance proportional to $\Delta t$, so their standard deviation scales with $\sqrt{\Delta t}$, not $\Delta t$. This is a fundamentally different scaling from ordinary calculus, and it is the reason Euler–Maruyama behaves differently from ODE Euler.

## How accurate is it?

For ordinary ODEs, Euler's method has **strong order 1** — error shrinks proportionally to $\Delta t$.

For SDEs, Euler–Maruyama only achieves:

- **Strong order 0.5** — pathwise accuracy (how close one simulated path is to the true path) shrinks like $\sqrt{\Delta t}$
- **Weak order 1.0** — accuracy of statistics like the mean or variance, computed across many simulated paths, shrinks like $\Delta t$

This slower convergence is a direct consequence of Brownian motion's roughness. A more accurate (but more complex) alternative is the **Milstein method**, which adds a correction term and achieves strong order 1.

## Example: Geometric Brownian Motion (GBM)

GBM is the SDE used to model stock prices, and is the running example used to apply Euler–Maruyama in this project:

$$
dX_t = \mu X_t\,dt + \sigma X_t\,dW_t
$$

Substituting $\mu(X,t) = \mu X$ and $\sigma(X,t) = \sigma X$ into the general Euler–Maruyama formula gives:

$$
X_{n+1} = X_n + \mu X_n \,\Delta t + \sigma X_n \,\Delta W_n
$$

This is the update rule implemented in the accompanying code for this topic.
