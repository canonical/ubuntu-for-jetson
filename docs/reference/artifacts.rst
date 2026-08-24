.. _artifacts:

Jetson artifact sources
=======================

Consolidated source locations for all Jetson build artifacts.

.. list-table:: Ubuntu images and firmware
   :header-rows: 1
   :widths: 45 55

   * - Artifact
     - Source
   * - Ubuntu Server images and boot firmware tarball
     - `Ubuntu download page for NVIDIA Jetson`_
   * - Full JetPack SDK / Jetson Linux BSP
     - `NVIDIA Jetson Linux release page`_

.. list-table:: Kernel artifacts
   :header-rows: 1
   :widths: 45 55

   * - Artifact
     - Source
   * - Installed meta-package (``linux-nvidia-tegra-jetson``)
     - Binary meta-package; not available via ``apt source``
   * - Kernel source package, Ubuntu 22.04 LTS (``linux-nvidia-tegra-igx``)
     - `linux-nvidia-tegra-igx on Launchpad`_
   * - Kernel source package, Ubuntu 24.04 LTS (``linux-nvidia-tegra``)
     - `linux-nvidia-tegra on Launchpad`_
   * - Kernel snap (``tegra-kernel``)
     - `tegra-kernel on Snap Store`_

.. list-table:: Core image components
   :header-rows: 1
   :widths: 45 55

   * - Artifact
     - Source
   * - Gadget snap (``tegra``)
     - `tegra gadget on Snap Store`_
   * - Gadget snap source
     - `tegra-gadget source on Launchpad`_
   * - Ubuntu Core model assertions
     - `Jetson model assertions on GitHub`_

.. list-table:: Device tree sources
   :header-rows: 1
   :widths: 45 55

   * - Artifact
     - Source
   * - In-tree DTS files
     - ``arch/arm64/boot/dts/nvidia`` in the kernel source package
   * - Reference AGX Orin DTS
     - `Jetson AGX Orin DTS on Launchpad`_
   * - Stock prebuilt DTBs
     - Shipped in the boot firmware tarball (see `Ubuntu download page for NVIDIA Jetson`_)

See :doc:`/how-to/build-kernel`, :doc:`/how-to/build-server-image`,
:doc:`/how-to/build-core-image`, and :doc:`/how-to/flash` for procedures
that use these artifacts.

.. _Ubuntu download page for NVIDIA Jetson: https://ubuntu.com/download/nvidia-jetson
.. _NVIDIA Jetson Linux release page: https://developer.nvidia.com/embedded/jetson-linux
.. _linux-nvidia-tegra-igx on Launchpad: https://launchpad.net/ubuntu/+source/linux-nvidia-tegra-igx
.. _linux-nvidia-tegra on Launchpad: https://launchpad.net/ubuntu/+source/linux-nvidia-tegra
.. _tegra-kernel on Snap Store: https://snapcraft.io/tegra-kernel
.. _tegra gadget on Snap Store: https://snapcraft.io/tegra
.. _tegra-gadget source on Launchpad: https://code.launchpad.net/~ubuntu-tegra/+git/tegra-gadget
.. _Jetson model assertions on GitHub: https://github.com/canonical/models/tree/master/devices/nvidia/jetson
.. _Jetson AGX Orin DTS on Launchpad: https://git.launchpad.net/~riverside-team/+git/sidecar-device-trees/tree/latest/jetson-agx-orin.dts
