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

An image definition YAML file is required to build a custom Ubuntu Server image. This file specifies the required configurations, such as the ones listed below.

* **class**: Type of image: cloud, installer or preinstalled.
* **kernel**: By default there is just one kernel and defaults to "linux", but through this field there can be specified an alternative kernel to install in the image.
* **gadget**: Boot assets of an image.
* **customization**: Features such as particular snaps and packages that will come installed in the image.
* **artifacts**: Artifacts to create, including (but not limited to) the actual images, and manifest files.

For more details about each field, the `image definition`_ documentation can be consulted.

.. _image definition: https://github.com/canonical/ubuntu-image/blob/main/internal/imagedefinition/README.rst

The `ubuntu-images`_ repository serves as the official source for Canonical’s image definitions and scripts. It is used to build various Ubuntu versions and flavors. This includes optimized versions for Jetson Tegra hardware, currently supporting Ubuntu 22.04 (Jammy Jellyfish) and 24.04 (Noble Numbat).

.. _ubuntu-images: https://git.launchpad.net/ubuntu-images

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

            name: ubuntu-server-tegra-jetson-arm64
            display-name: Ubuntu Server Tegra Jetson arm64
            revision: 2
            architecture: arm64
            series: jammy
            class: preinstalled
            kernel: linux-nvidia-tegra-jetson
            gadget:
              url: "https://git.launchpad.net/~canonical-foundations/snap-pc/+git/github-mirror-amd64"
              branch: classic
              type: "git"
            rootfs:
              mirror: "http://ports.ubuntu.com/ubuntu-ports/"
              pocket: updates
              components:
                - main
                - restricted
                # TODO should ideally not enable universe during image build; kernel is now
                # in main, but need to MIR nvidia-tegra-defaults and list it in a seed (see
                # LP #2083775 for a proposal to add seeds)
                - universe
                - multiverse
              seed:
                urls:
                  - "git://git.launchpad.net/~ubuntu-core-dev/ubuntu-seeds/+git/"
                branch: jammy
                names:
                  - server
                  - minimal
                  - standard
                  - cloud-image
                  # TODO would like to list server-tegra here once available (see
                  # LP #2083775)
            customization:
              # We need to duplicate the list of components as we currently have some
              # packages in universe and multiverse
              components:
                - main
                - restricted
                - universe
                - multiverse
              cloud-init:
                user-data: |
                  #cloud-config
                  chpasswd:
                    expire: true
                    users:
                      - name: ubuntu
                        password: ubuntu
                        type: text
                meta-data: |
                  dsmode: local
                  instance_id: ubuntu-server
              extra-snaps:
                - name: snapd
                # FIXME ubuntu-image fails in prepare_image if this isn't listed: cannot
                # add snap "lxd" without also adding its base "core20" explicitly
                - name: core20
              extra-packages:
                # FIXME using extra-packages as a workaround for missing bits to make the
                # system bootable and has the right platform specific packages installed;
                # this could be pulled by seeds or metapackages while we design a more
                # scalable solution (see LP #2083775)
                # signed EFI boot chain
                - name: grub-efi-arm64-signed
                - name: shim-signed
                # add serial consoles to cmdline via GRUB config
                - name: nvidia-tegra-defaults
                # to get wireless support with netplan.io
                - name: wpasupplicant
                # install Tegra Orin firmware files
                - name: linux-firmware-nvidia-tegra
            artifacts:
              img:
                -
                  name: ubuntu-22.04-preinstalled-server-arm64+tegra-jetson.img
              manifest:
                name: ubuntu-22.04-preinstalled-server-arm64+tegra-jetson.manifest

   .. group-tab:: Noble

      .. code-block:: bash

            name: ubuntu-server-jetson-arm64
            display-name: Ubuntu Server Jetson arm64
            revision: 1
            architecture: arm64
            series: noble
            class: preinstalled
            kernel: linux-nvidia-tegra-jetson
            gadget:
              url: "https://git.launchpad.net/~canonical-foundations/snap-pc/+git/github-mirror-amd64"
              branch: "classic"
              type: "git"
            rootfs:
              archive: ubuntu
              components:
                - main
                - restricted
                - universe
                - multiverse
              mirror: "http://ports.ubuntu.com/ubuntu-ports/"
              sources-list-deb822: true
              pocket: updates
              seed:
                urls:
                  - "https://git.launchpad.net/~ubuntu-core-dev/ubuntu-seeds/+git/"
                branch: noble
                names:
                  - server
                  - minimal
                  - standard
                  - cloud-image
            customization:
              extra-snaps:
                - name: snapd
              extra-packages:
                # FIXME using extra-packages as a workaround for missing bits to make the
                # system bootable that should be pulled by seeds or metapackages
                # signed EFI boot chain
                - name: grub-efi-arm64-signed
                - name: shim-signed
                # for update-grub
                - name: grub2-common
                - name: nvidia-tegra-defaults
                - name: wpasupplicant
                # install Tegra firmware files
                - name: linux-firmware-nvidia-tegra
              cloud-init:
                user-data: |
                  #cloud-config
                  chpasswd:
                    expire: true
                    users:
                      - name: ubuntu
                        password: ubuntu
                        type: text
                meta-data: |
                  dsmode: local
                  instance_id: ubuntu-server
            artifacts:
              img:
                - name: ubuntu-24.04-preinstalled-server-arm64+jetson.img
              manifest:
                name: ubuntu-24.04-preinstalled-server-arm64+jetson.manifest

Build the Classic Server image
------------------------------

Once having the corresponding image definition file, the server image can be built through the following command.

.. code-block:: bash

    sudo ubuntu-image classic <image_definition>.yaml --debug

On the example above, the `--debug`  flag prints additional information about each step executed as part of the image build process, useful to troubleshoot any issues while running the command.
