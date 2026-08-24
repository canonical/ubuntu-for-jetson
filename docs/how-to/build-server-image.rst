.. _build-server-image:

Ubuntu Server preinstalled image
================================================


Create an image definition file
-------------------------------

An image definition file, in YAML format, defines the various configurations required to build a custom Ubuntu image:

* **class**: Defines the type of image, such as `cloud`, `installer` or `preinstalled` (Canonical certified Server images are preinstalled).
* **kernel**: Specifies the preinstalled kernel in the image. The official Ubuntu kernel for Jetson is "linux-nvidia-tegra-jetson", but it can be replaced with an alternative custom kernel, for instance hosted in a PPA for development purposes.
* **gadget**: Boot assets of an image.
* **customization**: Features such as particular snaps and packages that will come installed in the image.
* **artifacts**: Artifacts to create, including (but not limited to) the actual images, and manifest files.

For more details about each field, refer to the `image definition`_ documentation.

.. _image definition: https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/README.rst

Ubuntu certified Image definition files
---------------------------------------

The ubuntu-images repository serves as the official source for Canonical image definitions and scripts. It is used to build various Ubuntu versions and flavors. This includes optimized versions for Jetson Tegra hardware, currently supporting Ubuntu 22.04 (Jammy Jellyfish) for Jetson Orin platforms and 24.04 (Noble Numbat) for Jetson Thor devices.

The following commands allow you to check the definition files used to generate the Ubuntu Server image certified by Canonical:

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         git clone -b jammy https://git.launchpad.net/ubuntu-images
         cd ubuntu-images
         cat ubuntu-server-tegra-jetson.yaml

   .. group-tab:: Noble

      .. code-block:: bash

         git clone -b noble https://git.launchpad.net/ubuntu-images
         cd ubuntu-images
         cat ubuntu-server-tegra-jetson.yaml

Build the Classic Server image
------------------------------

Once the corresponding image definition file is created, the server image can be built through the following command.

.. code-block:: bash

    sudo ubuntu-image classic <image_definition>.yaml --debug

In the example above, the `--debug` flag prints additional information about each step executed as part of the image build process, coming in handy to troubleshoot any issues.
