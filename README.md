Here’s a clean, well-structured **README-ready** version of your derivation, formatted in Markdown and written clearly for GitHub documentation.

---

# 📘 Compound Poisson Process — Derivation and Explanation

This document provides a clear derivation of the distribution of a **Compound Poisson Process**:

[
S(t) = \sum_{i=1}^{N(t)} X_i
]

where

* (N(t)) is a Poisson counting process with rate (\lambda),
* (X_i) are i.i.d. claim sizes with (X_i \sim \text{Exp}(\mu)).

---

## 🔢 1. Counting Process Distribution

Because the interarrival times follow an exponential distribution (\text{Exp}(\lambda)), the number of arrivals in time (t) follows a **Poisson distribution**:

[
P(N(t) = k) = e^{-\lambda t} \frac{(\lambda t)^k}{k!}, \quad k = 0, 1, 2, \dots
]

---

## 📈 2. Moment Generating Function (MGF) of (S(t))

A fundamental tool for deriving the distribution of a Compound Poisson Process is its **moment generating function (MGF)**:

[
M_{S(t)}(\theta) = \exp\left{ \lambda t\left[ M_X(\theta) - 1 \right] \right}.
]

Since (X \sim \text{Exp}(\mu)), its MGF is:

[
M_X(\theta) = \frac{\mu}{\mu - \theta}, \qquad \theta < \mu.
]

Substituting this into the MGF of the compound process:

[
\begin{aligned}
M_{S(t)}(\theta)
&= \exp\left{ \lambda t\left[ \frac{\mu}{\mu - \theta} - 1 \right] \right} \
&= \exp\left{ \lambda t \left[ \frac{\theta}{\mu - \theta} \right] \right}.
\end{aligned}
]

Thus, the MGF of (S(t)) is:

[
\boxed{ M_{S(t)}(\theta) = \exp\left( \frac{\lambda t , \theta}{\mu - \theta} \right) }.
]

---

## 📦 3. Distribution of the Compound Sum (S(t))

The distribution of (S(t)) is **not a standard named distribution**, but can be expressed through the law of total probability:

[
P(S(t) \le s) = \sum_{k=0}^{\infty} P(S(t) \le s \mid N(t)=k), P(N(t)=k).
]

### Case: (k = 0)

If no arrivals occur, (S(t) = 0):

[
P(S(t) = 0) = e^{-\lambda t}.
]

### Case: (k > 0)

Given (N(t)=k):

[
S(t) = \sum_{i=1}^k X_i.
]

Because each (X_i \sim \text{Exp}(\mu)), the sum is **Gamma distributed**:

[
S(t) \mid N(t)=k \sim \text{Gamma}(k, \mu).
]

The Gamma PDF for shape (k) and rate (\mu) is:

[
f(s \mid N(t)=k) = \frac{\mu^k s^{k-1} e^{-\mu s}}{(k-1)!}, \qquad s>0.
]

---

## 📊 Final PDF of (S(t))

The full density is a **mixture** of a point mass at zero and an infinite weighted sum of Gamma densities:

[
f_{S(t)}(s) =
\begin{cases}
e^{-\lambda t}, & s = 0, [6pt]
\displaystyle
\sum_{k=1}^{\infty}
e^{-\lambda t} \frac{(\lambda t)^k}{k!}
\cdot
\frac{\mu^k s^{k-1} e^{-\mu s}}{(k-1)!},
& s > 0.
\end{cases}
]

This is known as the **Compound Poisson–Exponential distribution**.

---

If you'd like, I can also add:

✅ A visualization section (plots)
✅ Example simulation code (Python / R)
✅ A table of moments (E[S(t)]), (\text{Var}(S(t)))
✅ A “Usage” section for GitHub

Just tell me!
