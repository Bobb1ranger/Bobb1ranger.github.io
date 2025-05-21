---
layout: page
title: Network distributed control
description: Study how to design multiple controllers cooperating by communication over a network to better regulate a networked plant. 
img: assets/img/3nodesys_withdelayedcontrol.png
importance: 1
category: work
related_publications: true
---

The control theory of cyber-physical systems is important and applicable to many real-world scenarios like Vehicle-to-Vehicle communications, power grids and water treatment plants for instance. Network distributed control, which allow subcontrollers to make decisions based on limited local information, offers a suitable compromise for improving performance and managing computational load between decentralized and centralized control in cyber-physical systems. Designing a network distributed controller is hard due to the implicit constraints from network structure. The purpose of this project is to find computationally efficient methodology for optimal network distributed controller design and to gain a deeper understanding of the underlying nature of network distributed control.

The project focuses on the design of Quadratic Invariance optimal distributed controllers for linear-time-invariant plants. In this case, the current consensus on distributed controller design procedure is to first develop an integrated controller that reflects the information flow patterns of the communication network. Afterwards, a distributed implementation of that controller over the network needs to be found.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NWDCpics/Centralized_control.png" title="Centralized control" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NWDCpics/Decentralized_control.png" title="Decentralized control" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/NWDCpics/Distributed_control.png" title="Distributed control" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
