.. _rn_classic_noble_thor_ga:

========================================================
Ubuntu for Jetson 24.04 Server Thor GA release (Noble)
========================================================


*2026-02 Release Notes*


Purpose
-------

This is the General Availability release of Ubuntu 24.04 for Jetson AGX Thor.

Images
------

Ubuntu images can be downloaded from https://ubuntu.com/download/nvidia-jetson:


* Ubuntu Server 24.04:

..  * https://cdimage.ubuntu.com/releases/jammy/release/nvidia-tegra/ubuntu-24.04-preinstalled-server-arm64+tegra-jetson.img.xz
  * https://cdimage.ubuntu.com/nvidia-tegra/ubuntu-server/noble/daily-preinstalled/manual/noble-preinstalled-server-arm64+tegra-jetson.img.xz (TBR)
  * Image SHA256SUM: ``cbb71942162adb0cf02ea0b80fac67930fb5c3a0845d26cb741d742a279d91fd``

* Boot firmware 38.4:

  * https://developer.nvidia.com/downloads/embedded/L4T/r38_Release_v4.0/release/Jetson_Linux_R38.4.0_aarch64.tbz2
  * Image SHA256SUM: ``6bb0dd0786f0fe9fbd0cbcc48bce33b778f01972cdfcdf5d6f73ac8b46f90f67``

Hardware Platforms Tested
-------------------------


* `Jetson AGX Thor Developer Kit`_ (TBR)

.. _Jetson AGX Thor Developer Kit: https://certification.canonical.com/hardware/202508-37859
.. https://ubuntu.com/certified/202508-37859

Release Highlights
------------------


* This release only targets Jetson AGX Thor, it does not support the Jetson Orin development kits.
* This release enables :doc:`/classic/installation-noble`.
* Canonical QA team has been running intensive testing of this release in order to qualify it as Ubuntu certified on the hardware platform referenced below:

  * `Jetson AGX Thor Developer Kit`_


Recent fixes
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2122501 <https://bugs.launchpad.net/riverside/+bug/2122501>`_
     - The Bluetooth controller firmware `rtl8852cu_fw` `has been packaged <https://bugs.launchpad.net/ubuntu/+source/linux-firmware-nvidia-tegra/+bug/2127473>`_ in the linux-firmware-nvidia-tegra.


Known issues
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2139652 <https://bugs.launchpad.net/riverside/+bug/2139652>`_
     - A temporary issue with the `ubuntu-image tool <https://snapcraft.io/ubuntu-image>`_ used to build the release image caused the `/usr/sbin/start-stop-daemon` utility to be misaligned from its package checksum. Note that it doesn't affect the usage of the tool. This tool being part of the `dpkg` package, upgrading dpkg will fix this temporary issue.


Report Bugs
-----------

To report a bug, identify the related package in https://launchpad.net/ubuntu , create a bug, then subscribe the team ``ubuntu-tegra`` to it. For firmware related issues, report a bug `in the launchpad project <https://launchpad.net/ubuntu/+source/nvidia-tegra-defaults>`_.
