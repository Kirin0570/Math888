---
title: Causal Inference in Recommender Systems

# Date published
date: '2022-10-4T00:00:00Z'

# Date updated
#lastmod: '2022-10-4T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: ''
  focal_point: ''
  placement: 1
  preview_only: false

authors:
  - admin
---

In the era of the information overwhelming, recommender systems play a crucial role in information filtering and further affect people's cognition and decision-making. Existing recommender systems, like behavioral correlation in collaborative filtering, are mostly based on learning the correlation in data. However, the real world is driven by causality rather than correlation, and correlation does not imply causation. This inspires us to utilize causal inference to enhance the recommender system.

One way to formulate recommendation in the potential outcome framework is that:
- Characteristics: user preferences and item attributes,
- Treatment: whether a user used and rated the item,
- Outcome: User's rating.

We are interested in the average causal effects between these two treatments. Since the assumption of ignorability does not hold in this context, we need to measure and control the confounders.

The relationship between variables will be case by case, depending on the application problem we study. I wish to see the causal recommenders outperform correlation-based recommenders.




#### Homepage

I am a second-year Ph.D. student in the Department of Statistics at the University of Wisconsin-Madison. Before coming to Madison, I received my bachelor's degree from the School of Mathematical Sciences at Zhejiang University in 2020. I was doing a machine learning internship at Deepwise. Inc from 2020 to 2021.

I am broadly interested in the theory and methodology of causal inference and its application in genetics. I am interested in casual inference because it is an essential tool in epidemiology, health policy, education, sociology, and economics. The statistical problem of causal inference is also challenging and interesting.

This website page is a project report for Math/Stat 888(Causal Inference) at UW-Madison. I will present the paper [Double/debiased machine learning for treatment and structural parameters]([https://link-url-here.org](https://academic.oup.com/ectj/article/21/1/C1/5056401). This paper was published not long ago and is a great work showing how to incorporate modern methods like machine learning in causal inference.

#### Motivation

The causal inference problem can be formulated as a semi-parametric problem that treats the causal effect as target parameters and covariate effects as nuisance parameters. We depart from the classical setting by allowing nuisance parameters to be high-dimensional. Machine learning methods perform nuisance parameters well to estimatby employing regularization to reduce variance and trade-off regularization bias with overfitting. However, both regularization bias and overfitting cause a heavy bias in the estimation of the target parameter (the causal effect ) and make it fail to be $n^{\frac{1}{2}}$-consistent. This paper removes the impact by adding two simple yet critical ingredients to the estimation process. 

**Partially Linear Regression**

Now, we use the following partially linear regression to illustrate motivation clearly.

{{< math >}}
$$
\begin{gathered}
Y=D \theta_0+g_0(X)+U, \quad E[U \mid X, D]=0 \\
D=m_0(X)+V, \quad E[V \mid X]=0
\end{gathered}
$$
{{< math >}}

Here, $Y$ is the outcome variable, $D$ is the treatment variable, $X$ is a vector of confounding factors, and $U,V$ are disturbances. $\theta_0 $ is the target parameter (causal effect) that we would like to infer. $X$ affect the outcome via the treatment assignment via $m_0(X)$ and the outcome variable via the function $g_0(X)$. $(m_0, g_0)$ are the nuisance parameters. The dimension of $X$ is large relative to the sample size $N$ and we allow it to increase with $N$.

**Regularization bias**

To estimate $\theta_0$ using ML methods, we first split our sample into the auxiliary part and the main part. We can construct a sophisticated ML estimator $D\hat{\theta}_0+\hat{g}_0(X)$,  learning the regression function using auxiliary data, and obtaining $\hat{g}_0$. The final estimate of $\theta_0$ is obtained using the main sample:

{{< math >}}
$$
\hat{\theta}_0=(\frac{1}{n} \sum_{i \in I} D_i^2)^{-1} \frac{1}{n} \sum_{i \in I} D_i(Y_i-\hat{g}_0(X_i)) .
$$
{{< math >}}

The estimator $\hat{\theta}_0$ will generally have a rate of convergence slower than $n^{-\frac{1}{2}}$. As detailed below, the driving force behind this inferior behavior is the bias in learning $\hat{g}_0$.

We decompose the scaled estimation error as 

{{< math >}}
$$
\sqrt{n}\left(\hat{\theta}_0-\theta_0\right)=\underbrace{\left(\frac{1}{n} \sum_{i \in I} D_i^2\right)^{-1} \frac{1}{\sqrt{n}} \sum_{i \in I} D_i U_i}_{:=a}+\underbrace{\left(\frac{1}{n} \sum_{i \in I} D_i^2\right)^{-1} \frac{1}{\sqrt{n}} \sum_{i \in I} D_i\left(g_0\left(X_i\right)-\hat{g}_0\left(X_i\right)\right)}_{:=b} .
$$
{{< math >}}

The first term is well-behaved under mild conditions, obeying asyptotically normal. The second term is the regularization bias term, which is not centered and diverges in general. Indeed, we have 

$$
b=\left(E\left[D_i^2\right]\right)^{-1} \frac{1}{\sqrt{n}} \sum_{i \in I} m_0\left(X_i\right)\left(g_0\left(X_i\right)-\hat{g}_0\left(X_i\right)\right)+o_P(1)
$$

It is the sum of $n$ terms that do not have a mean zero, $m_0\left(X_i\right)\left(g_0\left(X_i\right)-\hat{g}_0\left(X_i\right)\right)$. These terms have non-zero mean because, in high-dimensional settings, we must employ regularized estimators to estimate $g_0$. The regularization keeps the variance from exploding but necessarily induces bias in the estimation of $g_0$.

This paper removes the bias by (1) using Neyman-orthogonal moments/scores that have reduced sensitivity with respect to nuisance parameters to estimate causal inference; (2) making use of cross-fitting, which provides an efficient form of data-splitting. They call the new set of methods debiased machine learning method (DML).  They also show that the generic statistical theory of DML is elementary and simultaneously relies on only weak theoretical requirements.
