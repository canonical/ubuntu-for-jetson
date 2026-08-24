.. _install_classic_desktop:

==================================
Installing the desktop environment
==================================



Some use cases might require a desktop environment. To turn your Ubuntu Server image into a Desktop one, with hardware accelerated rendering, run the following commands (after installing :doc:`NVIDIA JetPack</classic/jp-installation>`):


.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo apt install -y ubuntu-desktop-minimal
         sudo sed -i 's/allowed_users.*/allowed_users=anybody/' "/etc/X11/Xwrapper.config"
         echo "needs_root_rights=yes" | sudo tee -a "/etc/X11/Xwrapper.config"
         sudo sed 's/#WaylandEnable=false/WaylandEnable=false/' -i /etc/gdm3/custom.conf
         sudo adduser gdm video
         sudo reboot

   .. group-tab:: Noble

      .. code-block:: bash

         sudo apt install -y ubuntu-desktop-minimal nvidia-l4t-gbm
         echo 'GRUB_CMDLINE_LINUX_DEFAULT="$GRUB_CMDLINE_LINUX_DEFAULT nouveau.modeset=0 modprobe.blacklist=nouveau preempt=full"' | sudo tee /etc/default/grub.d/blacklist-nouveau.cfg
         sudo update-grub
         sudo reboot

