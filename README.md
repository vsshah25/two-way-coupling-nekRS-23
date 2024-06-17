# 2 way -coupling in nekRS

This document describes how to use two-way coupling in nekRS (v23).
Our approach is based on PPICLF library developed by D. Zwick.
Here I include the changes in source code required for builing the two-way code.
Two specific examples.
1) Single particle in a triply periodic domain with unit velocity
2) Multiple particles in triply periodic domain with homogenous isotropic turbulence

## Changes to source code

A few changes were made to the 'src/pluggin/lpm.cpp' of the nekRS directory and corresponding 'src/pluggin/lpm.hpp'
User can replace lpm.cpp and lpm.hpp with the files attached.

In order to implement two-way coupling using binning, mesh particles/points are created and transfered from fluid domain to particle domain.
Within the particle domain forces are calculated based on particles and ghost particles.
After this mesh particles with corresponding forces (and necessary quantities) are sent back to fluid domain.

## Examples
### Single particle Demo

Here we have a triply periodic box domain
A unit velocity is provided to the domain in x direction
Single particle is released with zero velocity.
One can observe that the particle velocity increases and its effect on neighbouring fluid points.

### Homogenous Isotropic turbulence
Here we have a triply periodic box domain with homogenous isotropic turbulence.
Number of particles = 36*36*36 = 46656
Mesh points = 512000

## Running examples

Once nekRS is build using the lpm.cpp and lpm.hpp attached, the examples can be executed as a regular nekRS cases without any additional steps.
The cases will generate field files (case0.f*) and particle data files (par*.vtu)
These can be loaded anf visualized in visit.

## Testing and results
The code was tested using two GPU machine.
At this point the code is not generalized for multiple GPUs but can be easily extended to work on multiple GPUs with minor modifications in udf and oudf files.

Timing analysis is attached in the ppt.
Due to creation and migration (to and fro) of mesh points there is a drastic increase in time for communication resulting in the time increase for 2-way coupling.


Another version of the code that takes into account some special assumptions is also developed.




