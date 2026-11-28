---
layout: page
title: RoboBuilder
permalink: /projects/robobuilder/
description: A benchmark for AI agents that design robot hardware and control programs, then improve their designs using video from physical tests.
category: work
importance: 0
featured: true
img: https://ghadinehme.com/robobuilder.github.io/assets/img/hero/steps-l2-astra.jpg
---

Ghadi Nehme, Dahyun Nam, **Akshay Govind Srinivasan**, and Faez Ahmed  
MIT DeCoDE Lab

[Paper](https://arxiv.org/abs/2611.23855) · [Code](https://github.com/ghadinehme/RoboBuilder) · [Dataset](https://huggingface.co/datasets/DeCoDELab/RoboBuilder) · [Interactive demo](https://ghadinehme.com/robobuilder.github.io/)

<figure class="my-4">
  <img class="img-fluid rounded" src="{{ page.img | escape }}" alt="A small wheeled RoboBuilder robot beside a stair obstacle on a physical test course." loading="lazy" decoding="async">
  <figcaption class="caption">
    A physical obstacle test. Image from the
    <a href="https://ghadinehme.com/robobuilder.github.io/">RoboBuilder project site</a>
    (<a href="https://creativecommons.org/licenses/by-sa/4.0/">CC BY-SA 4.0</a>).
  </figcaption>
</figure>

## How it works

Agents design a robot and write its motor program using standard parts, motors, and one custom 3D-printed component. The team built 135 robots during the study and tested them on physical obstacle courses. When a robot that passed in simulation failed on the course, the agent could use video of the run to revise the design or update the simulator.

## What we found

Across the benchmark, 66% of simulated passes also succeeded on the physical course. With video feedback, agents resolved 72% of real-world failures on the two easier difficulty levels. The [interactive demo](https://ghadinehme.com/robobuilder.github.io/#explorer) lets you explore individual designs and compare simulated and physical runs.
