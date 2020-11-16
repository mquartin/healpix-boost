# healpix-boost
This modified version of HEALPix, HEALPix-Boost was originally created by Riccardo Catena with help from Alessio Notari and later contributions from Miguel Quartin and Pedro da Silveira Ferreira.

HEALPix-Boost was tested with version HEALPix v3.10, and may need adjustments to be compatible with other HEALPix versions.

To compile HEALPix-Boost just download the source code of HEALPix v3.10 and replace the "f90" directory inside the "src" directory with the one provided on this webpage: "f90-aber-3.10.zip". It may be necessary to do a "make clean" before configuring the HEALPix installation with the new f90 folder.

This modified package probably does not work with the Parallel implementation of HEALPix, so we recommend that you choose the Serial implementation when compiling.

****************************************************************

Any publications making use of HEALPix-Boost should cite these papers on which the code was originally tested:

R. Catena and A. Notari, "Cosmological parameter estimation: impact of CMB aberration," JCAP 1304 (2013) 028 [arXiv:1210.2731 [astro-ph.CO]].

A. Notari, M. Quartin and R. Catena, "CMB Aberration and Doppler Effects as a Source of Hemispherical Asymmetries", JCAP 1403(2013) 019 [arXiv:1304.3506 [astro-ph.CO]].

****************************************************************

How to use HEALPix-Boost

HEALPix-Boost works by implementing Doppler and aberration in pixel-space. It modifies the synfast, anafast and alteralm routines.

[--SYNFAST--] 
It adds two flags to the synfast routine:

1) ab_dopp = 0, 1, 2 or 3  [0 for no boost, 1 for Doppler only, 2 for both Doppler and aberration, 3 for aberration only]

2) beta [signed real number describing the magnitude of the velocity v/c, assumed to be along the north pole of the map]

The modified synfast routine produces a map with the selected boost effects included. 

IMPORTANT NOTE: the outfile_alms file produced DOES NOT include the boost effects. For the boosted alm's, it is necessary to run anafast on the boosted map.

To produce boosts along different directions it is necessary to rotate the map, using for instance the modified HEALPix alteralm routine (see below).

[--ANAFAST--]
The modified anafast routine also allows now for asymmetric theta_cut_deg. The theta_cut_deg flag now corresponds to a cut above the equator, and we introduced a new flag, theta_cut_degS, which specifies the theta cut below the equator. This allows for simple cuts of discs on the maps to be analyzed.

[--ALTERALM--]
We added the option to do Wigner rotations directly in the alteralm routine. It is implemented using HEALPix internal sub-routines. It is triggered by the DoWigner boolean flag, along with inputing 3 Euler angles: phi1, theta, phi2 (see the original HEALPix documentation for further explanations on these 3 angles). To rotate the alm's such that the dipole direction replaces the galactic north as the new alm north, just use: 
DoWigner = T
phi1 = 1.6755   
theta = -0.7285  
phi2 = 0
