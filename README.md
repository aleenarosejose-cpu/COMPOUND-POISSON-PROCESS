Here is the derivation and discussion, along with a suggested plan for the R Shiny application.

-----

## 🧐 Derivation of the Compound Process Distribution

The compound process $S(t)$ is defined as:
$$S(t) = \sum_{i=1}^{N(t)} X_i$$
where:

  * $N(t)$ is the counting process for the number of events (arrivals) up to time $t$.
  * $X_i$ is the magnitude (or 'claim size') of the $i$-th event.

### 1\. The Counting Process $N(t)$

Given that the interarrival times are **exponentially distributed**, the counting process $N(t)$ is a **Poisson process**.

  * Let the interarrival times $T_i$ be $\text{Exponential}(\lambda_N)$.
  * The number of arrivals $N(t)$ follows a **Poisson distribution** with rate $\lambda_N$:
    $$P(N(t) = n) = \frac{e^{-\lambda_N t} (\lambda_N t)^n}{n!}$$

### 2\. The Claim Size $X_i$

Given that the claim sizes $X_i$'s are **independent and identically distributed (i.i.d.) exponential variables**, and they are independent of $N(t)$.

  * Let $X_i \sim \text{Exponential}(\lambda_X)$.
  * The probability density function (PDF) is $f_X(x) = \lambda_X e^{-\lambda_X x}$ for $x>0$.
  * The Moment Generating Function (MGF) is $M_X(\theta) = E[e^{\theta X_i}] = \frac{\lambda_X}{\lambda_X - \theta}$, for $\theta < \lambda_X$.

### 3\. The Compound Process $S(t)$

The Moment Generating Function (MGF) is the easiest way to derive the distribution of a compound Poisson process.

  * The MGF of the compound process $S(t)$ is given by:
    $$M_{S(t)}(\theta) = E[e^{\theta S(t)}] = E\left[ E\left[\left. e^{\theta \sum_{i=1}^{N(t)} X_i} \right| N(t) \right] \right]$$
  * Since the $X_i$'s are i.i.d. and independent of $N(t)$:
    $$E\left[\left. e^{\theta \sum_{i=1}^{N(t)} X_i} \right| N(t)=n \right] = \left( M_X(\theta) \right)^n$$
  * Substituting this into the MGF of $S(t)$:
    $$M_{S(t)}(\theta) = \sum_{n=0}^{\infty} \left( M_X(\theta) \right)^n P(N(t)=n)$$
    $$M_{S(t)}(\theta) = \sum_{n=0}^{\infty} \left( \frac{\lambda_X}{\lambda_X - \theta} \right)^n \frac{e^{-\lambda_N t} (\lambda_N t)^n}{n!}$$
    $$M_{S(t)}(\theta) = e^{-\lambda_N t} \sum_{n=0}^{\infty} \frac{1}{n!} \left( \lambda_N t \frac{\lambda_X}{\lambda_X - \theta} \right)^n$$
  * Using the Taylor expansion for $e^z = \sum_{n=0}^{\infty} \frac{z^n}{n!}$, where $z = \lambda_N t \frac{\lambda_X}{\lambda_X - \theta}$:
    $$M_{S(t)}(\theta) = e^{-\lambda_N t} \exp \left( \lambda_N t \frac{\lambda_X}{\lambda_X - \theta} \right)$$
    $$M_{S(t)}(\theta) = \exp \left( \lambda_N t \left( \frac{\lambda_X}{\lambda_X - \theta} - 1 \right) \right)$$
    $$M_{S(t)}(\theta) = \exp \left( \lambda_N t \left( \frac{\lambda_X - (\lambda_X - \theta)}{\lambda_X - \theta} \right) \right)$$
    $$M_{S(t)}(\theta) = \exp \left( \lambda_N t \frac{\theta}{\lambda_X - \theta} \right)$$

This is the MGF of the compound process $S(t)$. **There is no simple, named distribution** (like Gaussian or Gamma) that corresponds directly to this MGF. Its density function can be found by inversion, but it is typically expressed as a convolution (a weighted sum of Gamma distributions):

$$f_{S(t)}(s) = \sum_{n=1}^{\infty} P(N(t)=n) f_{X_1+\dots+X_n}(s) + P(N(t)=0) \delta(s)$$
where:

  * $P(N(t)=n)$ is the Poisson probability.
  * $f_{X_1+\dots+X_n}(s)$ is the $\text{Gamma}(n, \lambda_X)$ PDF (since the sum of $n$ i.i.d. $\text{Exponential}(\lambda_X)$ variables is a $\text{Gamma}(n, \lambda_X)$ distribution).
  * $\delta(s)$ is the Dirac delta function at $s=0$ (representing $S(t)=0$ when $N(t)=0$).

-----

## 📈 Simulation and The Central Limit Theorem

### Histogram Plotting ($t=10, 100, 1000, 10000$)

While I cannot run the simulation, the theoretical expectation for the distribution of $S(t)$ as $t \to \infty$ is based on the **Central Limit Theorem (CLT)**.

  * As $t$ increases, the expected number of arrivals, $E[N(t)] = \lambda_N t$, also increases.
  * $S(t)$ is the sum of a *random number* of i.i.d. random variables.
  * When the expected number of terms $\lambda_N t$ is large, the distribution of $S(t)$ will be approximately **Normal (Gaussian)**.

The expectation and variance are:

  * $E[S(t)] = E[N(t)] E[X_i] = (\lambda_N t) \left( \frac{1}{\lambda_X} \right) = \frac{\lambda_N t}{\lambda_X}$
  * $\text{Var}[S(t)] = E[N(t)] \text{Var}[X_i] + \text{Var}[N(t)] (E[X_i])^2$
      * $\text{Var}[X_i] = 1/\lambda_X^2$
      * $\text{Var}[N(t)] = \lambda_N t$
      * $$\text{Var}[S(t)] = (\lambda_N t) \left( \frac{1}{\lambda_X^2} \right) + (\lambda_N t) \left( \frac{1}{\lambda_X} \right)^2 = 2 \frac{\lambda_N t}{\lambda_X^2}$$

As $t$ increases, the histograms for $S(t)$ should transition from a right-skewed distribution (a weighted sum of Gamma) towards a **symmetric bell-shaped Normal distribution** with mean $\frac{\lambda_N t}{\lambda_X}$ and variance $2 \frac{\lambda_N t}{\lambda_X^2}$.

-----

## 💡 Discussion on Parameter Impact

There are two key parameters: $\lambda_N$ (interarrival rate) and $\lambda_X$ (claim size rate).

| Parameter | Interpretation | Impact on $S(t)$ (Expected Value $E[S(t)]$ and Spread $\text{Var}[S(t)]$) |
| :--- | :--- | :--- |
| **$\lambda_N$** | **Arrival Rate:** The frequency of events/claims. (Mean interarrival time is $1/\lambda_N$). | **Direct Proportionality to $t$**: Increasing $\lambda_N$ makes events happen more often. $E[S(t)]$ and $\text{Var}[S(t)]$ both increase **linearly** with $\lambda_N$. |
| **$\lambda_X$** | **Claim Size Rate:** Governs the magnitude of individual claims. (Mean claim size is $1/\lambda_X$). | **Inverse Relationship:** Increasing $\lambda_X$ means the *average claim size* ($1/\lambda_X$) gets **smaller**. This reduces $E[S(t)]$ and $\text{Var}[S(t)]$. |

### Summary of Sensitivity

  * **Impact of $\lambda_N$:** Primarily controls the **scale** and **growth rate** of $S(t)$ over time. A small increase in $\lambda_N$ leads to a proportional increase in the overall risk.
  * **Impact of $\lambda_X$:** Controls the **unit size** of the claims. Changes in $\lambda_X$ have a more complex impact: a small $\lambda_X$ (large claims) will increase the variance much more significantly than the mean, increasing the chance of extreme outcomes.

-----

## 💻 R Shiny Deployment Plan

The R Shiny application would be the ideal way to demonstrate the sensitivity of $\lambda_N$ and $\lambda_X$.

### I. `ui.R` (User Interface)

  * **Sliders** for input parameters:
      * $\lambda_N$: Arrival Rate (e.g., from 0.1 to 5)
      * $\lambda_X$: Claim Size Rate (e.g., from 0.1 to 5)
      * $t_{\max}$: Maximum Time for simulation (e.g., from 1 to 100)
      * $N_{\text{sim}}$: Number of simulations (e.g., 1000 to 10000)
  * **Radio buttons/Checkboxes** to select plots:
      * **Plot 1:** $S(t)$ vs. Time (for a few individual paths)
      * **Plot 2:** Histogram of $S(t)$ at a fixed time $t^*$

### II. `server.R` (Backend Logic)

1.  **Simulation Function:** A function to simulate one path of $S(t)$ over the interval $[0, t_{\max}]$.
      * **Step A:** Simulate interarrival times $T_i \sim \text{Exponential}(\lambda_N)$.
      * **Step B:** Determine the arrival times $A_n = \sum_{i=1}^{n} T_i$.
      * **Step C:** Simulate claim sizes $X_i \sim \text{Exponential}(\lambda_X)$.
      * **Step D:** Calculate $S(t)$ at all arrival times.
2.  **Path Visualization:** Use the simulation function to plot a few (e.g., 5-10) sample paths of $S(t)$ over time.
3.  **Distribution Visualization (Histogram):**
      * Run the simulation function $N_{\text{sim}}$ times up to the user-selected time $t^*$.
      * Store the final values $S(t^*)$.
      * Plot a **histogram** of the $N_{\text{sim}}$ values of $S(t^*)$.
      * Overlay the theoretical $\text{Normal}$ distribution (based on CLT) with mean $\frac{\lambda_N t^*}{\lambda_X}$ and variance $2 \frac{\lambda_N t^*}{\lambda_X^2}$ to show convergence.


## ⚙️ Installation and Usage

### Prerequisites

You need R and the following packages installed:

```r
install.packages(c("shiny", "ggplot2"))
```

### Running the App

1.  Clone this repository:
    ```bash
    git clone [Your-Repo-Link]
    cd [Your-Repo-Name]
    ```
2.  Run the application in R:
    ```r
    library(shiny)
    runApp()
    ```
