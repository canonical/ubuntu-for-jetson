.. _build-image:

Create a custom Ubuntu image
========================================================


Prerequisites
-------------

* `Ubuntu-image`_ is the tool used by Canonical to build official Ubuntu images. We recommend running Ubuntu 24.04 (Noble Numbat) on the build environment while the minimum requirement is Ubuntu 22.04 (Jammy Jellyfish).

.. _Ubuntu-image: https://github.com/canonical/ubuntu-image

.. code-block:: bash

    sudo snap install --classic --channel latest/stable ubuntu-image

To build a custom kernel for inclusion in the image, see :doc:`build-kernel`. To modify or
overlay the device tree before flashing, see :doc:`customize-device-tree`. To produce arm64
artifacts — including the image itself — on an amd64 host, see :doc:`cross-build`.


.. toctree::
   :maxdepth: 1

   build-server-image
   build-core-image
