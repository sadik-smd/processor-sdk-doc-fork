########
Overview
########

Buildroot is an open-source, simple, and efficient tool for building complete
Linux systems for embedded devices. It automates the process of generating
cross-compiled toolchains, root filesystems, Linux kernels, and bootloaders.
Buildroot is highly customizable, allowing developers to tailor their Linux
environment to meet the specific needs of their embedded projects.


**************************************
Buildroot Architecture for TI Devices
**************************************

The diagram below shows the Buildroot-based Linux embedded software distribution main components for TI devices.

The open-source Linux\ :sup:`®` distribution, based on the Buildroot build system, running on the Arm\ :sup:`®` Cortex\ :sup:`®`\-A processors, includes:

- **TI BSP Components** — the Texas Instruments Board Support Package, comprising:

  - The boot chain based on **TF-A** (Trusted Firmware-A) and **U-Boot**.
  - The secure monitor based on **TF-A** and **OP-TEE**, running on the Arm\ :sup:`®`
    Cortex\ :sup:`®`\-A in secure mode.
  - The **OP-TEE** secure OS running on the Arm\ :sup:`®` Cortex\ :sup:`®`\-A in secure mode,
    providing a Trusted Execution Environment (TEE) for Trusted Applications.
  - The **Linux Kernel** and **TI Kernel Drivers** running on the Arm\ :sup:`®`
    Cortex\ :sup:`®`\-A in non-secure mode, along with board-specific **Device Trees**.
  - The **TI Linux Firmware** providing co-processor firmwares
    (not applicable for AM62Lx-EVM).

- **Buildroot Build System** — consisting of:

  - **Buildroot Core**: the upstream Buildroot framework that drives the entire build.
  - **Buildroot External-TI**: the TI-specific external tree
    (`buildroot-external-TI <https://github.com/TexasInstruments/buildroot-external-TI>`__)
    that adds TI board configurations, packages, and patches without modifying upstream Buildroot.

- **Applications** — the user-space software layer built on top of the BSP, including
  GUI applications, custom applications, Trusted Applications running under OP-TEE,
  and middleware and libraries.

- **Build Artifacts** — the outputs produced by the Buildroot build system: an SD card
  image, individual boot artifacts, and a root filesystem.

.. note::

   Each component in the diagram below is a **clickable hyperlink** that navigates
   directly to the corresponding source repository or documentation.

.. container:: buildroot-arch-diagram

   .. graphviz::
      :caption: Buildroot-based Linux Architecture for TI Devices

      digraph buildroot_architecture {
          rankdir=TB;
          graph [splines=ortho, nodesep=0.7, ranksep=0.9,
                 fontname="Arial", pad=0.7, bgcolor="white"];
          node  [fontname="Arial", fontsize=7, margin=0];
          edge  [fontname="Arial", fontsize=13, penwidth=2.5,
                 arrowsize=1.5];

          subgraph cluster_outer {
              label="Buildroot-based Linux(R) Embedded Software for TI Devices";
              style="filled,bold";
              fillcolor="#F0F4FF";
              color="#1A237E";
              penwidth=5;
              fontsize=26;
              fontcolor="#1A237E";
              fontname="Arial Bold";

              legend [
                  shape=none,
                  label=<
                    <TABLE BORDER="2" CELLBORDER="0" CELLSPACING="4"
                           CELLPADDING="5" BGCOLOR="#FAFAFA"
                           COLOR="#424242" STYLE="ROUNDED">
                      <TR>
                        <TD COLSPAN="2" BGCOLOR="#424242"
                            STYLE="ROUNDED" WIDTH="150">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>Legend</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#2E7D32" STYLE="ROUNDED"
                            WIDTH="18" HEIGHT="18">
                          <FONT COLOR="white" POINT-SIZE="4"> </FONT>
                        </TD>
                        <TD ALIGN="LEFT" WIDTH="130">
                          <FONT COLOR="#2E7D32" POINT-SIZE="11">
                            <B>Applications Layer</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#E65100" STYLE="ROUNDED"
                            WIDTH="18" HEIGHT="18">
                          <FONT COLOR="white" POINT-SIZE="4"> </FONT>
                        </TD>
                        <TD ALIGN="LEFT" WIDTH="130">
                          <FONT COLOR="#E65100" POINT-SIZE="11">
                            <B>Buildroot Build System</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#4527A0" STYLE="ROUNDED"
                            WIDTH="18" HEIGHT="18">
                          <FONT COLOR="white" POINT-SIZE="4"> </FONT>
                        </TD>
                        <TD ALIGN="LEFT" WIDTH="130">
                          <FONT COLOR="#4527A0" POINT-SIZE="11">
                            <B>TI BSP Components</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#880E4F" STYLE="ROUNDED"
                            WIDTH="18" HEIGHT="18">
                          <FONT COLOR="white" POINT-SIZE="4"> </FONT>
                        </TD>
                        <TD ALIGN="LEFT" WIDTH="130">
                          <FONT COLOR="#880E4F" POINT-SIZE="11">
                            <B>Build Artifacts</B>
                          </FONT>
                        </TD>
                      </TR>
                    </TABLE>
                  >
              ];

              apps [
                  shape=none,
                  label=<
                    <TABLE BORDER="4" CELLBORDER="0" CELLSPACING="7"
                           CELLPADDING="7" BGCOLOR="#E8F5E9"
                           COLOR="#2E7D32" STYLE="ROUNDED">
                      <TR>
                        <TD COLSPAN="2" BGCOLOR="#2E7D32"
                            STYLE="ROUNDED" WIDTH="219">
                          <FONT COLOR="white" POINT-SIZE="15">
                            <B>&#9312; Applications</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#43A047" STYLE="ROUNDED"
                            WIDTH="103" HEIGHT="51">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>GUI Applications</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#43A047" STYLE="ROUNDED"
                            WIDTH="103" HEIGHT="51">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>Custom Applications</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#66BB6A" STYLE="ROUNDED"
                            WIDTH="103" HEIGHT="51">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>Trusted Applications</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#66BB6A" STYLE="ROUNDED"
                            WIDTH="103" HEIGHT="51">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>Middleware and Libraries</B>
                          </FONT>
                        </TD>
                      </TR>
                    </TABLE>
                  >
              ];

              buildroot [
                  shape=none,
                  label=<
                    <TABLE BORDER="4" CELLBORDER="0" CELLSPACING="7"
                           CELLPADDING="11" BGCOLOR="#FFFDE7"
                           COLOR="#E65100" STYLE="ROUNDED">
                      <TR>
                        <TD COLSPAN="2" BGCOLOR="#E65100"
                            STYLE="ROUNDED" WIDTH="336"
                            HREF="https://gitlab.com/buildroot.org/buildroot/"
                            TARGET="_blank"
                            TOOLTIP="Buildroot official repository">
                          <FONT COLOR="white" POINT-SIZE="23">
                            <B>&#9313; Buildroot Build System</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#F9A825" STYLE="ROUNDED"
                            WIDTH="162" HEIGHT="78"
                            HREF="https://gitlab.com/buildroot.org/buildroot/"
                            TARGET="_blank"
                            TOOLTIP="Buildroot Core repository">
                          <FONT COLOR="#3E2723" POINT-SIZE="19">
                            <B>Buildroot</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#FFB300" STYLE="ROUNDED"
                            WIDTH="162" HEIGHT="78"
                            HREF="https://github.com/TexasInstruments/buildroot-external-TI"
                            TARGET="_blank"
                            TOOLTIP="TI Buildroot external tree repository">
                          <FONT COLOR="#3E2723" POINT-SIZE="19">
                            <B>Buildroot External-TI</B>
                          </FONT>
                        </TD>
                      </TR>
                    </TABLE>
                  >
              ];

              artifacts [
                  shape=none,
                  label=<
                    <TABLE BORDER="4" CELLBORDER="0" CELLSPACING="7"
                           CELLPADDING="7" BGCOLOR="#F3E5F5"
                           COLOR="#880E4F" STYLE="ROUNDED">
                      <TR>
                        <TD BGCOLOR="#880E4F" STYLE="ROUNDED" WIDTH="147">
                          <FONT COLOR="white" POINT-SIZE="15">
                            <B>Build Artifacts</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#AD1457" STYLE="ROUNDED" HEIGHT="47">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>SD Card Image</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#C2185B" STYLE="ROUNDED" HEIGHT="47">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>Boot Artifacts</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#D81B60" STYLE="ROUNDED" HEIGHT="47">
                          <FONT COLOR="white" POINT-SIZE="13">
                            <B>Root Filesystem</B>
                          </FONT>
                        </TD>
                      </TR>
                    </TABLE>
                  >
              ];

              tibsp [
                  shape=none,
                  label=<
                    <TABLE BORDER="4" CELLBORDER="0" CELLSPACING="7"
                           CELLPADDING="7" BGCOLOR="#EDE7F6"
                           COLOR="#4527A0" STYLE="ROUNDED">
                      <TR>
                        <TD COLSPAN="4" BGCOLOR="#4527A0"
                            STYLE="ROUNDED" WIDTH="351">
                          <FONT COLOR="white" POINT-SIZE="15">
                            <B>&#9314; TI BSP Components</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD COLSPAN="4" BGCOLOR="#512DA8" STYLE="ROUNDED">
                          <FONT COLOR="white" POINT-SIZE="11">
                            <B>Firmware and Security Components</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#7B1FA2" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git/"
                            TARGET="_blank"
                            TOOLTIP="Trusted Firmware-A repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>TF-A</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#8E24AA" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://github.com/OP-TEE/optee_os"
                            TARGET="_blank"
                            TOOLTIP="OP-TEE OS repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>OP-TEE</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#AB47BC" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://git.ti.com/cgit/processor-firmware/ti-linux-firmware"
                            TARGET="_blank"
                            TOOLTIP="TI Linux Firmware repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>TI Linux Firmware</B>
                            <BR/>
                            <FONT POINT-SIZE="8">
                              (N/A for AM62Lx-EVM)
                            </FONT>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#9C27B0" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://github.com/u-boot/u-boot"
                            TARGET="_blank"
                            TOOLTIP="U-Boot repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>U-Boot</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD COLSPAN="4" BGCOLOR="#1565C0" STYLE="ROUNDED">
                          <FONT COLOR="white" POINT-SIZE="11">
                            <B>Linux Kernel Components</B>
                          </FONT>
                        </TD>
                      </TR>
                      <TR>
                        <TD BGCOLOR="#1976D2" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/"
                            TARGET="_blank"
                            TOOLTIP="Linux Kernel repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>TI Linux Kernel</B>
                          </FONT>
                        </TD>
                        <TD COLSPAN="2" BGCOLOR="#1E88E5"
                            STYLE="ROUNDED" HEIGHT="51"
                            HREF="https://git.ti.com/cgit/ti-linux-kernel/ti-linux-kernel"
                            TARGET="_blank"
                            TOOLTIP="TI Linux Kernel repository">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>TI Kernel Drivers</B>
                          </FONT>
                        </TD>
                        <TD BGCOLOR="#42A5F5" STYLE="ROUNDED"
                            WIDTH="81" HEIGHT="51"
                            HREF="https://github.com/torvalds/linux/tree/master/arch/arm64/boot/dts/ti"
                            TARGET="_blank"
                            TOOLTIP="TI Device Tree sources">
                          <FONT COLOR="white" POINT-SIZE="12">
                            <B>Device Tree</B>
                          </FONT>
                        </TD>
                      </TR>
                    </TABLE>
                  >
              ];
          }

          // Invisible anchor node for right column row 3
          anc_top [style=invis, width=0, height=0, label=""];

          // Row 1: apps (left) | legend (right)
          { rank=same; apps; legend; }
          // Row 2: buildroot (left) | artifacts (right)
          { rank=same; buildroot; artifacts; }
          // Row 3: tibsp (left) | anchor (right)
          { rank=same; tibsp; anc_top; }

          // Data-flow edges
          apps -> buildroot [
              color="#2E7D32",
              penwidth=3.0,
              arrowsize=1.5,
              fontcolor="#2E7D32",
              fontsize=13,
              fontname="Arial Bold"
          ];

          tibsp -> buildroot [
              color="#4527A0",
              penwidth=3.0,
              arrowsize=1.5,
              fontcolor="#4527A0",
              fontsize=13,
              fontname="Arial Bold"
          ];

          buildroot -> artifacts [
              color="#E65100",
              penwidth=3.5,
              arrowsize=1.8,
              label="  Output  ",
              fontcolor="#E65100",
              fontsize=13,
              fontname="Arial Bold",
              style=bold
          ];

          // Invisible ordering edges
          apps      -> buildroot [style=invis, weight=10];
          buildroot -> tibsp     [style=invis, weight=10];
          legend    -> artifacts [style=invis, weight=10];
          artifacts -> anc_top   [style=invis, weight=10];
      }

*************************
Key Features of Buildroot
*************************

#. **Simplicity**:
    Buildroot is designed to be easy to use and understand, making it
    accessible for developers with varying levels of experience in embedded
    systems.
#. **Customization**:
    With a vast library of packages and a flexible configuration system,
    Buildroot allows for extensive customization of the target system.
#. **Efficiency**:
    Buildroot’s minimalistic approach ensures that the generated system is lean
    and efficient, which is critical for resource-constrained embedded
    environments.
#. **Active Community**:
    A vibrant community of developers continually maintains and enhances
    Buildroot, providing regular updates and support.

***********************
Buildroot External Tree
***********************

TI provides a Buildroot external tree `Github/TexasInstruments <https://github.com/TexasInstruments/buildroot-external-TI>`__.
, to incorporate TI specific configurations and packages. This external
tree allows to customize and extend the Buildroot environment without modifying
the original Buildroot source code. It serves as a centralized repository for
custom configurations, ensuring consistency and ease of maintenance. The entire
project is made public and we accept community contributions as pull requests
to github repository.

Repository structure
====================

.. code-block:: text

   buildroot-external-TI
   ├── external.desc
   ├── external.mk
   ├── Config.in
   ├── board
   │   ├── ti
   │   │   ├── am62x-sk
   │   │   ├── am62lx-evm
   │   │   ├── common
   ├── COPYING
   ├── README.md
   ├── configs
   │   ├─ ti_release_am62x_sk_defconfig
   │   ├─ ti_release_am62x_sk_rt_defconfig
   │   ├─ ti_release_am62lx_evm_defconfig
   │   ├─ ti_release_am62lx_evm_rt_defconfig
   ├── package

:file:`external.desc`: contains name and description of br2-external tree.

:file:`configs/`: contains board specific configurtion files.

:file:`board/`: contains board specific files patches, genimage configuraion and hash file.

:file:`external.mk`: used to define package recipes.

:file:`Config.in`: used to define package recipes.

:file:`package`: used to add package.

.. _technical-support:

*****************
Technical Support
*****************

Technical support is a broad term. Our desire is to provide a solid
product, good documentation, and useful training that defines a clear
path for developing a product based on the Linux/Debian/Buildroot/RTOS/Android
SDKs. However, we know we'll never cover everything that can be done, and
occasionally we even make mistakes <gasp>. So, when you can't seem to
find what you need, there's a good place to search through previously
answered questions and ask a new one - The E2E Support Forums.

There is an active community of TIers and other customers like you
already using a TI Processor, on these forums. You may find your
question has already been answered with a quick Search of the Forums. If
not, a quick post will likely provide you the answers you need.

-  `E2E Processor Support
   <https://e2e.ti.com/support/processors/>`__
