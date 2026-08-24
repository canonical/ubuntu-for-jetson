.. _build-image:

Create a custom Ubuntu image
========================================================


Prerequisites
-------------

* `Ubuntu-image`_ is the tool used by Canonical to build official Ubuntu images. We recommend running Ubuntu 24.04 LTS (Noble Numbat) on the build environment while the minimum requirement is Ubuntu 22.04 LTS (Jammy Jellyfish).

.. _Ubuntu-image: https://github.com/canonical/ubuntu-image

.. code-block:: bash

    sudo snap install --classic --channel latest/stable ubuntu-image


.. toctree::
   :maxdepth: 1

   build-server-image
   build-core-image
