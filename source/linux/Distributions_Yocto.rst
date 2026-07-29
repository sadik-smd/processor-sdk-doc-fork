.. _distributions-yocto:

#####
Yocto
#####

TI's primary Linux distribution is based on the `OpenEmbedded/Yocto Project
<https://www.yoctoproject.org/>`__ build framework. It is the reference
distribution for Processor SDK Linux and is used to build the default SD card
images and filesystem shipped with every release.

The Yocto-based distribution provides:

- A fully source-built, reproducible Linux system for TI K3 devices.
- Integration with TI's Arago layer (``meta-arago``) for board support,
  drivers, and TI-specific middleware.
- RT-Linux (PREEMPT_RT) kernel variants for real-time use cases.
- SELinux, disk encryption (LUKS), and Chromium browser build configurations.
- Compatibility with standard OpenEmbedded tooling (``devtool``, ``bitbake``,
  ``oe-layertool-setup``).

For the Yocto layer configuration used in the current release, see
:ref:`yocto-layer-configuration`.

For instructions on building the SDK from source, see
`Building the SDK <Overview_Building_the_SDK.html>`__.
