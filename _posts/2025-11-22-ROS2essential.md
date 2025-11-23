---
layout: post
title: ROS 2 Essentials
date: 2025-11-22 21:59:00-0500
description: How ROS 2 is different from ROS.
tags: robotics, simulation, programming.
related_posts: true
related_posts_list:
  - "/blog/2025/Mujoco_notes/"
---

A quick Recap on why ROS 2 is invented to improve upon ROS.

# I Why ROS 2

| Feature               | ROS                         | ROS 2                           |
|-----------------------|-----------------------------|----------------------------------|
| Computational Resources | Strong, local resources     | Potentially limited locally       |
| Number of Agents       | Few agents                   | Hundreds of agents                |
| Network Reliability    | Stable and reliable          | Not always reliable               |
| Application            | Research-oriented            | Commercial and industrial use     |

---

## 1. Limitations of ROS:
* ROS is no longer used exclusively for scientific research.
* More ROS-based products are being commercialized.
* The increasing variety of application scenarios introduces additional requirements for the system.

## 2. Various demands in the era of intelligent robotics:
- Streamlined methods for multi-robot systems
- heterogeneous system compatibility
- Real-time performance capabilities
- Robustness to changing network conditions
- Better suitability for commercial products
- Lifetime project management

---

# II Main differences

## 1. A revolution on architecture

In ROS, all nodes are managed by a single node called "Master". This increases the risk of single point failure. In ROS2, the discovery mechanism based on DDS mitigates the risk.

* Date Distribution Service (DDS): A middleware standard that enables real-time, decentralized communication between applications.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/Blogimgs1/ROS1-ROS2-architecture.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The architectures of ROS and ROS 2.
</div>

## 2. Re-design of the API

ROS 2 adopts the latest C++ standards and Python 3 while retaining most of the familiar usage patterns.

## 3. Build-process Upgrade

In ROS 2, the build process is based on ament and colcon, replacing the older rosbuild and catkin systems used in ROS 1. Ament provides a more modular and extensible build framework, while colcon acts as a higher-level build tool that supports parallel builds, better dependency handling, and cleaner workspace management. Together, they offer a more reliable and scalable workflow, especially for large, multi-package ROS projects.


