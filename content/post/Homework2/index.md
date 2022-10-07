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




