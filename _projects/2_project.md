---
layout: page
title: ANID postdoc 3230302 (2023-2026)
description: Mathematical and computational analysis of solid and fluid mechanics
img: assets/img/ANID_postdoc_main_img.jpeg
importance: 1
category: work
related_publications: true
---
Numerical solutions to real problems have always interested mathematicians. Prominent researchers in the last couple of centuries have investigated natural phenomena, and contributed with the development of mathematical models as a product of their research. One of the most commonly used approaches to model these problems is partial differential equations. Analysis of these models called for the creation of analytical-numerical methods which may vary on each model. Currently, we can see these results in the automotive industry with the construction of safer chassis; in the aerospace industry with the development of aerodynamic chassis, capable of resisting strong winds or high temperatures; in civil engineering to build anti-seismic structures, in medicine with MRIs; or simply to control land/sea/air traffic.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ANID_postdoc_img1.jpeg" title="Deformed elastic 
structure" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ANID_postdoc_img2.jpeg" title="Streamlines of 
velocity in a 3D-lshaped domain" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/ANID_postdoc_img3.jpeg" title="Refined meshes in a 
3D-lshaped domain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Samples images of several simulations such as the elastic deformation in a Fluid-Structure eigenvalue problem (left), Stokes-Brinkman eigenvalue problem (middle) and adaptive refined meshes on a DG scheme for elasticity (right).
</div>

In most cases, these phenomena are associated with partial differential equations, where finite differences method, finite elements method or virtual elements methods are used to solve the equations numerically. Considering this, the present project aims to analyze numerically and mathematically problems in solid and fluid mechanics, including eigenvalue problems in structures, fluids and fluid-structure coupling, or  induced stresses in structures with memory and viscoelastic fluids. All this, taking into account the existing variability in the types of materials and geometrical conditions. The numerical analysis of these phenomena will be approached using the finite element method and its possible extension to the virtual element method, where the comparison of the advantages and disadvantages of each of them according to the problem under study will be one of the main goals.

At this moment, as a product of this research, different results have been obtained in eigenvalue problems in finite elements {% cite LEPE2024116959 lepe2024finite LEPE2024115700 LEPE2023114798 refId0 lepe_calcolo_stokes  %}. We are currently developing loading models in fluid-transport couplings, virtual elements for fluid problems with slip conditions and eigenvalue problems with virtual elements.

