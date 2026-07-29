#############################################
TI Processor SDK — Distributions Overview
#############################################

Texas Instruments provides multiple Linux distribution options for its Processor
SDK, each targeting different use cases — from rapid prototyping and community
development to production-hardened embedded systems. This page describes every
available distribution, when to choose each one, and which TI devices support it.

.. contents:: On this page
   :local:
   :depth: 2

----

Overview
========

All TI Processor SDK distributions share the same underlying TI BSP components
(U-Boot, TI Linux Kernel, TI Linux Firmware, TF-A, OP-TEE), but differ in their
userspace tooling, package management, and build system.

.. list-table::
   :header-rows: 1
   :widths: 18 20 20 22 20

   * - Distribution
     - Build System
     - Package Manager
     - Target Use Case
     - Community
   * - **Yocto Linux**
     - OpenEmbedded / Yocto
     - ``opkg`` / ``rpm`` / ``deb``
     - Production embedded systems
     - Yocto Project (LF)
   * - **Debian (Armbian)**
     - Armbian build framework
     - ``apt`` / ``dpkg``
     - Development, prototyping, community
     - Debian + Armbian
   * - **Buildroot**
     - Buildroot
     - None (static image)
     - Minimal footprint, constrained hardware
     - Buildroot community
   * - **Android**
     - Android Build System (Soong)
     - Android APK
     - Consumer / HMI applications
     - AOSP
   * - **EdgeAI**
     - Yocto (specialized overlay)
     - ``opkg``
     - AI/ML inference at the edge
     - TI EdgeAI

----

Yocto Linux
===========

Yocto Linux is the **primary TI reference distribution** for Processor SDK. It
is built using the `OpenEmbedded <https://www.openembedded.org/>`__ build
framework and the `Yocto Project <https://www.yoctoproject.org/>`__ layer model.

What is Yocto?
--------------

The Yocto Project is a Linux Foundation collaborative project that provides
templates, tools, and methods to help you create custom Linux-based systems for
embedded products regardless of hardware architecture. Rather than shipping a
pre-built rootfs, Yocto generates a **custom rootfs from source** — every
package, library, and kernel driver is compiled specifically for your target
hardware.

TI's Yocto layers (``meta-ti-bsp``, ``meta-ti-extras``) extend the upstream
``poky`` reference distribution with TI-specific device trees, firmware blobs,
drivers, and demos.

Key Characteristics
-------------------

- **Full customization** — add or remove any package; control every aspect of
  the root filesystem
- **Reproducible builds** — pinned layer hashes guarantee bit-exact images
  across releases
- **Production ready** — used in commercial products; supports signed boot,
  secure boot chain, and OTA frameworks (SWUpdate, RAUC)
- **Long-term support** — TI tracks Yocto LTS releases (Kirkstone, Scarthgap)
- **Broad device support** — available for every TI device family (see
  compatibility matrix below)

Typical Workflow
----------------

.. code-block:: bash

   # Set up repo manifest
   repo init -u https://git.ti.com/git/arago-project/oe-layersetup.git \
       -b scarthgap -m default.xml
   repo sync

   # Configure build
   MACHINE=am62xx-evm DISTRO=arago-base-tisdk . ./oe-init-build-env

   # Build SDK image
   bitbake arago-base-tisdk-image

When to Choose Yocto
--------------------

Choose Yocto Linux when:

- You are developing a **commercial or production product**
- You need a **minimal, auditable rootfs** with no unnecessary packages
- You require **secure / signed boot** integration
- You need **precise control** over every userspace component
- You are targeting **resource-constrained devices** (limited flash/RAM)

Getting Started
---------------

Refer to the device-specific **Getting Started Guide** in the Overview section
of your device's SDK documentation. For shared Yocto build instructions see
:ref:`building-the-sdk`.

----

Debian (Armbian)
================

The Debian distribution for TI Processor SDK is built using the
`Armbian <https://www.armbian.com/>`__ build framework, which produces a
standard Debian-based image for embedded ARM boards. The result is a familiar
Debian system with full ``apt`` package management, a large pre-built package
repository, and community ecosystem access.

What is Armbian?
----------------

Armbian is an open-source project that brings Debian/Ubuntu to ARM single-board
computers. It uses the official Debian userspace combined with a curated Linux
kernel and board support packages. For TI devices, Armbian is configured with
TI's downstream kernel and BSP patches, providing a community-friendly
experience on professional-grade silicon.

Key Characteristics
-------------------

- **Familiar Debian environment** — standard ``apt`` package manager, Debian
  repositories, ``systemd`` init system
- **Rapid development** — install packages directly with ``apt install``, no
  rebuild required
- **Large ecosystem** — tens of thousands of pre-built packages from Debian
  ``bookworm`` / ``trixie`` repositories
- **Community boards** — ideal for BeagleBone, BeaglePlay, and other
  community hardware running TI SoCs
- **Desktop-capable** — supports graphical environments (Weston, XFCE) for HMI
  development
- **Focused scope** — supported on AM62X, AM62LX, AM62PX, and AM64X

Typical Workflow
----------------

.. code-block:: bash

   # Clone Armbian build framework
   git clone https://github.com/armbian/build.git
   cd build

   # Build image for a TI board (example: BeaglePlay / AM625)
   ./compile.sh BOARD=beagleplay BRANCH=current BUILD_DESKTOP=no \
       KERNEL_CONFIGURE=no

   # Flash to SD card
   dd if=output/images/Armbian_*.img of=/dev/sdX bs=4M status=progress

When to Choose Debian
---------------------

Choose Debian / Armbian when:

- You are **prototyping or doing early-stage development** and want fast
  iteration with ``apt install``
- You are working with a **community board** (BeagleBone, BeaglePlay)
- You need access to the **broad Debian package ecosystem** (Python packages,
  server daemons, development tools)
- Your team has **existing Debian/Ubuntu expertise** and wants minimal
  ramp-up time
- You plan to **contribute to or leverage the Armbian community**

Getting Started
---------------

See the device-specific **Debian Getting Started Guide** in the SDK documentation
for supported devices (AM62X, AM62LX, AM62PX, AM64X).

----

Buildroot
=========

Buildroot is a **minimal, fast build system** that generates a complete Linux
system — bootloader, kernel, and root filesystem — from source in a single,
reproducible pass. It is designed for simplicity and small footprint rather than
feature richness.

What is Buildroot?
------------------

`Buildroot <https://buildroot.org/>`__ is an open-source tool that automates
cross-compiled Linux system builds. Unlike Yocto, which uses a layered recipe
model, Buildroot uses a ``make``-based menuconfig approach familiar to Linux
kernel developers. TI maintains
`buildroot-external-TI <https://github.com/TexasInstruments/buildroot-external-TI>`__
— an external tree that adds TI-specific board configurations, package
selections, and firmware integration on top of upstream Buildroot.

Key Characteristics
-------------------

- **Minimal footprint** — images in the range of tens of megabytes; no package
  manager overhead at runtime
- **Fast builds** — simpler dependency model than Yocto; significantly shorter
  full build
- **menuconfig interface** — kernel-style ``make menuconfig`` for package
  selection; low barrier to entry
- **No runtime package management** — rootfs is fully static; no ``apt`` or
  ``opkg`` at runtime (a feature for production security hardening)
- **Reproducible** — fixed Buildroot version + external tree = deterministic
  output
- **Supported on** AM62X and AM62LX only

Buildroot Architecture
----------------------

The diagram below shows how the Buildroot-based Linux system for TI devices
is structured:

.. graphviz:: /graphviz/Buildroot_architecture.gv
   :caption: Buildroot-based Linux Architecture for TI Devices

Typical Workflow
----------------

.. code-block:: bash

   # Clone TI Buildroot external tree and upstream Buildroot
   git clone https://github.com/TexasInstruments/buildroot-external-TI.git
   git clone https://gitlab.com/buildroot.org/buildroot.git

   # Configure for AM62X EVM
   cd buildroot
   make BR2_EXTERNAL=../buildroot-external-TI \
       am62x_evm_defconfig

   # Optional: customize packages
   make menuconfig

   # Build
   make -j$(nproc)

   # Output images are in output/images/
   ls output/images/

When to Choose Buildroot
------------------------

Choose Buildroot when:

- You need the **smallest possible rootfs** (e.g., flash-constrained hardware)
- **Boot time** is critical — fewer daemons, faster init
- You want a **locked-down, read-only rootfs** with no package management attack
  surface
- Your application has **well-known, fixed dependencies** that do not change
  at runtime
- You are familiar with the **menuconfig workflow** and prefer it over Yocto
  recipes
- You are targeting **AM62X or AM62LX** devices

Getting Started
---------------

See the **Buildroot Getting Started Guide** for AM62X or AM62LX. The TI external
tree README provides full setup instructions.

----

Android
=======

Android support is available for select TI devices as part of the Processor SDK
Android offering. It is based on the
`Android Open Source Project (AOSP) <https://source.android.com/>`__ and
integrates TI's BSP (kernel, device trees, firmware) into the Android build
system.

Key Characteristics
-------------------

- **AOSP-based** — standard Android application framework; compatible with the
  Android app ecosystem
- **HMI / infotainment focus** — suited for displays, touch interfaces,
  automotive cockpit, and industrial HMI
- **Hardware acceleration** — integrates TI DSP/GPU capabilities via Android HALs
- **Supported on** AM62X, AM62PX, AM67A, and J7 Family devices

When to Choose Android
-----------------------

Choose Android when:

- You are building a **consumer-facing HMI** or infotainment system
- You need to run **Android applications** on embedded hardware
- You require **standard Android APIs** (Camera2, AudioManager, SurfaceFlinger)

----

EdgeAI
======

The EdgeAI distribution is a specialized Yocto-based SDK focused on
**AI/ML inference at the edge**. It packages TI's EdgeAI runtime libraries,
model zoo, and inference pipeline tools on top of the standard Linux SDK.

Key Characteristics
-------------------

- **Pre-integrated AI stack** — TI's EdgeAI SDK, TVM, ONNX Runtime, TFLite
- **Model zoo** — ready-to-run models for object detection, segmentation,
  classification
- **OpenVX / TIDL** — leverages on-chip DSP and MMA accelerators
- **Supported on** AM62AX and J7 Family (J721E, J721S2, etc.)

When to Choose EdgeAI
---------------------

Choose EdgeAI when:

- Your application requires **on-device AI inference** (computer vision, sensor
  fusion)
- You want to use **TI's deep learning accelerators** (MMA, C7x DSP) out of
  the box
- You need a **pre-validated model pipeline** without building the AI stack
  from scratch

----

Device × Distribution Compatibility Matrix
==========================================

The diagram below shows which distributions are available for each TI device
family. Cells marked **✓** (colored) are supported; cells marked **✗** (grey)
are not available. Colored cells with hyperlinks navigate to the corresponding
SDK documentation when viewed in a browser.

.. graphviz:: /graphviz/distro_compatibility_matrix.gv
   :caption: TI Processor SDK — Device × Distribution Compatibility Matrix

----

Choosing the Right Distribution
================================

The following decision guide helps you select the right distribution for your
project:

.. code-block:: text

   Start
     │
     ├─ Need AI/ML inference on-device?
     │     └─ YES → EdgeAI SDK (AM62AX, J7 Family)
     │
     ├─ Building an Android HMI / infotainment?
     │     └─ YES → Android SDK (AM62X, AM62PX, AM67A, J7 Family)
     │
     ├─ Smallest possible footprint / no package manager at runtime?
     │     └─ YES → Buildroot (AM62X, AM62LX)
     │
     ├─ Need fast iteration with apt / familiar Debian tools?
     │     └─ YES → Debian / Armbian (AM62X, AM62LX, AM62PX, AM64X)
     │
     └─ Production product / full customization / secure boot / OTA?
           └─ YES → Yocto Linux (all devices)

----

Additional Resources
=====================

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Resource
     - Link
   * - Yocto Project
     - `yoctoproject.org <https://www.yoctoproject.org/>`__
   * - OpenEmbedded layers
     - `layers.openembedded.org <https://layers.openembedded.org/>`__
   * - meta-ti-bsp layer
     - `git.ti.com/git/arago-project/meta-ti <https://git.ti.com/git/arago-project/meta-ti/>`__
   * - Armbian
     - `armbian.com <https://www.armbian.com/>`__
   * - Buildroot
     - `buildroot.org <https://buildroot.org/>`__
   * - buildroot-external-TI
     - `github.com/TexasInstruments/buildroot-external-TI <https://github.com/TexasInstruments/buildroot-external-TI>`__
   * - Android Open Source Project
     - `source.android.com <https://source.android.com/>`__
   * - TI E2E Community
     - `e2e.ti.com <https://e2e.ti.com/>`__
