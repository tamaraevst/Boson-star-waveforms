# Boson-star-waveforms
This repository contains numerical-relativity (NR) waveforms of boson-star (BS) binaries from two independent NR codes, [GRChombo](https://github.com/GRTLCollaboration/GRChombo) and Lean. The data for each binary configuration is stored in the HDF5 format (the Hierarchical Data Format version 5) following the standards of the LIGO Algorithms Library (LAL) supoprting the simplest format 1 (see [here](https://arxiv.org/abs/1703.01076) for more details). 

# Our conventions 

You should familiriase yourself with our papers on BSs to understand in more detail how our numerical simulations are set-up (please refer to References below). Here, we briefly outline some important BS nomenclature. BSs are modelled by a complex scalar field, which we decompose into:

<p align="center">
$\varphi = A(r) e^{i (\epsilon \omega t + \delta \phi)}$,
</p>

where

* $A(r)$ is the amplitude, so that $A_{ctr} = A(0)$ is the central amplitude of the BS,
* $\epsilon$ is parameter controlling the rotation of the scalar field in the complex plane, and is either $\epsilon = 1$ (regular BS) or $\epsilon = -1$ (anti-boson star),
* $\omega$ is the frequency,
* $\delta \phi$ is the phase off-set parameter.

# File names

The files have specific naming conventions that we describe below. We note that the names of the files do not contain all of the information about the simulations, which should instead be read from the attributes inside the file. We can use _GRChombo_BBSsol02_A17A17q100d17p000a_h40.h5_ as our reference example. 

* The first string corresponds to the code name. i.e. either GRChombo or Lean.
* Next, it is followed by _BBSsol02_ standing for binary boson star with solitonic potential and the self-interaction term $\sigma_0 = 0.2$.
* We then have _A17A17_ denoting the central amplitudes of the two stars, here they have the same amplitudes of $A_{ctr} = 0.17$.
* Next, we give the mass ratio _q100_ corresponding to equal-mass $q=1$.
* The separation of $d=17$ is given via _d17_ suffix.
* Whether a phase off-set is applied is indicated by _p000_; here there is no phase-offset applied. However, if a binary had a phase-offset $\delta \phi = \pi$, we would have _p180_ in the name. 
* Whether one of the stars is anti-boson is indicated by _'a'_ and if no _'a'_ is appended, this means that both of the stars are regular BSs. 
* Finally, _h40_ stands for the grid spacing on the finest level, i.e. here it is $dx=1/40$. 

# Reading files

Here we show a simple example on how you can use [h5py](https://pypi.org/project/h5py/) package to read our HDF5 files.

You first need to load the data.

```python
#Import modules
import h5py as h5
import numpy as np

filename = "./GRChombo_BBSsol02_A17q1d17p000_Res40.h5" #Pick you favourite filename
f = h5.File(filename, "r") #Read the file
```

You can then access the attributes of the file, containing additional information about the simulation and read-off their values/strings. 

```python
for key, val in f.attrs.items():
        print("    %s: %s" % (key, val))
```
For example, the attributes contain the key ```initpos1```, which is the initial position of the first star, and its value is:

```initpos1: "-0.15    8.5     0.0"```

***NOTE***: The positions of the stars (```initpos1``` and ```initpos2```) are given in code units! 

The attributes' keys show information required for format 1 of [LAL NR injections](https://arxiv.org/abs/1703.01076) as well as BS specific information such as central amplutudes, dephasing and many more. 

Finally, you may want to access the information on phases and amplitudes of a specific $(lm)$-mode. For example:  

```python
#Print the data available in the group for the amplitude (22)-mode
for dset in f["amp_l2_m2"].keys():      
    print (dset)

time = np.array(f["amp_l2_m2"]["X"]) #time array for the amplitude of the (22)-mode 
amplitude = np.array(f["amp_l2_m2"]["Y"]) #amplitude array for the (22)-mode

import matplotlib.pyplot as plt 

#Plot the GW amplitude of the (22)-mode

plt.plot(time, amplitude)
plt.xlabel("Time")
plt.ylabel("Amplitude")
plt.show()
```

Now, you are ready to use the HDF5 files! 

# Further information on codes

**GRChombo & Lean:**

* M.Radia et.al.: [Lessons for adaptive mesh refinement in numerical relativity](https://arxiv.org/abs/2112.10567).

**GRChombo:**

* T.Andrade et.al.: [GRChombo: An adaptable numerical relativity code for fundamental physics](https://arxiv.org/abs/2201.03458),
* K.Clough et.al.: [GRChombo : Numerical Relativity with Adaptive Mesh Refinement](https://arxiv.org/abs/1503.03436).

**Lean:**

 * G. Allen et.al.: [The Cactus computational toolkit and using distributed computing to collide neutron stars](https://ieeexplore.ieee.org/document/805282),
 * E. Schnetter et.al.: [Evolutions in 3D numerical relativity using fixed mesh refinement](https://arxiv.org/abs/gr-qc/0310042).

# References

* T. Helfer et.al.: [Malaise and remedy of binary boson-star initial data](https://arxiv.org/abs/2108.11995),
* T. Evstafyeva et.al.: [Unequal-mass boson-star binaries: Initial data and merger dynamics](https://arxiv.org/abs/2212.08023),
* R. Croft et.al.: [The Gravitational Afterglow of Boson Stars](https://arxiv.org/abs/2207.05690). 
