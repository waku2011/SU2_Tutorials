# SU2_Tutorials
How to configure and run SU2

# Install 
I installed binary from SU2 site for Ubuntu 22.04 LTS on WSL2.
[https://su2code.github.io/download.html](https://su2code.github.io/download.html)

Download binary SU2MPI v8.0.0 for Linux, which requires MPICH installation,
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
$ git clone https://github.com/su2code/TetsCases.git
```

# Quick start



