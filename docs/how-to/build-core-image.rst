.. _build-core-image:

Create a custom Ubuntu Core image
================================================


Prerequisites
-------------

* `Ubuntu-image`_ is the tool used by Canonical to build official Ubuntu images. It must be installed on a build environment running Ubuntu 22.04 (Jammy Jellyfish) or newer. It is recommended to use Ubuntu 24.04 (Noble Numbat).

.. _Ubuntu-image: https://github.com/canonical/ubuntu-image

.. code-block:: bash

    sudo snap install --classic --channel latest/stable ubuntu-image

Download official model assertion file
--------------------------------------

Official model assertions signed by Canonical can be directly fetched from the store. The available model assertions are the following:

* ubuntu-core-22-tegra-jetson
* ubuntu-core-22-tegra-jetson-candidate
* ubuntu-core-22-tegra-jetson-beta
* ubuntu-core-22-tegra-jetson-edge
* ubuntu-core-22-tegra-jetson-dangerous
* ubuntu-core-22-tegra-jetson-dangerous-candidate
* ubuntu-core-22-tegra-jetson-dangerous-beta
* ubuntu-core-22-tegra-jetson-dangerous-edge

.. note:: The model assertions that have ``-dangerous`` in their name are of ``grade: dangerous`` and can be used to make modifications to the image before installation. This should **not** be used in production!

The model assertions can be fetched and saved with the following command:

.. code-block:: bash

   snap known --remote model authority-id=canonical series=16 brand-id=canonical model=<model-name> > ubuntu-core-jetson.model

Alternatively, the model assertions can be downloaded in JSON format from the `official model repository`_. However, they need to be converted into ``.model`` files by `signing them`_ before they can be passed to the ``ubuntu-image`` command.

.. _official model repository: https://github.com/canonical/models/tree/master/devices/nvidia/jetson
.. _signing them: https://documentation.ubuntu.com/core/tutorials/build-your-first-image/sign-the-model/#ref-sign-the-model-sign-the-model

Build the image
---------------

In order to build the image, we simply pass the model file to ``ubuntu-image``.

.. code-block:: bash

   ubuntu-image snap ubuntu-core-jetson.model

The resulting image will be named ``pc.img`` and can be installed by following :doc:`documentation on how to program an ubuntu image <flash>`

Further steps
-------------

For further steps regarding customization of the image, please refer to the `official Ubuntu Core documentation`_. You can skip the step to download a model assertion.

.. _official Ubuntu Core documentation: https://documentation.ubuntu.com/core/tutorials/build-your-first-image/
