---
title: Popularity-bias Deconfounding (PD) - Conclusion

# Date published
date: '2022-12-12T00:00:00Z'

# Date updated
#lastmod: '2022-12-12T00:00:00Z'

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



In summary, we proposed a Popularity-bias Deconfounding framework based on backdoor adjustment to deconfound popularity bias for better recommendation. The new paradigm was demonstrated on the latent factor model and empirical studies on three real-world datasets. Unfortunately, we were unable to provide a good sensitivity analysis since the intervention is multi-level, which goes beyond our course. How to apply sensitivity analysis in the context of recommender system is a good question for future work. What's more, there are probably other factors other than popularity affecting the process of recommendation. We can try to apply similar idea to those factors to further improve the recommendation. On the one hand, people can directly implement this work in real-world recommender system to improve the recommendation. On the other hand,  this work provides a framework to deal with other factors in the process of recommendation.
