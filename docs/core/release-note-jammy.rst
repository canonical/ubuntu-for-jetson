.. _rn_core_jammy:

======================================
Ubuntu Core 22 for Jetson (Jammy)
======================================


*2026-05 Release Notes*


Purpose
-------

This is the General Availability release of Ubuntu Core 22 for Jetson. All release assets are provided by Canonical.

Images
------

Ubuntu images can be downloaded from https://ubuntu.com/download/nvidia-jetson:


* Ubuntu Core 22:

  * https://cdimage.ubuntu.com/nvidia-tegra/ubuntu-core/22/stable/manual/ubuntu-core-22-arm64+tegra-jetson.img.xz
  * Image SHA256SUM: ``a051ca3667e6410ec6dd4b4bf048dcfac0c4b3bd8b49552530f455a7e052b881``

* Boot firmware 36.5:

  * https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v5.0/release/Jetson_Linux_r36.5.0_aarch64.tbz2
  * Image SHA256SUM: ``414e58d97ac4b84fb02cbca621d46598f0bc8b811b6b9c3ad778b04e8d321ca7``

Hardware Platforms Tested
-------------------------


* `Jetson AGX Orin Developer kit`_
* `Jetson Orin Nano Developer kit`_
* `Jetson Orin NX SOM on Jetson Orin Nano Developer kit`_

Release Highlights
------------------


* First Ubuntu Core image release for Tegra platforms
* Full Disk Encryption and secure boot support. Full Disk Encryption will be automatically enabled when hardware support is detected. For enabling secure boot, refer to :doc:`the secure boot instructions </how-to/secure-boot>`
* Strictly confined applications
* OTA updates
* Nvidia introduced Nano Super power mode with Jetpack 6.2. To enable this power mode, as described in `the Jetson Linux Developer Guide`_, requires both `flashing JetPack`_ with a specific configuration (the ubuntu image must be reinstalled afterwards), and switching to a specific power mode using nvpmodel command (refer to the `snap samples`_)
* Canonical QA team has been running intensive testing of this release in order to qualify it as Ubuntu certified on the three hardware platforms referenced below:

  * `Jetson AGX Orin Developer kit`_
  * `Jetson Orin Nano Developer kit`_
  * `Jetson Orin NX SOM on Jetson Orin Nano Developer kit`_

.. _flashing Jetpack: https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/IN/QuickStart.html#to-flash-the-jetson-developer-kit-operating-software
.. _snap samples: https://github.com/canonical/tegra-snap-samples/tree/main/nvpmodel
.. _the Jetson Linux Developer Guide: https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/PlatformPowerAndPerformance/JetsonOrinNanoSeriesJetsonOrinNxSeriesAndJetsonAgxOrinSeries.html#supported-modes-and-power-efficiency


.. _Jetson AGX Orin Developer kit: https://ubuntu.com/certified/202406-34151
.. _Jetson Orin Nano Developer kit: https://ubuntu.com/certified/202406-34152
.. _Jetson Orin NX SOM on Jetson Orin Nano Developer kit: https://ubuntu.com/certified/202407-34213


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

The following tests have been excluded from the :abbr:`CQA (Compliance Quality Assurance)` tests

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
     - On AGX, after installing the ``nvpmodel`` snap, all CPU governor tests on policy 8 failed. That’s because the snap will install and apply the default related model.
   * - `2083009 <https://bugs.launchpad.net/riverside/+bug/2083009>`_
     - Similarly, on NX, after installing the ``nvpmodel`` snap, all CPU governor tests on policy 4 failed
   * - `2091684 <https://bugs.launchpad.net/riverside/+bug/2091684>`_
     - When a monitor is connected to the device, the gstreamer transcoding might be considerably slower than without.
   * - `2150448 <https://bugs.launchpad.net/riverside/+bug/2150448>`_
     - While running the gstreamer image capture pipelines described in the `tegra snap samples repository`_, the pipeline can return an error code of 1 even though the image gets captured correctly. This is due to a bug in the nvarguscamerasrc plugin that will fail to clean up the pipeline correctly.
   * - NA
     - Running LXD and Docker on the same host can cause `connectivity issues <https://documentation.ubuntu.com/lxd/en/latest/howto/network_bridge_firewalld/#prevent-connectivity-issues-with-lxd-and-docker>`_. This is something to keep in mind after installing Nvidia Container runtime.

.. _tegra snap samples repository: https://github.com/canonical/tegra-snap-samples/tree/main/multimedia#camera-capture-using-gstreamer


Report Bugs
-----------

If a bug is found in a specific snap, bugs should be reported against that specific snap using the contact on that snap's page on https://snapcraft.io. If a generic Ubuntu Core system bug is discovered, please report it to snapd under https://bugs.launchpad.net/snapd/+filebug. For firmware related issues, report a bug `in the launchpad project <https://launchpad.net/ubuntu/+source/linux-firmware-nvidia-tegra>`_.
