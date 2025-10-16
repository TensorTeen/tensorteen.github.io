---
title: "Gradient-Based Regularization for Inverse Airfoil Design"
collection: publications
permalink: /publication/2025-grad-reg-inv-design
excerpt: 'A gradient-regularized deep learning framework is proposed for inverse airfoil design, enforcing geometric smoothness through spatial derivatives while preserving aerodynamic accuracy.'
date: 2025-07-20
venue: 'Physics of Fluids, AIP Publishing'
---

### Abstract
Inverse airfoil design, a cornerstone in aerospace engineering, involves iterative modification of airfoil geometries to achieve specified aerodynamic performance. While recent advances in deep learning have enabled faster alternatives for airfoil shape predictions from pressure coefficient (Cp) distributions, a persistent challenge remains: existing deep-
learning-based models often generate airfoils with non-smooth geometries, manifesting as wiggles and kinks on the surface, that are physically nonviable for practical applications. Addressing this critical gap, we introduce a novel gradient-based regularization approach that incorporates first- and second-order spatial gradient terms into the loss function of deep neural networks. This regularization enforces geometric smoothness while preserving essential aero- dynamic characteristics. Results demonstrate that our method achieves a 13% reduction in geometric prediction error with first-order gradient regularization, and a 15.5% reduction when both first- and second-order terms are included, compared to baseline deep learning frameworks. Furthermore, airfoils generated yield pressure coefficient distributions with a 13% lower error relative to vanilla deep neural network (DNN) models, underscoring the aerodynamic fidelity of our approach. We further evaluate our models on unseen angles of attack (AoA), and show that within defined AoA limits, the framework reliably generates manufacturable and aerodynamically robust designs, extending the scope of our findings to applications in airfoil morphing use-cases.
