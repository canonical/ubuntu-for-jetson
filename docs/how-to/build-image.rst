.. _build-image:

Create a custom Ubuntu Server image
===================================


Prerequisites
-------------

* `Ubuntu-image`_ must be installed on a build environment running Ubuntu 20.04 (Focal Fossa) or newer. It is recommended to use Ubuntu 24.04 (Noble Numbat).

.. _Ubuntu-image: https://github.com/canonical/ubuntu-image

.. code-block:: bash

    sudo snap install --classic --channel latest/stable ubuntu-image

Create an image definition file
-------------------------------

To build a custom Ubuntu Server image, there is needed an image definition YAML file, which specifies required configurations, like the ones listed below.

* **class**: Type of image cloud, installer, or preinstalled.
* **kernel**: By default there is just one kernel and defaults to "linux", but through this field there can be specified an alternative kernel to install in the image.
* **gadget**: Boot assets of an image.
* **customization**: Features such as particular snaps and packages that will come installed in the image.
* **artifacts**: Artifacts to create, including (but not limited to) the actual images, and manifest files.

For more details about each field the `image definition`_ documentation can be consulted.

.. _image definition: https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/README.rst

This is an `example of the image definition`_ file used to build the preinstalled images for Jetson devices.

.. _example of the image definition: https://git.launchpad.net/ubuntu-images/tree/ubuntu-server-tegra-jetson.yaml?h=jammy

Build the Classic Server image
------------------------------

Once having the corresponding image definition file, the server image can be built through the following command.

.. code-block:: bash

    sudo ubuntu-image classic <image_definition>.yaml --debug

On the example above, the `--debug`  flag prints additional information about each step executed as part of the image build process, useful to troubleshoot any issues while running the command.
