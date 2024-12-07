# Multi-Camera PIV Image Generation
Generate artificial images for multi-camera particle image velocimetry.

<img src="https://github.com/user-attachments/assets/9e6117a0-6c7c-4c42-a56a-30d9aed01769" width="500" height="500"/>

## Usage 
This example generates multi-camera images of a Burgers vortex.

```matlab
% Compile C code to mex
compile_generateParticleImage();

% Run test program to generate multi-camera images and save them to the current directory
makeImages();

% Run test program to generate images and save them to a different directory.
% This example will save image_0001.tif, image_0002.tif, etc., in ./myOutputDir
makeImages('outdir', './myOutputDir', 'outbase', 'image_');
```

## Specifying your own velocity field
To specify your own velocity field rather than the default Burgers vortex:
1. Following `burgersVortex.m` as an example, write a velocity function that forward-propagates (e.g., using `ODE45`) a set of (*x,y,z*) positions according to a velocity **u**(*x,y,z*). The easiest way to do this is probably to create a copy of `burgersVortex.m` and modify it. 
2. Pass a function handle to your velocity function as an input argument to `makeImages()`:
```matlab
  makeImages('velocityFunction', @burgersVortex);
```

## Camera Parameters
Specify camera intrinsic parameters by modifying the file `defaultCamera.m`. Currently only the pinhole camera model is supported.
Specify the extrinsic parameters of a set of cameras by modifying the file `defaultCameraArrangement.m`, or creating your own function to output a struct whose elements are `camera` structs, and passing it to `makeImages` using the `cameras` input variable:

```matlab
% Using defaultCameraArrangement.m:
cameras = defaultCameraArrangement();
makeImages('cameras', cameras);

% Specifying your own camera arrangement function:
myCameras = myCameraArrangementFun();
makeImages('cameras', myCameras) 
```

