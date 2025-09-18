.. _rn_classic_noble_thor_ea:

========================================================
Ubuntu for Jetson 24.04 Server Thor EA release (Noble)
========================================================


*2025-10 Release Notes*


Purpose
-------

This is the Early Access release of Ubuntu 24.04 for Jetson AGX Thor.

Images
------

Ubuntu images can be downloaded from https://ubuntu.com/download/nvidia-jetson:


* Ubuntu Server 24.04:

  * https://people.canonical.com/~platform/images/nvidia-tegra/ubuntu-24.04-preinstalled-server-thor-arm64+jetson.img.xz
  * Image SHA256SUM: ``6724ba86126228c989ae794bf2472a8109e3354fa7411becf089706a7e041a3e``

* Boot firmware 38.2:

  * https://developer.nvidia.com/downloads/embedded/L4T/r38_Release_v2.0/release/Jetson_Linux_R38.2.0_aarch64.tbz2
  * Image SHA256SUM: ``bef352597d8ebcb1bedff1f2abef5957dbd34faa8c8cff7d9d1e576c18c7a46d``

Hardware Platforms Tested
-------------------------


* Jetson AGX Thor Developer Kit

Release Highlights
------------------


* This is an early access release only targeting Jetson AGX Thor, it does not support the Jetson Orin development kits.
* At this early stage, camera testing has been deferred to a later release.
* The Linux kernel used on the Jetson AGX Thor (`̀ linux-nvidia-tegra-ppadev-jetson``) is under active development and has not yet been added to the official archive. Please note that this kernel is not stable, and future updates may introduce unexpected regressions.
* Migrating from this Early Access image to a future release will require switching to a different kernel meta-package.


Known issues
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2122501 <https://bugs.launchpad.net/riverside/+bug/2122501>`_
     - The Bluetooth controller firmware ``rtl8852cu_fw`` is currently missing during boot, which prevents the Bluetooth service from functioning correctly. This issue will be resolved in an upcoming update to the ``linux-firmware-nvidia-tegra`` package. As a workaround, installing the ``nvidia-l4t-firmware`` package can resolve the issue immediately. Refer to the documentation here: "TODO: add link".
   * - `2121984 <https://bugs.launchpad.net/riverside/+bug/2121984>`_
     - Connecting to a WPA3 802.11ax access point currently results in a kernel crash. This issue is still under investigation but can be avoided by `Updating the development kernel`_.
   * - `2122572 <https://bugs.launchpad.net/riverside/+bug/2122572>`_
     - The system currently exits suspend mode automatically after a few seconds, instead of waiting for a proper wake-up event (e.g., RTC alarm, keyboard input). This premature wake-up behaviour is a known issue and is expected to be resolved in an upcoming kernel update.
   * - `2122629 <https://bugs.launchpad.net/riverside/+bug/2122629>`_
     - Some of the stress tests performed are based on `stress-ng <https://github.com/ColinIanKing/stress-ng>`_: ``stress-ng --af-alg 0 --timeout 30 --oom-avoid-bytes 10% --skip-silent --verbose``. This command fails with the following error: ``tegra-se 8188120000.crypto: failed to allocate key slot``. This issue is currently under investigation but can be avoided by `Updating the development kernel`_.
   * - `2120690 <https://bugs.launchpad.net/riverside/+bug/2120690>`_
     - Resuming the system from suspend mode may lead to a system freeze under specific conditions. This issue has only been observed when running the following command: ``sudo fwts uefirtmisc`` (from the ``fwts`` package). When this occurs, a full system reboot is required to recover.
   * - `2122571 <https://bugs.launchpad.net/riverside/+bug/2122571>`_
     - Resuming the system from suspend mode may result in a black screen when connected to a 4K monitor. When this occurs, the system does not recover automatically, and a reboot is required.
   * - `2122580 <https://bugs.launchpad.net/riverside/+bug/2122580>`_
     - Audio output is limited to the first connected monitor via DisplayPort or HDMI. If a second monitor is connected, or if the monitor is reconnected to a different port, sound output will stop functioning. A system reboot is required to restore audio playback.
   * - `2123881 <https://bugs.launchpad.net/riverside/+bug/2123881>`_
     - GStreamer pipelines that perform transcoding from H.265 to AV1 currently fail with an error. This issue is under investigation.
   * - `2115050 <https://bugs.launchpad.net/riverside/+bug/2115050>`_
     - Automatic power-on of the Jetson AGX Thor development kit may fail if a USB-C cable is connected to another machine. Depending on the automation header configuration, the system is expected to boot automatically upon power-up, but this does not occur in this scenario. Manual intervention is required to start the system. NVIDIA is expected to release a firmware update soon to address this issue.
   * - `2121989 <https://bugs.launchpad.net/riverside/+bug/2121989>`_
     - The message "watchdog0: watchdog did not stop" appears during reboot. This is a known issue and is harmless.


Updating the development kernel
-------------------------------

To update the development kernel, please run the following commands

.. code-block:: bash

  sudo add-apt-repository ppa:ubuntu-tegra/kernel-daily
  sudo apt update
  sudo apt install linux-nvidia-tegra-ppadev-jetson


Report Bugs
-----------

To report a bug, identify the related package in https://launchpad.net/ubuntu , create a bug, then subscribe the team ``ubuntu-tegra`` to it. For firmware related issues, report a bug `in the launchpad project <https://launchpad.net/ubuntu/+source/nvidia-tegra-defaults>`_.
