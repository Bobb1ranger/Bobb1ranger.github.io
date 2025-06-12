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
Different types of control on distributed systems. 
</div>


