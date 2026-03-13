.. _build-image:

Create a custom Ubuntu Server image
===================================


Prerequisites
-------------

* `Ubuntu-image`_ is the tool used by Canonical to build official Ubuntu images. It must be installed on a build environment running Ubuntu 20.04 (Focal Fossa) or newer. It is recommended to use Ubuntu 24.04 (Noble Numbat).

.. _Ubuntu-image: https://github.com/canonical/ubuntu-image

.. code-block:: bash

    sudo snap install --classic --channel latest/stable ubuntu-image

Create an image definition file
-------------------------------

An image definition YAML file is required to build a custom Ubuntu Server image. This file specifies the required configurations, such as the ones listed below.

* **class**: Defines the type of image, such as `cloud`, `installer` or `preinstalled` (Canonical's certified Server images are preinstalled).
* **kernel**: Specifies the preinstalled kernel in the image. The official Ubuntu kernel for Jetson is "linux-nvidia-tegra-jetson", but it can be replaced with an alternative custom kernel, for instance hosted in a PPA for development purpose.
* **gadget**: Boot assets of an image.
* **customization**: Features such as particular snaps and packages that will come installed in the image.
* **artifacts**: Artifacts to create, including (but not limited to) the actual images, and manifest files.

For more details about each field, the `image definition`_ documentation can be consulted.

.. _image definition: https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/README.rst

The `ubuntu-images`_ repository serves as the official source for Canonical’s image definitions and scripts. It is used to build various Ubuntu versions and flavors. This includes optimized versions for Jetson Tegra hardware, currently supporting Ubuntu 22.04 (Jammy Jellyfish) and 24.04 (Noble Numbat).

.. _ubuntu-images: https://git.launchpad.net/ubuntu-images

Ubuntu certified Image definition files
---------------------------------------

The following command allows to check the definition files used to generate the Ubuntu Server image certified by Canonical:

.. _Certified Ubuntu Server Jammy image definition: https://git.launchpad.net/ubuntu-images/tree/ubuntu-server-tegra-jetson.yaml?h=jammy
.. _Certified Ubuntu Server Noble image definition: https://git.launchpad.net/ubuntu-images/tree/ubuntu-server-tegra-jetson.yaml?h=noble

.. tabs::

   .. group-tab:: Jammy

      `Certified Ubuntu Server Jammy image definition`_

   .. group-tab:: Noble

      `Certified Ubuntu Server Noble image definition`_

Build the Classic Server image
------------------------------

Once having the corresponding image definition file, the server image can be built through the following command.

.. code-block:: bash

    sudo ubuntu-image classic <image_definition>.yaml --debug

On the example above, the `--debug`  flag prints additional information about each step executed as part of the image build process, useful to troubleshoot any issues while running the command.
