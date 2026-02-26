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

Ubuntu images can be downloaded from `Install Ubuntu on NVIDIA Jetson <https://ubuntu.com/download/nvidia-jetson#jetson-agx-thor>`_:


..  * https://cdimage.ubuntu.com/releases/jammy/release/nvidia-tegra/ubuntu-24.04-preinstalled-server-arm64+tegra-jetson.img.xz
* Ubuntu Server 24.04:

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


Tests skipped or adapted during the certification
-------------------------------------------------

The following tests have been excluded from the :abbr:`CQA (Compliance Quality Assurance)` tests

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - \-
     - Jetson AGX Thor development kit don't have a CSI connector, so Camera testing was excluded from the scope of this certification.
   * - \-
     - Similarly, the development kit include a QSPF connector, but qualifying that generic interface wasn't part of the test scope for this certification.
   * - `2122577 <https://bugs.launchpad.net/riverside/+bug/2122577>`_
     - USB-C storage tests have been excluded as the 2 USB-C ports of the development kit were already reserved (one for flashing operations, and the other one to connect the power supply).


Known issues
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2142589 <https://bugs.launchpad.net/ubuntu-image/+bug/2142589>`_
     - A temporary issue with the `ubuntu-image tool <https://snapcraft.io/ubuntu-image>`_ used to build the release image caused the `/usr/sbin/start-stop-daemon` utility to be misaligned from its package checksum. Note that it doesn't impact the usage of the tool. This tool being part of the `dpkg` package, upgrading `dpkg` will fix this temporary issue.
   * - `2142602 <https://bugs.launchpad.net/ubuntu/+source/linux-nvidia-tegra-modules-signed/+bug/2142602>`_
     - The `stress-ng procfs <https://github.com/ColinIanKing/stress-ng>`_ stressor highlight an issue with the `rtl8852ce` wireless driver, leading to spurious kernel traces and a reboot of the device. This issue is currently under investigation and should be fixed soon via a kernel package update.
   * - `2140170 <https://bugs.launchpad.net/riverside/+bug/2140170>`_
     - By the time this image was tested, :ref:`the TensorRT installation instructions <classic/installation-noble:install cuda and tensorrt>` couldn't apply properly, leading to broken packages. That was due to a package dependency issue in NVIDIA's archive, which can be fixed using the `TensorRT workaround`_.
   * - `2140523 <https://bugs.launchpad.net/riverside/+bug/2140523>`_
     - A similar issue was observed with the `latest TensorRT NGC docker container <https://catalog.ngc.nvidia.com/orgs/nvidia/containers/tensorrt?version=26.01-py3>`_, preinstalled with an incompatible TensorRT version.
   * - `2140293 <https://bugs.launchpad.net/riverside/+bug/2140293>`_
     - The VPI sample applications `run-sample-cpp-pva` and `run-sample-py-pva` didn't run successfully on the test image. PVA support wasn't fully enabled in v1013 of the `linux-nvidia-tegra-jetson` kernel preinstalled in the image, but this has been fixed in v1016. Please run `apt update; apt upgrade; reboot` as root to upgrade the kernel.


TensorRT workaround
------------------

This Ubuntu image was tested with NVIDIA's Jetson Linux 38.4, which comes with CUDA runtime 13.0. However, the TensorRT runtime and sample application packages have a dependency on CUDA runtime 13.1.
In order to fix that temporary issue with :ref:`the TensorRT installation instructions <classic/installation-noble:install cuda and tensorrt>`, please use the following workaround
to install a version compatible with NVIDIA's Jetson Linux 38.4:

.. code-block:: bash

    LIBNVINFER_VER="10.13.3.9-1+cuda13.0"
    LIBNVINFER_PKGS=(
      libnvinfer{-bin,-samples,-lean-dev,-dev,-dispatch-dev,-plugin-dev,-vc-plugin-dev,10,-lean10,-plugin10,-vc-plugin10,-dispatch10,-headers-dev,-headers-plugin-dev}=$LIBNVINFER_VER
      libnvonnxparsers{-dev,10}=$LIBNVINFER_VER
    )
    sudo DEBIAN_FRONTEND=noninteractive apt install -y --allow-downgrades \
      "${LIBNVINFER_PKGS[@]}"


Report Bugs
-----------

To report a bug, identify the related package in https://launchpad.net/ubuntu , create a bug, then subscribe the team ``ubuntu-tegra`` to it. For firmware related issues, report a bug `in the launchpad project <https://launchpad.net/ubuntu/+source/nvidia-tegra-defaults>`_.
