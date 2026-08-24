.. _build-kernel:

Build a custom kernel
=====================

A Jetson system installs the ``linux-nvidia-tegra-jetson`` meta-package, a vendor-flavored
kernel carrying NVIDIA's BSP patches and Tegra-specific drivers. The underlying kernel
source package differs by series: ``linux-nvidia-tegra-igx`` on Ubuntu 22.04 LTS (Orin
family) and ``linux-nvidia-tegra`` on Ubuntu 24.04 LTS (AGX Thor). For Ubuntu Core, the
kernel is packaged as the ``tegra-kernel`` snap. For background on why Jetson uses a
vendor kernel rather than the stock ``linux-generic``, see :doc:`/explanation/kernel`.


Prerequisites
-------------

* An Ubuntu build host. For amd64 hosts, see :doc:`/how-to/cross-build` first.
* The kernel source package for your series, fetched via ``apt source`` (see below).


Get the kernel source
---------------------

Fetch the kernel source package for your Ubuntu release:

.. tabs::

   .. group-tab:: Ubuntu 22.04 LTS (Jammy Jellyfish)

      .. code-block:: bash

          apt source linux-nvidia-tegra-igx

   .. group-tab:: Ubuntu 24.04 LTS (Noble Numbat)

      .. code-block:: bash

          apt source linux-nvidia-tegra

.. note::

   ``apt source linux-nvidia-tegra-jetson`` does not work because
   ``linux-nvidia-tegra-jetson`` is a binary meta-package, not a source package.

The source packages are published on Launchpad: `linux-nvidia-tegra-igx on Launchpad`_
(Ubuntu 22.04 LTS) and `linux-nvidia-tegra on Launchpad`_ (Ubuntu 24.04 LTS). For Ubuntu
Core, the equivalent is the ``tegra-kernel`` snap (`tegra-kernel snap on Snapcraft`_),
built from the same source tree.


Customize the configuration
---------------------------

Use the standard Ubuntu kernel-team annotations workflow to modify the kernel configuration.
See :external+kernel:doc:`how-to/develop-customise/build-kernel` for the full procedure,
including how to edit annotation files and rebuild the configuration.


Build a kernel snap for Ubuntu Core
-----------------------------------

To produce a ``tegra-kernel`` snap from source, follow
:external+kernel:doc:`how-to/develop-customise/build-kernel-snap`. The resulting snap is
consumed directly by the Ubuntu Core image build described in
:doc:`/how-to/build-core-image`.


Device trees
------------

In-tree Jetson device tree sources live under ``arch/arm64/boot/dts/nvidia`` in the
kernel source tree. The built DTB is programmed alongside the boot
firmware in QSPI. To modify a device tree or produce a merged overlay, see
:doc:`/how-to/customize-device-tree`.


Next steps
----------

* :doc:`/how-to/build-server-image` — embed the kernel deb in a custom Ubuntu Server image.
* :doc:`/how-to/build-core-image` — assemble an Ubuntu Core image with a ``tegra-kernel`` snap.
* :doc:`/how-to/flash` — write firmware and image to a Jetson device.


.. _linux-nvidia-tegra-igx on Launchpad: https://launchpad.net/ubuntu/+source/linux-nvidia-tegra-igx
.. _linux-nvidia-tegra on Launchpad: https://launchpad.net/ubuntu/+source/linux-nvidia-tegra
.. _tegra-kernel snap on Snapcraft: https://snapcraft.io/tegra-kernel
