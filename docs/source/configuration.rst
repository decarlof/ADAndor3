=============
Configuration
=============

The Andor3 driver is created with the andor3Config command, either from C/C++ or from the EPICS IOC shell.

int andor3Config(const char *portName, int cameraId,
                int maxBuffers, size_t maxMemory,
                int priority, int stackSize, int maxFrames)
  

For details on the meaning of the parameters to this function refer to the detailed documentation on the andor3Config function in the andor3.cpp documentation and in the documentation for the constructor for the andor3 class. The maxFrames parameter controls the number of frame buffers the driver queues to the SDK when acquiring data. The default value is 10. Increasing this number will allow the SDK to transfer images from the camera at the full interface speed even when the driver is not reading them that quickly. This will help to prevent frames from filling the camera RAM when operating close to the maximum interface transfer rate (=TransferRate).

There an example IOC boot directory and startup script (iocBoot/iocAndor3/st.cmd) provided with areaDetector. 

.. contents:: Contents:
   :local:

