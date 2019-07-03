# A physically-based precipitation separation algorithm for convection-permitting models over complex topography

## Description of the algorithm

This algorithm is designed to separate the precipitation from convection-permitting model outputs into three categories : (1) convective, (2) stratiform, and (3) orographically enhanced stratiform precipitation. The algorithm is presented and discussed in a research article (B.Poujol, S.Sobolowski, P.Mooney and S.Berthou, *A physically-based precipitation separation algorithm for convection-permitting models over complex topography*, in preparation). Refer to it for further information. The algorithm has been designed in spring 2019 by the authors of the article.

## Requirements
All the input of the algorithm is NetCDF format, and over a grid with constant spacing between the grid points.
The main data required by the algorithm is :
 - Precipitation
 - Three dimensional wind speed (*u*,*v*,*w*) at the middle of the troposphere
 - Horizontal wind speed (*u*,*v*) at a level close to the ground, but above most of the topography in the dataset domain

All this data must be stocked in 3-Dimensional NetCDF files.
The algorithm also requires the topography. The topography NetCDF file must be on the same grid than the data.

It is possible to run the algorithm even if the wind speed has degraded temporal resolution compared to precipitation. If there are k more timesteps in the precipitation data than in the wind speed data, you just have to make sure that the first time step of wind speed data is the middle of the k first time steps of precipitation data, and fill in the appropriate value of k in the code of Part 2.

## How to use the scripts
Once you have all this data, the algorithm is quite easy to use. It is divided in two scripts, written in Python language. The scripts require the NetCDF4 and numpy modules.
 - The first script calculates the standard deviations needed for the separation. This takes most of the time. The script produces three output files at the `.npy` format, that are numpy arrays the same shape than the wind speed data and contain the standard deviations of vertical velocity, uplift velocity and vorticity.
 - The second script uses the standard deviations and the thresholds to separate the precipitation. It is fast to run but requires a lot of memory, so it is generally useful to use a mask to select a subregion of the domain. If a mask is used, it has to be at the NetCDF format. This script uses the output of the first script and produces three files : `convective.npy`, `stratiform.npy` and `orographic.npy` that contain the convective, stratiform and orog-strati precipitation. They have exactly the same shape than the precipitation data but are stocked as numpy arrays.