# stabRAS_OF50

## Introduction to the problem and solution
In past simulations of free-surface waves (both breaking and non-breaking) using RANS models, there have been a marked a collective tendency to over-estimate the turbulence levels. In cases involving breaking waves, this has even been most pronounced prior to breaking with turbulence levels pre-breaking of same order of magnitude as in the surf-zone. The underlying cause was originally identified by Mayer and Madsen (2000) who showed that turbulence production exists in potential flow waves. They further showed that if omega falls below a certain threshold, the model becomes unstable, leading to an exponential growth in both the eddy viscosity and turbulent kinetic energy.

In Larsen and Fuhrman (2018) it was proved that (1) standard two-equation closures (k-omega and k-epsilon) are unconditionally unstable and (2) they can be simply and elegantly stabilized, thus solving this long-standing wide-spread problem. 

This repository contains formally stabilized versions of standard two-equation turbulence models for OpenFOAM-5.0
Additonally all versions in this repository, similar to the model in Larsen and Furhman (2018) also include a buoyancy production term. See e.g. Umlauf et al. (2003) or Burchard (2002) for more details. 

For any usage of these models please refer to

	@article{LarsenFuhrman2018,
	title = {On the over-production of turbulence
   	beneath surface waves in {Reynolds-averaged Navier-Stokes} models},
  	language = {eng},
  	author = {Larsen, B. E. and Fuhrman, D. R.},
	journal = {J. Fluid Mech.},
	VOLUME={853},
  	pages = {419--460},
  	year = {2018},
	 doi =  {10.1017/jfm.2018.577},
	url= {https://doi.org/10.1017/jfm.2018.577}
	}
	
We acknowledge financial support from the Independent Research Fund Denmark project SWASH: Simulating WAve Surf-zone Hydrodynamics and sea bed morphology, Grant no. 8022-00137B.

## Installation
Source OpenFOAM 5.0:

	source /home/$USER/OpenFOAM/OpenFOAM-5.0/etc/bashrc

In a linux terminal download the package with git by typing:

	git clone https://github.com/BjarkeEltardLarsen/stabRAS_OF50.git
	
Create folder for turbulence model (if the folders already exist skip this part)

	mkdir $WM_PROJECT_USER_DIR $WM_PROJECT_USER_DIR/src $WM_PROJECT_USER_DIR/src/TurbulenceModels $WM_PROJECT_USER_DIR/src/TurbulenceModels/turbulenceModels

Move the folder to the user source code

	mv stabRAS_OF50 $WM_PROJECT_USER_DIR/src/TurbulenceModels/turbulenceModels/
	
Go to the directory and compile the turbulence models

	cd $WM_PROJECT_USER_DIR/src/TurbulenceModels/turbulenceModels/stabRAS_OF50
	
	wmake libso	
	
Move the tutorials to the desired folder e.g FOAM_RUN

	mv Tutorials $FOAM_RUN
	
## Tutorials

The tutorials consist of eight different version of the compiled turbulence models. In each of these tutorials a wave is inialized in a domain which is exactly one wave length long with cyclic boundaries and a slip condition at the bed. This in other words an idealized case where the flow can considered very close to potential flow. 

The cases ending with "Stab" have lambda2=0.05 and are stabilized version of the new turbulence models. In these cases the initial turbulence levels will decay in time. 

Cases not ending with Stab corresponds to the standard models (with buoyancy production included) and here growth rather than decay in the turbulence level can be seen.

To run a tutorial go to the folder of the tutorials and choose one of the eight cases. 

Make the RunScrip excecutable by 

 	chmod +x RunScript

Then run the RunScript by typing (the tutorials will take a few ours depending on speed of the machine)
	
	RunScript

The tutorials folder also contain a small animation, which shows the development in the turbulent kinetic energy for all eight tutorials.
	
## Usage
Include the libary of the stabilized turbulence models in the system/controlDict folder

	libs
	(
    	"libstabRASModels.so"
	);

Change the constant/turbulence
Each of the four models can be chosen by uncommenting the desired model.
If lambda2=0 the models default to their standard OpenFOAM implementation (but with the buoyancy production term added). 

	simulationType  RAS;

	RAS
	{
	RASModel        kOmegaStab;
	//RASModel        kOmegaSSTStab;
	//RASModel        kEpsilonStab;
	//RASModel        RNGkEpsilonStab;


	turbulence      on;

	printCoeffs     on;

	kOmegaStabCoeffs
	{
  	lambda2 0.05;
	}

	kOmegaSSTStabCoeffs
	{
  	lambda2 0.05;
	}

	kEpsilonStabCoeffs
	{
  	lambda2 0.05;
	}

	RNGkEpsilonStabCoeffs
	{
  	lambda2 0.05;
	}
## References

Burchard, H.
	Applied Turbulence modelling for Marine Waters
	Springer
	2002

Larsen, B.E. and Fuhrman, D.R.
	On the over-production of turbulence beneath surface waves in RANS models
	J. Fluid Mech. 853. pp 419-460. https://doi.org/10.1017/jfm.2018.577
	2018 
	
Mayer, S. & Madsen, P.A. 
	Simulation of breaking waves in the surf zone using a Navier-Stokes solver. 
	In: Proc. 25th. ICCE, Sydney. Australia, pp. 928-941.
 	2000

Umlauf, L., Burchard, H. and Hutter, K.
	Extending the k-omega turbulence model towards oceanic applications
	Ocean Modelling. 5(3), 195-218
	2003

	
