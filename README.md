# RaspiMJPEG for PlanktoScope

This repository is the fusionned host between [robettidey's fork](https://github.com/roberttidey/userland/) and the upstream repo from the [raspberry foundation](https://github.com/raspberrypi/userland/).

Those two repositories have been properly merged and rebased (the fork has been rebased on the upstream repo, as it should have).

This repository contains the changes that Plankton Planet did to the RaspiMJPEG tool to suit their needs regarding the [PlanktoScope](https://github.com/PlanktonPlanet/PlanktonScope).

Those changes are just adding a command to update the filename and to provide a name directly to the command that launches an image capture.



## Original README
This repository contains the source code for the ARM side libraries used on Raspberry Pi.
These typically are installed in /opt/vc/lib and includes source for the ARM side code to interface to:
EGL, mmal, GLESv2, vcos, openmaxil, vchiq_arm, bcm_host, WFC, OpenVG.

Use buildme to build. It requires cmake to be installed and an ARM cross compiler. For 32-bit cross compilation it is set up to use this one:
https://github.com/raspberrypi/tools/tree/master/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian

Whilst 64-bit userspace is not officially supported, some of the libraries will work for it. To cross compile, install gcc-aarch64-linux-gnu and g++-aarch64-linux-gnu first. For both native and cross compiles, add the option ```--aarch64``` to the buildme command.

Note that this repository does not contain the source for the edidparser and vcdbg binaries due to licensing restrictions.
