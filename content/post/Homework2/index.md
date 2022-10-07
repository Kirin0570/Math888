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

One way [<sup>1</sup>](#2020) to formulate recommendation in the potential outcome framework is that:
- Characteristics: user preferences($\theta$) and item attributes($\beta$),
- Treatment($A$): whether a user used and rated the item,
- Outcome($y$): User's rating.

We are interested in the average causal effects between these two treatments. Since the assumption of ignorability does not hold in this context, we need to measure and control the confounders($w$).

The relationship between variables will be case by case, depending on the application problem we study. I wish to see the causal recommenders outperform correlation-based recommenders.

Above is a tip of the iceberg. Indeed, there are a bunch of problems in the context of recommender system that can be addressed with causal inference. In a recent survey [<sup>2</sup>](#survey), current works are categorized as follows:
- Data debasing with causal inference,
- Data augmentation and data denosing with causal inference.
- Achieving explainability, diversity, and fairness via interpretable and controllable recommendation based on causal inference.
This survey also discusses some open problems:
- Causal discovery for Recommenation,
- Causality-aware stable and robust recommendation,
- Causality-aware evaluation for recommendation,
- Causality-aware graph neural network-based recommendation,
- Causality-aware Simulator and environment for recommendation.

## References

<div id ="2020"></div>

- [1] [Causal Inference for Recommender Systems](https://dl.acm.org/doi/abs/10.1145/3383313.3412225)

<div id ="survey"></div>

- [2] [Causal Inference in Recommender Systems: A Survey and Future Directions]([https://dl.acm.org/doi/abs/10.1145/3383313.3412225](https://arxiv.org/abs/2208.12397))



