.. _device-trees:

Device trees on Jetson
======================

Jetson is device-tree based; the standard arm64 ACPI boot path does not apply.

Where the DTB lives
-------------------

The Device Tree Binary (DTB) is programmed alongside the boot firmware in the QSPI flash on the
System-on-Module (SoM). Updating the device tree therefore requires flashing QSPI again.

GRUB and overlays
-----------------

Ubuntu on Jetson boots through GRUB rather than NVIDIA's stock ``extlinux`` loader. GRUB does not
apply Device Tree Overlays (DTBOs): the only mechanism available is whole-DTB replacement via
GRUB's non-secure ``devicetree`` command.

As a consequence, overlays must be merged offline into the base DTB before the image is flashed.
For bring-up and development scenarios, a DTBO can be flashed alongside the firmware using the
``ADDITIONAL_DTB_OVERLAY`` argument as documented in :doc:`/how-to/flash`.

Further reading
---------------

- :doc:`/how-to/customize-device-tree` — produce and flash a customized DTB
- :doc:`/how-to/flash` — QSPI flashing and the ``ADDITIONAL_DTB_OVERLAY`` option
