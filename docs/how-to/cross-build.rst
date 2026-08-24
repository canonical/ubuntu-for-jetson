.. _cross-build:

Cross-build Jetson artifacts on an amd64 host
=============================================

Every Jetson artifact — kernel deb, ``tegra-kernel`` snap, ``tegra`` gadget snap, QSPI boot
firmware, offline-merged DTBs, and the image assembled by ``ubuntu-image`` — can be produced
on an amd64 workstation. Jetson images are themselves cross-built on amd64 in CI.


Prerequisites
-------------

Install the cross toolchain and emulation support:

.. code-block:: bash

    sudo apt install crossbuild-essential-arm64 qemu-user-static

For snap builds, install snapcraft. For image assembly, install ``ubuntu-image`` as described
in :doc:`/how-to/build-image`.


Use a cross toolchain
---------------------

The cross toolchain is the complete, preferred path for all local builds.

**Kernel deb** — use ``sbuild`` with the ``--host`` flag:

.. code-block:: bash

    sbuild --host=arm64

**Kernel source** — pass the architecture and cross-compiler directly to ``make``:

.. code-block:: bash

    make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-

**Snaps** — declare the target platform in ``snapcraft.yaml``:

.. code-block:: yaml

    platforms:
      arm64:
        build-on:
          - amd64
        build-for:
          - arm64

Snapcraft builds the snap on the amd64 host and targets arm64. For a complete example, see
:external+snapcraft:doc:`how-to/integrations/craft-a-cross-compiled-app`.

**Image assembly** — ``ubuntu-image`` is architecture-agnostic and runs directly on the amd64
host without cross-compilation.

For a thorough reference on deb cross-building, see the `Debian CrossCompiling wiki`_.


Fall back to an emulated chroot
-------------------------------

If a build system resists cross-compilation, use ``qemu-user-static`` to run arm64 binaries
transparently on the amd64 host. This approach is 5–20 times slower than native but works for
any artifact.


Build on Launchpad builders
---------------------------

Launchpad builders handle archive-bound source packages and snaps:

* **Archive packages**: the ``linux-nvidia-tegra-igx`` kernel deb, signed shim, and GRUB are
  built as source packages uploaded to the Ubuntu archive.
* **Snaps**: ``tegra-kernel`` and ``tegra`` via Launchpad snap recipes.

Launchpad does **not** build QSPI boot firmware, offline-merged DTBs, or the ``ubuntu-image``
assembly step. Keep those steps local.


Next steps
----------

* :doc:`/how-to/build-kernel` — build the kernel deb or ``tegra-kernel`` snap.
* :doc:`/how-to/build-server-image` — write an image-definition YAML and run ``ubuntu-image classic`` to produce a preinstalled Ubuntu Server image.
* :doc:`/how-to/build-core-image` — fetch an official signed model assertion and run ``ubuntu-image snap`` to produce an Ubuntu Core image.


.. _Debian CrossCompiling wiki: https://wiki.debian.org/CrossCompiling
