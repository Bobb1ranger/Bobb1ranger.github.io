---
layout: page
title: Robust Mean Square Stability
description: study the interplay between multiplicative noise with model uncertainty
img: assets/img/RMSSpics/SISOInvertedPendulum.png
importance: 3
category: work
---

# *Abstract*:

In this paper, we formalize the Robust Mean Square Stability problem in the presence of deterministic LTI perturbations. In particular, we extend the robust stability notion of $$\mu$$ to include both deterministic and stochastic uncertainties and provide a computable sufficient condition that guarantees Robust Mean Square Stability of the closed-loop. We use an inverted pendulum example where the sensor is subject to link/attention dropout and find the trade-off between the amount of attention and fragility to model uncertainty expressed as the size of the disk margin.

More information can be found in our [ECC paper](https://doi.org/10.23919/ECC55457.2022.9838390).

# Problem Statement:

We are given a dynamical system that is partially modelled. The feedback interconnection with its controller is subject to packet drop.

* What is the largest variance can be tolerated in feedback channels when our system have bounded norm uncertainty in modeling?
* What is the failure mode (adversarial disturbance) that is most likely to destabilize a time-varying system with some stochasticity?

## System-modeling:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/RMSSpics/RMSS_model.png" title="example image" class="img-fluid rounded z-depth-1 w-50 mx-auto d-block" %}
    </div>
</div>
<div class="caption">
    Modeling unreliable channels and plant uncertainty.
</div>



