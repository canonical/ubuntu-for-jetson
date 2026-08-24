.. _build-boot-firmware:

Build custom boot firmware
==========================

The Jetson boot firmware spans NVIDIA-specific early-boot stages — BootROM through MB2 —
followed by UEFI (built from NVIDIA's EDK2 fork), then the standard shim–GRUB chain.
Customizing the firmware means either obtaining and rebuilding NVIDIA's L4T sources or working
with the prebuilt tarball that ships alongside the Ubuntu Jetson image.
See :doc:`/explanation/boot-chain` for an overview of every stage and how they fit together.


Prerequisites
-------------

* An x86 host running Ubuntu (the same requirement as :doc:`/how-to/flash`).
* The L4T boot firmware tarball that accompanies the Ubuntu Jetson image, available from the
  `Ubuntu download page for NVIDIA Jetson`_.
* For source-level changes: access to the L4T sources from NVIDIA's developer program, available
  from the `NVIDIA Jetson Linux developer page`_.


Get the firmware sources
------------------------

Boot firmware source is owned by NVIDIA.
Canonical does not redistribute the L4T sources; you obtain them from NVIDIA under NVIDIA's
developer program and build them per NVIDIA's developer guide.
See :doc:`/reference/artifacts` for the prebuilt artifacts that ship with the Ubuntu image.


Use the prebuilt firmware instead
---------------------------------

For most workflows the prebuilt tarball published with the Ubuntu Jetson image is sufficient.
The L4T flashing scripts ship inside it, selected by device:

* ``l4t_initrd_flash.sh`` — QSPI flashing on AGX Thor.
* ``flash.sh`` — QSPI flashing on AGX Orin and Orin Nano / NX.
* ``l4t_backup_restore.sh`` — internal-storage (eMMC / NVMe) writes.

For the full BSP extras — SDK Manager, sample root filesystem, and toolchains — download the
JetPack SDK or Jetson Linux release from the `NVIDIA Jetson Linux developer page`_.


Program the firmware
--------------------

To program the QSPI firmware, first put your board into recovery mode, then run the appropriate
L4T script for your device.
The complete procedure — including recovery mode steps and the exact flash commands for each
board variant — is in :doc:`/how-to/flash`.


Customize the Secure Boot keys
------------------------------

Secure Boot key ownership on Jetson is user-controlled: PK, KEK, and db are provisioned by you,
not by NVIDIA.
To generate PK, KEK, and db key pairs, insert the Microsoft KEK and db certificates, build the
UEFI-keys DTBO, and run ``l4t_initrd_flash.sh --uefi-keys``, follow :doc:`/how-to/secure-boot`.


Next steps
----------

* :doc:`/how-to/customize-device-tree` — change or merge the device tree before you flash it.
* :doc:`/how-to/flash` — full recovery mode and QSPI programming procedure.

.. _Ubuntu download page for NVIDIA Jetson: https://ubuntu.com/download/nvidia-jetson
.. _NVIDIA Jetson Linux developer page: https://developer.nvidia.com/embedded/jetson-linux
