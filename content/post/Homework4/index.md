---
title: Popularity-bias Deconfounding (PD) - Estimation 

# Date published
date: '2022-11-14T00:00:00Z'

# Date updated
#lastmod: '2022-25-4T00:00:00Z'

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

## Estimation method

Last time, we have identified the followiing causal effect.
{{< math >}}
$$
\mathbb{E}\left[ C(i)\middle| U=u \right]=\mathbb{E}\left\{\mathbb{E}\left[ C\middle|I=i,U=u,Z \right]\right\}.
$$
{{< /math >}}

To estimate the statistical estimand on the RHS, we first estimate $\mathbb{E}\left[ C\middle|I=i,U=u,Z \right]$. This can be down with some traditional recommendation training method. For example, optimizing the pairwise BPR objective function on historical data $\mathcal{D}$:
{{< math >}}
$$
\begin{aligned}
  \max_\Theta \sum_{(u,i,j)\in\mathcal{D}} \log \sigma ( \mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z=m_i^t \right] \\
  - \mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z=m_j^t \right] ) - \lambda\left\Vert \Theta \right\Vert_2^2,
\end{aligned}
$$
{{< /math >}}
where $\Theta$ denotes the parameters modeling $\mathbb{E}\left[ C\middle|I=i,U=u,Z \right]$, $j$ denotes the negative sample for $u$, $\sigma (\cdot)$ is the sigmoid function.

It remains to show how to parameterize {{< math >}}$\mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z=m_i^t \right]${{< /math >}} specifically. To decouple the user-item match with item popularity, we design the model as:
{{< math >}}
$$
\mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z=m_i^t \right] = ELU'(f_\Theta (u,i))\times (m_i^t)^\gamma,
$$
{{< /math >}}
where $f_\Theta (u,i)$ can be any user-item mathcing model and we choose the simple _Matrix Factorization_ (MF) here; hyperparameter $\gamma$ is to control the strenth of conformity effect. $ELU'(\cdot)$ is a variant of the Exponential Unit active function that ensures the positivity of the matching score:
{{< math >}}
$$
\begin{equation*}
ELU'(x)=
    \begin{cases}
        e^x & \text{if } x \leq 0\\
        x+1 & \text{else }.
    \end{cases}
\end{equation*}
$$
{{< /math >}}

Lastly, since we are only interested in the rank of items, we do not need to normalize the estimation to make it a rigorous probability.

Now we move forward to estimate {{< math >}}$\mathbb{E}\left\{\mathbb{E}\left[ C\middle|I=i,U=u,Z \right]\right\}${{< /math >}}. Plug in the model for {{< math >}}$\mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z \right]${{< /math >}}. 

{{< math >}}
$$
\begin{align}
    \mathbb{E}\left\{\mathbb{E}\left[ C\middle|I=i,U=u,Z \right]\right\} & = \mathbb{E}\left\{\mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z \right]\right\} \\
    & = ELU'(f_\Theta (u,i))\times \mathbb{E}\left[Z^\gamma\right].
\end{align}
$$
{{< /math >}}

Notice that {{< math >}}$ \mathbb{E}\left[Z^\gamma\right] ${{< /math >}} is a constant. Ignoring it will not affect ranking. Hence, we can simply use {{< math >}}$ ELU'(f_\Theta (u,i)) ${{< /math >}} to estimate {{< math >}}$\mathbb{E}\left[ C(i)\middle| U=u \right]${{< /math >}}.

To summarize, we fit the historical interaction data with {{< math >}}$\mathbb{E}_\Theta \left[ C\middle|I=i,U=u,Z=m_i^t \right]${{< /math >}}, and use the user-item matching component {{< math >}}$ ELU'(f_\Theta (u,i)) ${{< /math >}} for deconfounded ranking. We name this method as Popularity-bias Deconfounding (PD). Combined with cross-validation, we have the following algorithm.

![algo1](algo1.PNG)

## Datasets

In this article, experiments are conducted on three datasets. We summarize them in the following table.

|  dataset   | output type  | size |
|  :----:  | :----:  | :----:  |
| Kwai  | clicking | 7,658,510 interactions between 37,663 users and 128,879 items |
| Douban Movie  | rating | 7,174,218 interactions between 47,890 users and 26,047 items |
| Tencent | likes | 1,816,046 interactions between 80,339 users and 27,070 items |

From the table above, we can see that although we call $C$ "clicking", it is not necessary to be 0-1 value. It can be integer-valued or continuously valued rating. That is why this blog use the notation $\mathbb{E}[C\ldots]$ instead of $\mathbb{P}(C=1\ldots)$ as in the orinal paper. 


## References

<div id ="PDA"></div>

- [1] [Causal Intervention for Leveraging Popularity Bias in Recommendation](https://arxiv.org/abs/2105.06067v1)
