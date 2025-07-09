.. _rn_classic_jammy:

======================================
Ubuntu for Jetson 22.04 Server (Jammy)
======================================


*2025-02 Release Notes*

 
Purpose
-------

This is the General Availability release of Ubuntu 22.04 for Jetson. All release assets are provided by Canonical.

Images
------

Ubuntu images can be downloaded from https://ubuntu.com/download/nvidia-jetson:


* Ubuntu Server 22.04:

  * https://cdimage.ubuntu.com/releases/jammy/release/nvidia-tegra/ubuntu-22.04-preinstalled-server-arm64+tegra-jetson.img.xz
  * Image SHA256SUM: ``27c54b9f3a23b4c8a6a8490cc41281061b359fea031c44ebdf7932316331f68a``

* Boot firmware 36.4.3:

  * https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v4.3/release/Jetson_Linux_r36.4.3_aarch64.tbz2
  * Image SHA256SUM: ``949a44049c4ce6a8efdf572ea0820c874f6ee5d41ca3e4935b9f0e38d11873d2``

Hardware Platforms Tested
-------------------------


* `Jetson AGX Orin Developer kit`_
* `Jetson Orin Nano Developer kit`_
* `Jetson Orin NX SOM on Jetson Orin Nano Developer kit`_

Release Highlights
------------------


* The kernel meta-package for Jetson devices is now changed to ``linux-nvidia-tegra-jetson`` (instead of ``linux-nvidia-tegra-igx`` ). Further details (\ `LP#2069179 <https://bugs.launchpad.net/riverside/+bug/2069179>`_\ ) : https://discourse.ubuntu.com/t/changes-to-ubuntu-for-tegra-kernel-metapackages-on-jetson-and-igx/48807
* Nvidia introduced Nano Super power mode with Jetpack 6.2. To enable this power mode, as described in https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/SD/PlatformPowerAndPerformance/JetsonOrinNanoSeriesJetsonOrinNxSeriesAndJetsonAgxOrinSeries.html#supported-modes-and-power-efficiency, it requires both `flashing JetPack <https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/IN/QuickStart.html#to-flash-the-jetson-developer-kit-operating-software>`_ with a specific configuration (the Ubuntu image must be reinstalled afterwards), and switching to a specific power mode using the ``nvpmodel`` command (installed by the ``nvidia-tegra-drivers-36`` packages, please refer to `Ubuntu 22.04 for NVIDIA Jetson Installation instructions <https://ubuntu.com/download/nvidia-jetson>`_\ ).
* Canonical QA team has been running intensive testing of this release in order to qualify it as Ubuntu certified on the three hardware platforms referenced below:

  * `Jetson AGX Orin Developer kit`_
  * `Jetson Orin Nano Developer kit`_
  * `Jetson Orin NX SOM on Jetson Orin Nano Developer kit`_

.. _Jetson AGX Orin Developer kit: https://ubuntu.com/certified/202406-34151
.. _Jetson Orin Nano Developer kit: https://ubuntu.com/certified/202406-34152
.. _Jetson Orin NX SOM on Jetson Orin Nano Developer kit: https://ubuntu.com/certified/202407-34213

Recent fixes
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2071409 <https://bugs.launchpad.net/riverside/+bug/2071409>`_
     - No video output after resuming from suspend
   * - `2081141 <https://bugs.launchpad.net/riverside/+bug/2081141>`_
     - 3 Failures during v4l2 compliance test execution
   * - `2081801 <https://bugs.launchpad.net/riverside/+bug/2081801>`_
     - No wireless device detected: fixed as wireless/bluetooth firmware files are now installed by default with ``linux-firmware-nvidia-tegra`` package in the Ubuntu image
   * - `2082057 <https://bugs.launchpad.net/riverside/+bug/2082057>`_
     - bluetooth no default controller available
   * - `2081802 <https://bugs.launchpad.net/riverside/+bug/2081802>`_
     - bluetooth beacon test failed
   * - `2081822 <https://bugs.launchpad.net/riverside/+bug/2081822>`_
     - Failed to connect to any Wifi access points: ``wpa_supplicant`` is now installed by default in the Ubuntu image
   * - `2089043 <https://bugs.launchpad.net/riverside/+bug/2089043>`_
     - Audio over DisplayPort only works when loading ``snd_hda_tegra`` and ``snd_hda_codec_hdmi`` in initramfs
   * - `2071428 <https://bugs.launchpad.net/riverside/+bug/2071428>`_
     - On AGX, ``eth0`` (ethernet interface) has been renamed to ``eno1`` in order to have a predictable name



Features not supported in this release
--------------------------------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2070419 <https://bugs.launchpad.net/riverside/+bug/2070419>`_
     - Wake-up from S5 using RTC alarm (Orin SoC)
   * - `2070428 <https://bugs.launchpad.net/riverside/+bug/2070428>`_
     - :abbr:`WOL (Wake on LAN)` from power off isn’t supported, and :abbr:`WOL` from suspend mode isn’t fully operational




Tests skipped or adapted during the certification
-------------------------------------------------

The following tests have been excluded from the :abbr:`CQA (Compliance Quality A???)` tests

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2071401 <https://bugs.launchpad.net/riverside/+bug/2071401>`_ skipped
     - RTC clock 1 (skipped) : the development kits don’t have an external battery included
   * - `2071402 <https://bugs.launchpad.net/riverside/+bug/2071402>`_ adapted
     - Thermal zones : Some of them aren’t readable on Nano and require a specific workaround on AGX
   * - `2071403 <https://bugs.launchpad.net/riverside/+bug/2071403>`_ skipped
     - ``CAAM`` cryptography tests are only applicable to NXP devices
   * - `2071404 <https://bugs.launchpad.net/riverside/+bug/2071404>`_ skipped
     - ``MCRC`` cryptography tests are only applicable to TI devices
   * - `2071405 <https://bugs.launchpad.net/riverside/+bug/2071405>`_ skipped
     - ``sa2ul`` cryptography tests are only applicable to TI devices
   * - `2071407 <https://bugs.launchpad.net/riverside/+bug/2071407>`_ skipped
     - ``Crypto: no hwrng support`` for Linux
   * - `2071416 <https://bugs.launchpad.net/riverside/+bug/2071416>`_ skipped
     - :abbr:`MTD` is only accessible in case of Recovery Mode boot
   * - `2071418 <https://bugs.launchpad.net/riverside/+bug/2071418>`_ adapted
     - Tests have been adapted to match the detection of 2 :abbr:`SPI` controllers with 2 :abbr:`CS` per :abbr:`SPI`
   * - `2071419 <https://bugs.launchpad.net/riverside/+bug/2071419>`_ skipped
     - Write to :abbr:`EEPROM` tests are not allowed on the development kits as that would break them
   * - `2071422 <https://bugs.launchpad.net/riverside/+bug/2071422>`_ skipped
     - :abbr:`SPI` physical tests were skipped because that requires defining a specific PIN :abbr:`MUX` configuration
   * - `2073232 <https://bugs.launchpad.net/riverside/+bug/2073232>`_ adapted
     - Orin doesn’t support waking up from offline mode, ``rtcwake`` tests have been adapted to test only resuming from suspend
   * - `2091263 <https://bugs.launchpad.net/riverside/+bug/2091263>`_ skipped
     - ``NVIDIA SOC i2c adapter 0`` does not support detection commands




Known issues
------------

.. list-table::
   :header-rows: 1

   * - Issue
     - Description
   * - `2061598 <https://bugs.launchpad.net/riverside/+bug/2061598>`_
     - On an Orin NX development kit, the very first flash of the :abbr:`QSPI` boot firmware might fail due to a write protection bit being set. In this case you need to perform an initrd flash of the :abbr:`QSPI` firmware (only necessary once to fix this issue) by following these instructions: https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/IN/QuickStart.html#to-flash-the-jetson-developer-kit-operating-software. After this operation, every subsequent flash of the :abbr:`QSPI` firmware will work the usual way.
   * - `2071321 <https://bugs.launchpad.net/riverside/+bug/2071321>`_
     - Part of the stress tests executed during the certification tests are based on https://github.com/ColinIanKing/stress-ng. The following command failed to run successfully on the AGX development kit: ``stress-ng --af-alg 0 --timeout 30 --skip-silent --verbose``. This issue is currently under investigation and should be fixed soon via a kernel package update. It does not affect the Nano/NX development kit because the related cryptography engines are not enabled with the current boot firmware version (a future release will also enable them).
   * - `2071414 <https://bugs.launchpad.net/riverside/+bug/2071414>`_
     - Netplan.io package doesn’t support ``WPA2-PSK-SHA256`` in its current Jammy version. While the corrective patch is already available on the latest Ubuntu version (1.1.2), the Jammy backport should get released later on this year.
   * - `2039983 <https://bugs.launchpad.net/riverside/+bug/2039983>`_
     - On AGX development kit, power cycling the device using an external power switch introduces a noise in the serial input buffer that can, depending on the nature of the power switch, pause the GRUB menu, or directly launch the default entry (action \= ‘Enter’).
   * - `2081138 <https://bugs.launchpad.net/riverside/+bug/2081138>`_
     - As part of the compliance tests for camera, we figured out that running  ``gst-device-monitor-1.0 Video/Source`` would not list any device. This is because ``gst-plugins-good1.0`` is released as v1.20 in Jammy, while this issue was resolved with a patch available with 1.24.
   * - `2081139 <https://bugs.launchpad.net/riverside/+bug/2081139>`_
     - Similarly, the command ``gst-device-monitor-1.0`` will output a few “GStreamer-CRITICAL” when a camera is connected to the devkit. This is because the tool will send a ``VIDIOC_QUERYCAP`` instead of a ``VIDIOC_SUBDEV_QUERYCAP`` for a sub device. This needs to be fixed first in ``gst-plugins-good1.0`` before getting released in Ubuntu.
   * - `2091684 <https://bugs.launchpad.net/riverside/+bug/2091684>`_
     - During tests, GStreamer pipelines involving hardware encoding (``nvv4l2h265enc``) had slower performance than expected (3x time slower). This issue isn’t easily reproducible and still under investigation.
   * - `2083007 <https://bugs.launchpad.net/riverside/+bug/2083007>`_
     - On AGX, after installing the ``nvidia-tegra-drivers-36`` packages, all CPU governor tests on policy 8 failed. That’s because the packages are installing ``nvpmodel`` and applying the default related model.
   * - `2083009 <https://bugs.launchpad.net/riverside/+bug/2083009>`_
     - Similarly, on NX, after installing the ``nvidia-tegra-drivers-36`` packages, all CPU governor tests on policy 4 failed
   * - `2097636 <https://bugs.launchpad.net/riverside/+bug/2097636>`_
     - While running the transcoding test pipelines described in `Ubuntu 22.04 for NVIDIA Jetson Installation instructions <https://ubuntu.com/download/nvidia-jetson>`_ : ``gst-launch-1.0 filesrc location=h264-reenc.mp4 ! qtdemux !   h264parse ! nvv4l2decoder ! nvv4l2av1enc ! matroskamux name=mux !   filesink location=av1-reenc.mkv -e`` thousands of error traces ``ParseObuFrameHeader: 2367: Invalid buf_idx = -1 or  offset`` are displayed. While this is looking suspicious, this trace isn’t actually preventing the command to finish properly and successfully.
   * - NA
     - Running LXD and Docker on the same host can cause `connectivity issues <https://documentation.ubuntu.com/lxd/en/latest/howto/network_bridge_firewalld/#prevent-connectivity-issues-with-lxd-and-docker>`_. This is something to keep in mind after installing Nvidia Container runtime.


Report Bugs
-----------

To report a bug, identify the related package in https://launchpad.net/ubuntu , create a bug, then subscribe the team ``ubuntu-tegra`` to it. For firmware related issues, report a bug `in the launchpad project <https://launchpad.net/ubuntu/+source/nvidia-tegra-defaults>`_.

