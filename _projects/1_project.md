---
layout: page
title: Adaptive and Interpretable Mesh-Free Neural Solvers
description: Accelerating Stiff PDE Resolution via Kernel-Adaptive Physics-Informed Extreme Learning Machines (PI-ELMs)
img: assets/img/gyroid_geometry.jpg
importance: 1
category: work
related_publications:
    srinivasan2025deep
    srinivasan2026towards
    srinivasan2026learning
    srinivasan2025towards
---

Partial Differential Equations (PDEs) serve as the bedrock for modeling physical phenomena across engineering domains. While classical discretization approaches like FEM, FDM, and FVM are mathematically rigorous, they suffer from intensive mesh-generation bottlenecks in complex 3D structures. Physics-Informed Neural Networks (PINNs) offer a mesh-free path forward, yet they remain bottlenecked by slow iterative training, an uninterpretable "black-box" architecture, and volatile hyperparameter sensitivity. 

This project showcases **RBF-PIELM** and **GMM-PIELM**, which synergize the physics-constrained optimization of PINNs with the lightning-fast, non-iterative linear solving paradigm of Extreme Learning Machines (ELMs). By substituting global activation functions with localized Gaussian kernels, we enable physics-informed initialization and dynamic, error-driven resource allocation.

---

## Core Architectural Framework

Standard PIELMs rely on physics-agnostic, random input parameters, leaving hidden features completely uninterpretable. The **Radial Basis Function-based PIELM (RBF-PIELM)** addresses this by anchoring Gaussian kernels directly inside the computational domain:

$$\phi_{i}(x;x_{0}^{(i)},\sigma_{i}) = \exp\left(-\frac{||x-x_{0}^{(i)}||^{2}}{2\sigma_{i}^{2}}\right)$$

To resolve highly localized stiff dynamics where manual heuristics fail, the **Gaussian Mixture Model Adaptive PIELM (GMM-PIELM)** treats the unnormalized $\log(1+|residual|)$ field as a spatial probability density function (PDF) of the error profile. An Expectation-Maximization (EM) algorithm dynamically drives RBF centers to cluster closely within critical high-gradient zones, such as thin boundary layers.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/fig_1_single_nu_1e4_1d.jpg" title="" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/fig_2_single_nu_1e4_1d.jpg" title="GMM Learned Error Density" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
 Left: Solution profile of GMM-PIELM on the 1D convection-diffusion equation with single boundary layer. Right: The learned GMM probability density successfully mirroring high-frequency error landscapes across a stiff boundary layer.
</div>

---

## Fluid Mechanics Benchmarks: Lid-Driven Cavity & Backstep Flow

We benchmarked the RBF-PIELM framework against traditional PINNs on canonical incompressible fluid flow problems modeled via the stream function-vorticity (biharmonic) formulation:

$$\psi_{xxxx} + 2\psi_{xxyy} + \psi_{yyyy} = 0$$

Numerical experiments demonstrate that our mesh-free solver computes fluid structures accurately while crushing the training overhead of traditional networks. 

* **Extreme Acceleration:** RBF-PIELM achieves comparable accuracy while executing **350x to 658x faster** than PINNs.
* **Sustainable Computing:** Our non-iterative linear solve delivers a **2,000x to 3,800x reduction in energy consumption**, cutting carbon footprints down from grams to mere milligrams of $CO_2$ per run.
* **Parametric Efficiency:** Achieved lower physical residual error while using up to **13.2x fewer parameters**.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/pinn_backstep.jpg" title="PINN Flow Prediction" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/pielm-backstep-heatmap.jpg" title="RBF-PIELM Flow Prediction" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparative flow structure capturing in backstep geometry. While both models capture primary separation bubbles and recirculation zones, RBF-PIELM maps the fields effortlessly in a fraction of a second on standard CPU hardware.
</div>

---

## Scaling to Complex Geometry & High-Dimensional Finance

Beyond 2D fluid benchmarks, we validated the geometric versatility of the solver on highly complex **3D Gyroidal Minimal Surfaces** governed by a Poisson system:

$$\frac{\partial^2 u}{\partial x^2} + \frac{\partial^2 u}{\partial y^2} + \frac{\partial^2 u}{\partial z^2} = R(x,y,z)$$

Despite the highly convoluted, interconnected microfluidic channels, RBF-PIELM evaluates the full 3D domain space in just **6.88 seconds**.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/rbf_pielm_comparison.jpg" title="Gyroid Computational Domain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The complex triply periodic minimal surface of the 3D Gyroidal domain used to validate coordinate scaling and boundary resilience.
</div>

### Derivative Pricing via Black-Scholes and Heston-Hull-White Systems
In quantitative finance, the framework was adapted to circumvent the "curse of dimensionality" when evaluating complex multi-factor derivatives. We modeled options under the high-dimensional **Heston-Hull-White (HHW) framework**, which introduces simultaneous stochastic volatility and stochastic short interest rates:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \kappa_v(\theta_v - v)\frac{\partial V}{\partial v} + \kappa_r(\theta_r - r)\frac{\partial V}{\partial r} + \dots - rV = 0$$

Our approach maps combined stochastic fluctuations **32x faster than PINNs**, allowing for near real-time financial derivative discounting and risk assessment.

---

## Solving the Inverse Problem: Real-Time Parameter Calibration

For practical market integration, a quantitative model must undergo fast calibration—recovering unobservable parameters (e.g., long-term volatility mean, mean-reversion speeds) from noisy market prices. Because optimization loops solve the forward PDE thousands of times, iterative neural methods are too slow for production settings.

By implementing a **Teacher-Student validation protocol**, we paired our fast linear solver with a Bayesian Optimization loop. Synthetic pricing surfaces generated by a high-capacity teacher network were intentionally corrupted with high-variance multiplicative noise ($\eta = 10\%$) to simulate chaotic real-world spreads. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/parameter_recovery_error.jpg" title="Parameter Recovery Chart" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Relative parameter recovery error (%) across rising levels of artificial market noise. The student PIELM successfully filters out intense noise variance to recover the true asset volatility and risk-free rates with high numerical stability.
</div>

Our framework accurately reconstructs the underlying price manifold while keeping calibration parameter errors under safe margins, proving its viability for real-time risk evaluation workflows.
