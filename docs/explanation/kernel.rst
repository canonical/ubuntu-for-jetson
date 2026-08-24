.. _jetson-kernel:

The Jetson kernel
=================

Ubuntu on Jetson runs a vendor-flavored kernel rather than the stock ``linux-generic`` package.

The vendor kernel packages
--------------------------

A Jetson system installs the ``linux-nvidia-tegra-jetson`` meta-package. It carries
NVIDIA's BSP patches and the Tegra-specific drivers required by the Orin and Thor SoCs,
which are not present in the upstream ``linux-generic`` kernel. The underlying kernel
source package differs by Ubuntu release: ``linux-nvidia-tegra-igx`` on Ubuntu 22.04 LTS
(Orin family) and ``linux-nvidia-tegra`` on Ubuntu 24.04 LTS (AGX Thor).

The image is delivered as ``vmlinuz``, a gzip-wrapped ``Image.gz`` for arm64. GRUB loads
it through its standard ``linux``/``initrd`` flow once UEFI hands off execution. The load
path from UEFI onward matches a standard arm64 UEFI boot.

Ubuntu Core
-----------

For Ubuntu Core, the same kernel source is packaged as the ``tegra-kernel`` snap rather than as
a deb.

Further reading
---------------

- :doc:`/how-to/build-kernel` — customize and rebuild the Jetson kernel
- :doc:`/reference/artifacts` — source locations for the kernel package and snap
- :doc:`/core/index` — Ubuntu Core images for Jetson
