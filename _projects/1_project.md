---
layout: page
title: Dsalt Anillo ACT210087
description: Computational mathematics for desalination processes 
img: assets/img/Dsalt_project.jpeg
importance: 1
category: work
related_publications: true
---
The main goal of the project is to develop new computational algorithms to simulate seawater desalination processes. These advancements aim to provide technology developers with reliable simulations that support informed decision-making at a reduced cost.

As postdoctoral researcher during 2022 in the Anillo ACT210087 project, I focused on numerical methods with application on desalination processes. Water desalination is  a problem often mentioned in the scientific community due to the increasing scarcity of available drinking water. A first approach, commonly used for the modelling of reverse-osmosis system in water filtration is to propose a coupled Navier-Stokes Diffusion model, where the Darcy's law provides a relation between the permeability of the membrane and the amount of minerals in the water. One of the papers resulting from this research is {% cite https://doi.org/10.1002/fld.5252 %}, where we used Nitsche's method for imposing the mineral-dependent permeability condition in a channel with saltwater flow. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/flow-transport_project1.jpeg" title="Velocity and concentration" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Velocity streamlines (top panels) and concentration profiles (bottom panels) around the spacer in a cavity-type configuration.
</div>

The project allowed me to do a research stay with Dr. Ricardo Ruiz-Baier at Monash University. I had the opportunity to study new software for solving partial differential equations. Specifically, I learned to use the Gridap software. This software has the capabilites of using the skeleton of the mesh and is specially useful when implementing Cut Finite element methods. Additionally, we investigated the use of a Lagrange multiplier to enforce specific conditions. However, particular attention is required to ensure accurate trace estimates for the analysis. As future research, we aim to explore the use of H(div)-conforming methods combined with a posteriori analysis to enhance information retrieval along the membrane. Moreover, poromechanics is being considered to model a coupled fluid--porous membrane--transport model.
