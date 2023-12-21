# SU2_Tutorials
How to configure and run SU2.

# Install 
I installed binary from SU2 site for Ubuntu 22.04 LTS on WSL2.
[https://su2code.github.io/download.html](https://su2code.github.io/download.html)

Download binary 'SU2 MPI v8.0.0 for Linux', which requires MPICH installation,
unzip it for ~/SU2, and setup envronmental variables within ~/.bashrc as bellow.

```
## SU2
export SU2_RUN=/home/user/SU2/bin
export SU2_HOME=/home/user/SU2
export PATH=$PATH:$SU2_RUN
export PYTHONPATH=$PYTHONPATH:$SU2_RUN
```

# Download Tutorials and Test Case Suite.
From Github, they can be cloned.
I cloned them in /home/user/SU2.
```
$ git clone https://github.com/su2code/Tutorials
$ git clone https://github.com/su2code/TestCases.git
```

# Quick start 
`Quick start` stated in this [page](https://su2code.github.io/docs_v7/Quick-Start/).
Documents are prepared for Ver.7 but may be NOPROBLEM for Ver.8 :-)

## (1) Prepareation
Let's change directory to QuickStart, but it has no both SU2 configuretion and mesh files.
They can download from links within "Resources" section.
extantion `.cfg` means configuration and `.su2` mesh files. 
Details of these files will be examined for future task for me. 
```
cd ~/SU2/Tutorials/compressible_flow/QuickStart
Download 2 raw files from Github.
  (1) https://github.com/su2code/SU2/blob/master/QuickStart/inv_NACA0012.cfg
  (2) https://github.com/su2code/SU2/blob/master/QuickStart/mesh_NACA0012_inv.su2
```
## (2) Ploblem Setup
Solve Euler equations on the NACA0012 airfoil at AOA 1.25 deg.
- Pressure = 101325 Pa
- Temperature = 273.15 K
- Mach number = 0.8
- mesh consists of 10216 triangular cells, 5233 points, and two boundaries named _airfoil_ and _farfield_.

## (3) Run the case (SU2 Direct Analysis)
SU2 will print residual updates with each iteration of the flow solver, and the simulation will finish after reaching the specified convergence criteria. Output files containing the flow results (with “flow” in the file name) will be written upon exiting SU2. The flow solution can be visualized in ParaView (.vtu) or Tecplot (.dat or .szplt).

```
$ SU2_CFD inv_NACA0012.cfg

-------------------------------------------------------------------------
|    ___ _   _ ___                                                      |
|   / __| | | |_  )   Release 8.0.0 "Harrier"                           |
|   \__ \ |_| |/ /                                                      |
|   |___/\___//___|   Suite (Computational Fluid Dynamics Code)         |
|                                                                       |
-------------------------------------------------------------------------
| SU2 Project Website: https://su2code.github.io                        |
|                                                                       |
| The SU2 Project is maintained by the SU2 Foundation                   |
| (http://su2foundation.org)                                            |
-------------------------------------------------------------------------
| Copyright 2012-2023, SU2 Contributors                                 |
|                                                                       |
| SU2 is free software; you can redistribute it and/or                  |
| modify it under the terms of the GNU Lesser General Public            |
| License as published by the Free Software Foundation; either          |
| version 2.1 of the License, or (at your option) any later version.    |
|                                                                       |
| SU2 is distributed in the hope that it will be useful,                |
| but WITHOUT ANY WARRANTY; without even the implied warranty of        |
| MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU      |
| Lesser General Public License for more details.                       |
|                                                                       |
| You should have received a copy of the GNU Lesser General Public      |
| License along with SU2. If not, see <http://www.gnu.org/licenses/>.   |
-------------------------------------------------------------------------

Parsing config file for zone 0

----------------- Physical Case Definition ( Zone 0 ) -------------------
Compressible Euler equations.
Mach number: 0.8.
Angle of attack (AoA): 1.25 deg, and angle of sideslip (AoS): 0 deg.
No restart solution, use the values at infinity (freestream).
Dimensional simulation.
The reference area is 1 m^2.
The semi-span will be computed using the max y(3D) value.
The reference length is 1 m.
Reference origin for moment evaluation is (0.25, 0, 0).
Surface(s) where the force coefficients are evaluated: airfoil.
 :
 :
----------------------------- Solver Exit -------------------------------
All convergence criteria satisfied.
+-----------------------------------------------------------------------+
|      Convergence Field     |     Value    |   Criterion  |  Converged |
+-----------------------------------------------------------------------+
|                    rms[Rho]|      -8.02038|          < -8|         Yes|
+-----------------------------------------------------------------------+
-------------------------------------------------------------------------
+-----------------------------------------------------------------------+
|        File Writing Summary       |              Filename             |
+-----------------------------------------------------------------------+
|SU2 binary restart                 |restart_flow.dat                   |
|Paraview                           |flow.vtu                           |
|CSV file                           |surface_flow.csv                   |
+-----------------------------------------------------------------------+

--------------------------- Finalizing Solver ---------------------------
Deleted CNumerics container.
Deleted CIntegration container.
Deleted CSolver container.
Deleted CIteration container.
Deleted CInterface container.
Deleted CGeometry container.
Deleted CFreeFormDefBox class.
Deleted CSurfaceMovement class.
Deleted CVolumetricMovement class.
Deleted CConfig container.
Deleted nInst container.
Deleted COutput class.
-------------------------------------------------------------------------

------------------------- Exit Success (SU2_CFD) ------------------------
```

## Visualization with Paraview
```
$ paraview flow.vtu
```
![Mach contour](https://github.com/waku2011/SU2_Tutorials/assets/10591304/65ea27c8-6aee-4ff3-a59f-c777226d98e9)

# make mesh via GMSH
SU2 can use SU2 native format (.su2) and CGNS format [as stated in user guide](https://su2code.github.io/docs_v7/Mesh-File/).
SU2 native format is simple and easy to understand, also SU2 provides python, cpp and fortran sample programs to make simple mesh blocks. 
Here, I'll explore how to mesh using GMSH, which can export mesh in SU2 native format.

## sample CAD data (.step)
I made STEP file including farfield and solid using SolidWorks. farfield is a sphere of radius 10m as far boundary,
solid is something flying obstacle (wing-like?) at the center of spherical volume as bellow.

![image](https://github.com/waku2011/SU2_Tutorials/assets/10591304/e00a9df5-166d-4e85-9ab8-5026917d27c2)

## read STEP file
- read the step files from GMSH
- mesh 3D
- export mesh as SU2 
