.. _customize-device-tree:

Customize the device tree
=========================

The Jetson device tree is programmed into QSPI flash alongside the boot firmware.
Changing it requires editing or overlaying the device tree source and then flashing QSPI again.
See :doc:`/explanation/device-trees` for background on why GRUB handles overlays differently
from the stock Jetson boot path and why whole-DTB replacement is the only runtime option.


Prerequisites
-------------

* The kernel source package for your series, which you can fetch with:

  .. tabs::

     .. group-tab:: Ubuntu 22.04 LTS (Jammy Jellyfish)

        .. code-block:: bash

            apt source linux-nvidia-tegra-igx

     .. group-tab:: Ubuntu 24.04 LTS (Noble Numbat)

        .. code-block:: bash

            apt source linux-nvidia-tegra

  DTS files live under ``arch/arm64/boot/dts/nvidia`` in either source tree.

* The ``device-tree-compiler`` package, installed as part of the :doc:`/how-to/flash`
  preparation step.
* The L4T boot firmware tarball from the `Ubuntu download page for NVIDIA Jetson`_; stock
  prebuilt DTBs ship next to UEFI inside it.


Edit the device tree source
---------------------------

Inherit the upstream DTS that matches your SoC and carrier board from the kernel source package
for your release, apply your changes, and rebuild the kernel package so the resulting DTB is
available for flashing.
See :doc:`/how-to/build-kernel` for the kernel rebuild workflow.

A public reference DTS for the AGX Orin is available at the
`Jetson AGX Orin reference DTS`_ on Launchpad.


Merge an overlay offline
------------------------

GRUB does not apply Device Tree Overlays (DTBOs).
Only whole-DTB replacement is available at runtime, through the non-secure ``devicetree`` command.
To use an overlay in production, merge it into the base DTB offline using ``dtc`` from the
``device-tree-compiler`` package, then flash the resulting merged DTB to QSPI.


Flash the device tree
---------------------

The DTB lives in QSPI, so you program it together with the boot firmware.
Follow the QSPI programming steps in :doc:`/how-to/flash`.

For camera bring-up on development kits, the ``ADDITIONAL_DTB_OVERLAY`` variant documented in
:doc:`/how-to/flash` flashes a ``.dtbo`` file alongside the firmware without a full offline merge.

For the full Jetson module adaptation workflow, refer to the
`NVIDIA Jetson Module Adaptation and Bring-Up guide`_ and, for NX / Nano variants, the
`NVIDIA guide on updating DTB files`_.

.. _Ubuntu download page for NVIDIA Jetson: https://ubuntu.com/download/nvidia-jetson
.. _Jetson AGX Orin reference DTS: https://git.launchpad.net/~riverside-team/+git/sidecar-device-trees/tree/latest/jetson-agx-orin.dts
.. _NVIDIA Jetson Module Adaptation and Bring-Up guide: https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/HR/JetsonModuleAdaptationAndBringUp.html
.. _NVIDIA guide on updating DTB files: https://docs.nvidia.com/jetson/archives/r36.4.3/DeveloperGuide/HR/JetsonModuleAdaptationAndBringUp/JetsonOrinNxNanoSeries.html#updating-dtb-files
