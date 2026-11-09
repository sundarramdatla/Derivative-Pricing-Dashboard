# Derivative Pricing using Finite Difference and Monte Carlo Simulation

**[View Model in Browser](https://sundarramdatla.github.io/Derivative-Pricing-Model/)**

## 1. Introduction

Under the Black Scholes model, derivative prices satisfy a partial differential equation (PDE) derived from arbitrage-free pricing and delta hedging:

$$
\frac{\partial V}{\partial t}
+\frac{1}{2}\sigma^2S^2\frac{\partial^2V}{\partial S^2}
+rS\frac{\partial V}{\partial S}
-rV=0
$$

Solving this equation with appropriate terminal payoffs and boundary conditions determines the derivative's fair value \(V(t,S)\) at time \(t\) and underlying stock price \(S\).

My model implements two numerical methodologies and evaluates their accuracy against analytical Black Scholes solutions.

## 2. Explicit Finite Difference PDE Solver

The Black Scholes PDE is discretized over a 2D stock price and time mesh using central finite differences for spatial derivatives and explicit backward time stepping:

$$
\frac{\partial V}{\partial S}\approx
\frac{V_{i+1}^j-V_{i-1}^j}{2\Delta S},
\qquad
\frac{\partial^2V}{\partial S^2}\approx
\frac{V_{i+1}^j-2V_i^j+V_{i-1}^j}{\Delta S^2}
$$

The solver uses derivative payoff functions as boundary conditions. For example, a European put satisfies:

$$
V(T,S)=\max(K-S,0),\qquad
V(t,0)=Ke^{-r(T-t)}
$$

The numerical solution is propagated backward from maturity to \(t=0\), with time step size restrictions imposed for numerical stability.

## 3. Monte Carlo Simulation

The Fundamental Theorem of Asset Pricing and Feynman–Kac theorem establish the risk-neutral valuation relationship:

$$
V(0,S_0)=e^{-rT}\mathbb{E}^{\mathbb Q}[\Phi]
$$

Under the risk neutral measure, stock prices follow geometric Brownian motion:

$$
S_{t+\Delta t}=S_t\exp\left[
\left(r-\frac{\sigma^2}{2}\right)\Delta t
+\sigma\sqrt{\Delta t}Z
\right],\qquad Z\sim\mathcal N(0,1)
$$

This expected value can be found by the Strong Law of Large Numbers by simulating N geometric Brownian motion paths of the stock, taking the final stock price and calculating the derivative payoff, then averaging the sample of payoffs.
As \(N\to\infty\), this sample mean converges to the true expected value of the payoff distribution.

The model simulates 10,000 independent stock price paths over a one year maturity, calculates vanilla, digital, and path dependent barrier option payoffs, and discounts their sample average to obtain the present value:

$$
V_{\mathrm{MC}}=e^{-rT}\frac{1}{M}\sum_{i=1}^{M}\Phi^{(i)}
$$



## 4. Model Validation and Interactive Features

The application enables users to adjust pricing parameters, visualize Monte Carlo stock trajectories and PDE pricing surfaces, and compare numerical results against analytical solutions.

Additional validation includes **European put–call parity** and **barrier in–out parity**:

$$
C-P=S_0-Ke^{-rT}
$$

$$
V_{\mathrm{Knock\text{-}In}}+V_{\mathrm{Knock\text{-}Out}}=V_{\mathrm{Vanilla}}
$$

These relationships provide consistency checks across the numerical pricing models.

---

**Developed by Sundarram Datla**
