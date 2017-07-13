============
Introduction
============

 This is an EPICS areaDetector driver for CMOS detectors from Andor Technology. It has been tested on the Andor Neo sCMOS camera with 3-tap Camera Link interface, but should work with other cameras (e.g. Zyla) as well. The driver is supported on 32-bit and 64- bit Linux and 32-bit and 64-bit Windows. The driver is called "Andor3" because it is built with Version 3 of the Andor Software Development Kit (SDK). This version of the SDK is required to work with the Andor sCMOS cameras, and currently does not work with the Andor CCD cameras.

The driver provides access to essentially all of the features of the Andor sCMOS cameras:

- Fixed number of frames or continuous acquisition.
- Multiple accumulations per frame.
- Readout frequency
- Readout mode (11-bit low noise, 11-bit high-well, 16 bit combination).
- Support for all of the Andor trigger modes
- Binning and Area Of Interest (AOI) readout
- Set and monitor the camera temperature
- Set the camera fan speed.

This driver inherits from ADDriver. It implements many of the parameters in asynNDArrayDriver.h and in ADArrayDriver.h. It also implements a number of parameters that are specific to the Andor detectors. The andor3 class documentation describes this class in detail.

This document does not attempt to explain the meaning of the Andor-specific parameters. The Andor Software Development Kit documentation provides this detailed information. Andor does not allow me to redistribute the SDK documentation as part of areaDetector. It must be obtained from Andor's Web site.

The Andor3 SDK is very well designed. Camera parameters (e.g. exposure time, binning) are called "features". Features can be integer, float, bool, string, or enum. Each feature can be queried to determine if it is implemented on the current detector. In addition:

- For integer and float features:
  - What is the valid range of values for the current camera under the current conditions?
- For enum features
  - How many enum choices are there?
  - For each enum choice:
   - What is the string associated with that enum choice?
   - Is that choice implemented for the current camera?
   - If it is implemented, is it valid under the current conditions?
- Ability to register a user-defined C callback function that will be called whenever a feature value changes. These changes can be the indirect result of changing another feature. For example, changing the binning might force the exposure time to change, etc.

The areaDetector driver uses these features. All of the enum menus are built dynamically at iocInit, they are not preset in the template file. This ensures that the enum choices match the actual capabilities of the current camera. Whenever an integer or float parameter is changed it is checked to ensure it is within the current valid bounds for that feature. The feature callback is used to ensure that the current EPICS readback value of that parameter matches the actual camera value, without requiring the driver to poll.

areaDetector includes the header and library files required to build the andor3 driver on any Linux or Windows computer. However, it does not include the shareable libraries, DLLs or drivers to actually run a detector. Those must be obtained from Andor, either by purchasing their SDK or their Solis application software. On Windows the path to the directory containing the Andor DLLs from the SDK or Solis must be added to the PATH environment variable when running the areaDetector IOC. On Linux the path to the directory containing the Andor shareable libraries from the SDK must be added to the LD_LIBRARY_PATH environment variable when running the areaDetector IOC.

Note: Linux drivers and Bitflow based camera may require the removal of files /usr/local/lib/libatusb*. These files sometime interfere with Bitflow based cameras on Linux (per Andor).

.. contents:: Contents:
   :local:

