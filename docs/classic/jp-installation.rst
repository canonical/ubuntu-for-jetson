.. _install_classic:


===============================
NVIDIA JetPack on Ubuntu Server
===============================


Install NVIDIA proprietary software
===================================

The Ubuntu image brings everything necessary to boot Linux on a Jetson developer kit. However, to unlock the features of the Tegra SoC (wireless network, Bluetooth, GPU, …) you can install additional NVIDIA proprietary drivers and libraries using an additional repository :


.. _classic-nvidia-proprietary-software:
.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo add-apt-repository ppa:ubuntu-tegra/updates
         # Install Tegra firmwares and necessary NVIDIA libraries
         sudo apt install -y nvidia-tegra-drivers-36
         # Adding user to group render allows running GPU related commands as non root
         # video group is necessary to use the camera
         sudo usermod -a -G render,video ubuntu
         sudo reboot

   .. group-tab:: Noble

      .. code-block:: bash

         # The following assumes the boot firmware 39.2 is present on the system:
         # https://developer.nvidia.com/downloads/embedded/L4T/r39_Release_v2.0/release/Jetson_Linux_R39.2.0_aarch64.tbz2
         sudo apt-key adv --fetch-keys "https://repo.download.nvidia.com/jetson/jetson-ota-public.asc"
         sudo add-apt-repository -y "deb https://repo.download.nvidia.com/jetson/common r39.2 main"
         sudo add-apt-repository -y "deb https://repo.download.nvidia.com/jetson/som r39.2 main"
         # Install Tegra firmwares and necessary NVIDIA libraries
         sudo apt install -y nvidia-l4t-{core,nvml,init,firmware*,nvpmodel,tools}
         # Adding user to group render allows running GPU related commands as non root
         # video group is necessary to use the camera
         sudo usermod -a -G render,video ubuntu
         sudo reboot

Upgrade the system (optional)
=============================

Upgrade the system to install the kernel updates

.. code-block:: bash

    sudo apt update; sudo apt upgrade

Install CUDA and TensorRT
=========================

SDKs like CUDA Toolkit and TensorRT that allow building AI applications on Jetson devices are available directly from NVIDIA:


.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         # CUDA
         sudo apt-key adv --fetch-keys "https://repo.download.nvidia.com/jetson/jetson-ota-public.asc"
         sudo add-apt-repository -y "deb https://repo.download.nvidia.com/jetson/t234 r36.5 main"
         sudo add-apt-repository -y "deb https://repo.download.nvidia.com/jetson/common r36.5 main"
         sudo apt install -y cuda

         # cuda-samples dependencies
         sudo apt install -y cmake

         echo "export PATH=/usr/local/cuda-12.6/bin\${PATH:+:\${PATH}}" >> ~/.profile
         echo "export LD_LIBRARY_PATH=/usr/local/cuda-12.6/lib64\${LD_LIBRARY_PATH:+:\${LD_LIBRARY_PATH}}" >> ~/.profile

         # TensorRT
         sudo apt install -y libnvinfer-bin libnvinfer-samples

         # Logout or reboot to apply the profile change
         sudo reboot


   .. group-tab:: Noble

      .. code-block:: bash

         # CUDA
         sudo wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/sbsa/cuda-keyring_1.1-1_all.deb
         sudo dpkg -i cuda-keyring_1.1-1_all.deb
         sudo apt update
         sudo apt install -y nvidia-l4t-cuda* cuda-toolkit-13-2

         # cuda-samples dependencies
         sudo apt install -y cmake
         echo "export PATH=/usr/local/cuda-13.2/bin\${PATH:+:\${PATH}}" >> ~/.profile
         echo "export LD_LIBRARY_PATH=/usr/local/cuda-13.2/lib64\${LD_LIBRARY_PATH:+:\${LD_LIBRARY_PATH}}" >> ~/.profile

         # TensorRT
         TENSORRT_VERSION="10.16.2.10-1+cuda13.2"
         TENSORRT_PKGS=(
            tensorrt=$TENSORRT_VERSION
            libnvinfer{10,-lean10,-plugin10,-vc-plugin10,-dispatch10,-bin,-dev,-lean-dev,-plugin-dev,-vc-plugin-dev,-dispatch-dev,-headers-python-plugin-dev,-headers-dev,-safe-headers-dev,-headers-plugin-dev}=$TENSORRT_VERSION
            libnvonnxparsers{10,-dev}=$TENSORRT_VERSION
            python3-libnvinfer{,-dev,-lean,-dispatch}=$TENSORRT_VERSION
         )
         sudo DEBIAN_FRONTEND=noninteractive apt install -y "${TENSORRT_PKGS[@]}"

         # Logout or reboot to apply the profile change
         sudo reboot



Test your system
================


NVIDIA system management interface
----------------------------------

``nvidia-smi`` can be used to display GPU related information.

.. tabs::

   .. group-tab:: Jammy

        .. image:: nvidia-smi.png
           :alt: Screenshot of the ``nvidia-smi`` tool

   .. group-tab:: Noble

        .. image:: nvidia-smi-thor.png
           :alt: Screenshot of the ``nvidia-smi`` tool


Run GPU's sample code application
---------------------------------

CUDA samples
^^^^^^^^^^^^

You can build and run `CUDA sample`_ applications. You can start with ``deviceQuery``, but you can also build and try many others.

.. tabs::

   .. group-tab:: Jammy

       .. code-block:: bash

         git clone https://github.com/NVIDIA/cuda-samples.git -b v12.5
         cd cuda-samples
         cd Samples/1_Utilities/deviceQuery && make

   .. group-tab:: Noble

       .. code-block:: bash

         git clone https://github.com/NVIDIA/cuda-samples.git -b v13.2
         cd cuda-samples
         cd Samples/1_Utilities/deviceQuery && cmake . && make

Running this sample code should produce the following output

.. tabs::

   .. group-tab:: Jammy

      .. code-block::

         ubuntu@ubuntu:~/cuda-samples/Samples/1_Utilities/deviceQuery$ ./deviceQuery
         ./deviceQuery Starting...

         CUDA Device Query (Runtime API) version (CUDART static linking)

         Detected 1 CUDA Capable device(s)

         Device 0: "Orin"
         CUDA Driver Version / Runtime Version      	12.6 / 12.6
         CUDA Capability Major/Minor version number:	8.7
         Total amount of global memory:             	7618 MBytes (7987728384 bytes)
         (004) Multiprocessors, (128) CUDA Cores/MP:	512 CUDA Cores
         GPU Max Clock rate:                        	765 MHz (0.76 GHz)
         Memory Clock rate:                         	612 Mhz
         Memory Bus Width:                          	128-bit
         L2 Cache Size:                             	2097152 bytes
         Maximum Texture Dimension Size (x,y,z)     	1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
         Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
         Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
         Total amount of constant memory:           	65536 bytes
         Total amount of shared memory per block:   	49152 bytes
         Total shared memory per multiprocessor:    	167936 bytes
         Total number of registers available per block: 65536
         Warp size:                                 	32
         Maximum number of threads per multiprocessor:  1536
         Maximum number of threads per block:       	1024
         Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
         Max dimension size of a grid size	(x,y,z): (2147483647, 65535, 65535)
         Maximum memory pitch:                      	2147483647 bytes
         Texture alignment:                         	512 bytes
         Concurrent copy and kernel execution:      	Yes with 2 copy engine(s)
         Run time limit on kernels:                 	No
         Integrated GPU sharing Host Memory:        	Yes
         Support host page-locked memory mapping:   	Yes
         Alignment requirement for Surfaces:        	Yes
         Device has ECC support:                    	Disabled
         Device supports Unified Addressing (UVA):  	Yes
         Device supports Managed Memory:            	Yes
         Device supports Compute Preemption:        	Yes
         Supports Cooperative Kernel Launch:        	Yes
         Supports MultiDevice Co-op Kernel Launch:  	Yes
         Device PCI Domain ID / Bus ID / location ID:   0 / 0 / 0
         Compute Mode:
            < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

         deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 12.6, CUDA Runtime Version = 12.6, NumDevs = 1
         Result = PASS

   .. group-tab:: Noble

      .. code-block::

         ubuntu@ubuntu:~/cuda-samples/Samples/1_Utilities/deviceQuery$ ./deviceQuery 
         ./deviceQuery Starting...

         CUDA Device Query (Runtime API) version (CUDART static linking)

         Detected 1 CUDA Capable device(s)

         Device 0: "NVIDIA Thor"
         CUDA Driver Version / Runtime Version          13.2 / 13.2
         CUDA Capability Major/Minor version number:    11.0
         Total amount of global memory:                 125492 MBytes (131587801088 bytes)
         (020) Multiprocessors, (128) CUDA Cores/MP:    2560 CUDA Cores
         GPU Max Clock rate:                            1049 MHz (1.05 GHz)
         Memory Clock rate:                             4266 Mhz
         Memory Bus Width:                              256-bit
         L2 Cache Size:                                 33554432 bytes
         Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
         Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
         Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
         Total amount of constant memory:               65536 bytes
         Total amount of shared memory per block:       49152 bytes
         Total shared memory per multiprocessor:        233472 bytes
         Total number of registers available per block: 65536
         Warp size:                                     32
         Maximum number of threads per multiprocessor:  1536
         Maximum number of threads per block:           1024
         Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
         Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
         Maximum memory pitch:                          2147483647 bytes
         Texture alignment:                             512 bytes
         Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
         Run time limit on kernels:                     No
         Integrated GPU sharing Host Memory:            Yes
         Support host page-locked memory mapping:       Yes
         Alignment requirement for Surfaces:            Yes
         Device has ECC support:                        Disabled
         Device supports Unified Addressing (UVA):      Yes
         Device supports Managed Memory:                Yes
         Device supports Compute Preemption:            Yes
         Supports Cooperative Kernel Launch:            Yes
         Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
         Compute Mode:
            < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

         deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 13.2, CUDA Runtime Version = 13.2, NumDevs = 1
         Result = PASS


.. _CUDA sample: https://github.com/NVIDIA/cuda-samples/tree/master

TensorRT
^^^^^^^^

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         mkdir ${HOME}/tensorrt-samples
         ln -s /usr/src/tensorrt/data ${HOME}/tensorrt-samples/data
         cp -a /usr/src/tensorrt/samples ${HOME}/tensorrt-samples/
         cd ${HOME}/tensorrt-samples/samples/sampleAlgorithmSelector && make
         cd ${HOME}/tensorrt-samples/bin
         ./sample_algorithm_selector

   .. group-tab:: Noble

      .. code-block:: bash

         TENSORRT_SAMPLES_TAG=release/10.16
         sudo DEBIAN_FRONTEND=noninteractive apt install -y python3-virtualenv python3-pip unzip
         virtualenv .venv && source .venv/bin/activate
         pip install --upgrade cmake
         # Re-scan the system with the new Cmake
         hash -r && cmake --version
         git clone -b $TENSORRT_SAMPLES_TAG https://github.com/NVIDIA/TensorRT.git ${HOME}/TensorRT
         cd ${HOME}/TensorRT
         mkdir build
         cd build
         cmake .. \
            -DTRT_LIB_DIR=/usr/lib/aarch64-linux-gnu \
            -DTRT_OUT_DIR=`pwd`/bin \
            -DBUILD_SAMPLES=ON \
            -DBUILD_PARSERS=OFF \
            -DBUILD_PLUGINS=OFF \
            -DTRT_PLATFORM_ID=aarch64
         cmake --build . --parallel $(nproc) &> ~/tensorrt-samples-buildlog.txt
         sudo cp -r samples/ /usr/src/tensorrt/samples/
         sudo cp -r bin/ /usr/src/tensorrt
         deactivate
         wget -q https://github.com/NVIDIA/TensorRT/releases/download/v11.0/tensorrt_sample_data_20260602.zip
         unzip tensorrt_sample_data_*.zip -d data
         rm -rf tensorrt_sample_data_*.zip
         ./bin/sample_onnx_mnist


Camera
^^^^^^
The following camera tests were conducted on the following Jetson developer kits:

* Jetson AGX Orin connected to a LI Dual IMX274 camera module
* Jetson Orin Nano connected to an IMX219 camera module
* Jetson Orin NX connected to an IMX219 camera module
* Jetson AGX Thor with a developer carrier board connected to a LI Dual IMX274 camera module.

Prerequisites
"""""""""""""

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         # Allow camera stack to use the right libraries
         sudo update-alternatives \
               --install /etc/ld.so.conf.d/aarch64-linux-gnu_EGL.conf \
               aarch64-linux-gnu_egl_conf \
               /usr/lib/aarch64-linux-gnu/tegra-egl/ld.so.conf 1000
         sudo update-alternatives \
               --install /etc/ld.so.conf.d/aarch64-linux-gnu_GL.conf \
               aarch64-linux-gnu_gl_conf \
               /usr/lib/aarch64-linux-gnu/nvidia/ld.so.conf 1000
         sudo ldconfig
         sudo reboot

   .. group-tab:: Noble

      .. code-block:: bash

         sudo apt install -y nvidia-l4t-{gstreamer,3d-core,gbm,multimedia-*,video-codec-openrm}
         sudo reboot

Verify the camera is detected
"""""""""""""""""""""""""""""

.. tabs::

   .. group-tab:: Jammy

     Please also refer to the `r36.5 NVIDIA test plan camera setup <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#camera>`_

   .. group-tab:: Noble

     Please also refer to the `r39.2 NVIDIA test plan camera setup <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#camera>`_

.. code-block:: bash

      # Install v4l2-ctl
      sudo apt install v4l-utils
      v4l2-ctl --list-devices
      v4l2-ctl --list-formats-ext

If your device is properly detected, the output should be close to this one:

.. code-block::

      ubuntu@ubuntu:~$ v4l2-ctl --list-devices
      NVIDIA Tegra Video Input Device (platform:tegra-camrtc-ca):
            /dev/media0

      vi-output, imx219 10-0010 (platform:tegra-capture-vi:1):
            /dev/video0


You should then be able to `detect it via the NVARGUS daemon <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#verifying-imx274-camera-sensor>`_ (in this example, the ``sensor-id`` is ``0``):

.. code-block::

      ubuntu@ubuntu:~$ nvargus_nvraw --sensorinfo --c 0
      nvargus_nvraw version 1.15.0
      Number of sensors 1, Number of modes for selected sensor 5
      Selected sensor: jakku_front_RBP194 ID 0 Mode 0
      Number of exposures 1
      Index   Exposure time Range      	Sensor Gain Range
      0   	0.000013 - 0.500000      	1.000000 - 10.625000
      Warning: Maximum value of Exposure time 0.683709 secs is more than maximum Frame duration of 0.5 secs.
      Changing
         Maximum Exposure time to 0.5 secs.

Capture a JPEG image with NVARGUS
"""""""""""""""""""""""""""""""""

Still with the same ``sensor-id``

.. code-block:: bash

      # Unset DISPLAY only if running the commands from SSH or a serial console
      unset DISPLAY

      nvargus_nvraw --c 0 --format jpg --file ${HOME}/frame-cam0.jpg


GStreamer
^^^^^^^^^

Prerequisites for GStreamer
"""""""""""""""""""""""""""

Make sure to install the necessary GStreamer packages


.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         # Install GStreamer plugins
         sudo apt install -y gstreamer1.0-tools gstreamer1.0-alsa \
            gstreamer1.0-plugins-base gstreamer1.0-plugins-good \
            gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly \
            gstreamer1.0-libav
         sudo apt install -y libgstreamer1.0-dev \
            libgstreamer-plugins-base1.0-dev \
            libgstreamer-plugins-good1.0-dev \
            libgstreamer-plugins-bad1.0-dev


   .. group-tab:: Noble

      .. code-block:: bash

         # Install necessary NVIDIA libraries (if it wasn't already done in the previous steps)
         sudo apt install -y nvidia-l4t-{gstreamer,3d-core,gbm,multimedia-*,video-codec-openrm}

         # Install GStreamer plugins and NVIDIA codecs
         sudo apt install -y gstreamer1.0-tools gstreamer1.0-alsa \
            gstreamer1.0-plugins-base gstreamer1.0-plugins-good \
            gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly \
            gstreamer1.0-libav
         sudo apt install -y libgstreamer1.0-dev \
            libgstreamer-plugins-base1.0-dev \
            libgstreamer-plugins-good1.0-dev \
            libgstreamer-plugins-bad1.0-dev


Camera capture using GStreamer
""""""""""""""""""""""""""""""

.. tabs::

   .. group-tab:: Jammy

     You can find more information in the `NVIDIA test plan R36.5 regarding camera capture <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#camera-capture-using-gstreamer>`_

   .. group-tab:: Noble

     You can find more information in the `NVIDIA test plan R39.2 regarding camera capture <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#transcode-using-gstreamer>`_

.. code-block:: bash

    # Unset DISPLAY only if running the commands from SSH or a serial console
    unset DISPLAY

    # Capture an image
    gst-launch-1.0 nvarguscamerasrc num-buffers=1 sensor-id=0 ! \
        'video/x-raw(memory:NVMM), width=(int)1920, height=(int)1080,' \ 'format=(string)NV12' ! nvjpegenc ! filesink \
        location=${HOME}/gst-frame-cam0.jpg

    # Capturing Video from the Camera and Record
    gst-launch-1.0 nvarguscamerasrc num-buffers=300 sensor-id=0 ! \
        'video/x-raw(memory:NVMM), width=(int)1920, height=(int)1080,' \
        'format=(string)NV12, framerate=(fraction)30/1' ! \
        nvv4l2h265enc bitrate=8000000 ! h265parse ! qtmux ! \
        filesink location=test.mp4

Transcode using GStreamer
"""""""""""""""""""""""""

.. tabs::

   .. group-tab:: Jammy

     You can find more information in the `NVIDIA test plan R36.5 regarding gstreamer transcoding <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#transcode-using-gstreamer>`_

   .. group-tab:: Noble

     You can find more information in the `NVIDIA test plan R39.2 regarding gstreamer transcoding <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#transcode-using-gstreamer>`_


Using a stream from the `Big Buck Bunny project <https://peach.blender.org/>`_, you can easily test the transcoding pipelines (note that Jetson Orin Nano doesn’t have hardware encoders and won’t be able to run these pipelines):

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo apt install unzip
         wget -nv https://download.blender.org/demo/movies/BBB/bbb_sunflower_1080p_30fps_normal.mp4.zip
         unzip -qu bbb_sunflower_1080p_30fps_normal.mp4.zip
         echo "H.264 Decode (NVIDIA Accelerated Decode) to H265 encode"
         gst-launch-1.0 filesrc location=bbb_sunflower_1080p_30fps_normal.mp4 ! qtdemux ! queue ! \
             h264parse ! nvv4l2decoder ! nvv4l2h265enc bitrate=8000000 ! h265parse ! \
             qtmux ! filesink location=h265-reenc.mp4 -e
         echo "H.265 Decode (NVIDIA Accelerated Decode) to AV1 Encode (NVIDIA Accelerated Encode)"
         gst-launch-1.0 filesrc location=h265-reenc.mp4 ! qtdemux ! queue ! h265parse ! nvv4l2decoder ! \
             nvv4l2av1enc ! matroskamux name=mux ! filesink location=av1-reenc.mkv -e
         echo "AV1 Decode (NVIDIA Accelerated Decode) to H.264 encode"
         gst-launch-1.0 filesrc location=av1-reenc.mkv ! matroskademux ! queue ! av1parse ! nvv4l2decoder ! \
             nvv4l2h264enc bitrate=20000000 ! h264parse ! queue ! qtmux name=mux ! filesink \
             location=h264-reenc.mp4 -e
         echo "H.264 Decode (NVIDIA Accelerated Decode) to AV1"
         gst-launch-1.0 filesrc location=h264-reenc.mp4 ! qtdemux ! \
             h264parse ! nvv4l2decoder ! nvv4l2av1enc ! matroskamux name=mux ! \
             filesink location=av1-reenc.mkv -e

   .. group-tab:: Noble

      .. code-block:: bash

         sudo apt install unzip
         wget -nv https://download.blender.org/demo/movies/BBB/bbb_sunflower_1080p_30fps_normal.mp4.zip
         unzip -qu bbb_sunflower_1080p_30fps_normal.mp4.zip
         echo "H.264 Decode (NVIDIA Accelerated Decode) to H265 encode"
         gst-launch-1.0 filesrc location=bbb_sunflower_1080p_30fps_normal.mp4 ! qtdemux ! queue ! \
             h264parse ! nvv4l2decoder ! nvv4l2h265enc bitrate=8000000 ! h265parse ! \
             qtmux ! filesink location=h265-reenc.mp4 -e
         echo "H.265 Decode (NVIDIA Accelerated Decode) to H.264 encode"
         gst-launch-1.0 filesrc location=h265-reenc.mp4 ! qtdemux ! queue ! h265parse ! nvv4l2decoder ! \
             nvv4l2h264enc bitrate=20000000 ! h264parse ! queue ! qtmux name=mux ! \
             filesink location=h264-reenc.mp4 -e




cuDNN
^^^^^

Prerequisite
""""""""""""

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo apt install cudnn libcudnn9-samples

   .. group-tab:: Noble

      .. code-block:: bash

         sudo apt install cudnn libcudnn9-samples libfreeimage-dev


Run cuDNN Samples
"""""""""""""""""

.. tabs::

   .. group-tab:: Jammy

     You can find more information in the `NVIDIA test plan R36.5 regarding cudnn samples <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#run-cudnn-samples>`_

   .. group-tab:: Noble

     You can find more information in the `NVIDIA test plan R39.2 regarding cudnn samples <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#run-cudnn-samples>`_


Build and run the Converted sample.

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         cd /usr/src/cudnn_samples_v9
         cd conv_sample
         sudo make -j8

         sudo chmod +x run_conv_sample.sh
         sudo ./run_conv_sample.sh
   .. group-tab:: Noble

      .. code-block:: bash

         cd /usr/src/cudnn_samples_v9/
         sudo cmake \
               -DCMAKE_CUDA_COMPILER=/usr/local/cuda-13.2/bin/nvcc \
               -DCMAKE_CUDA_ARCHITECTURES=native \
               -DcuDNN_LIBRARY_DIR=/usr/lib/aarch64-linux-gnu/ .
         sudo cmake --build . -j$(nproc)
         cd conv_sample
         sudo chmod +x run_conv_sample.sh
         ./run_conv_sample.sh

You can also try other sample applications.


NVIDIA container runtime
^^^^^^^^^^^^^^^^^^^^^^^^

.. tabs::

   .. group-tab:: Jammy

      You can follow the `r36.5 NVIDIA container test plan <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#nvidia-containers>`_ to install and configure the `NVIDIA Container Toolkit <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-the-nvidia-container-toolkit>`_ before running the JetPack container.
      Try to run a previously built CUDA sample application:

   .. group-tab:: Noble

      You can follow the `r39.2 NVIDIA container test plan <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#nvidia-containers>`_ to install and configure the `NVIDIA Container Toolkit <https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-the-nvidia-container-toolkit>`_ before running the JetPack container.
      Try to run a previously built CUDA sample application:

.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo docker run --rm -it -e DISPLAY --net=host --runtime \
             nvidia -v /tmp/.X11-unix/:/tmp/.X11-unix  -v \
             ${HOME}/cuda-samples:/root/cuda-samples \
             nvcr.io/nvidia/l4t-jetpack:r36.3.0 \
             /root/cuda-samples/Samples/1_Utilities/deviceQuery/deviceQuery

   .. group-tab:: Noble

      .. code-block:: bash

         sudo docker run --rm --net=host --runtime nvidia -e DISPLAY=$DISPLAY \
            -v /tmp/.X11-unix/:/tmp/.X11-unix -v \
            ${HOME}/cuda-samples:/root/cuda-samples \
            nvcr.io/nvidia/cuda:13.2.0-runtime-ubuntu24.04 \
            /root/cuda-samples/Samples/1_Utilities/deviceQuery/deviceQuery


VPI
^^^

Prerequisites for VPI
"""""""""""""""""""""

Install VPI and its sample applications


.. tabs::

   .. group-tab:: Jammy

      .. code-block:: bash

         sudo apt install nvidia-vpi vpi3-samples libopencv cmake libpython3-dev python3-numpy libopencv-python

   .. group-tab:: Noble

      .. code-block:: bash

         sudo apt install nvidia-l4t-pva nvidia-vpi vpi4-samples libopencv cmake libpython3-dev python3-numpy libopencv-python python3-pil

Test
""""

.. tabs::

   .. group-tab:: Jammy

     Execute steps 1 to 6 from the `r36.5 NVIDIA VPI test plan <https://docs.nvidia.com/jetson/archives/r36.5/DeveloperGuide/SD/TestPlanValidation.html#vpi>`_, for each VPI sample application.

   .. group-tab:: Noble

     Execute steps 1 to 6 from the `r39.2 NVIDIA VPI test plan <https://docs.nvidia.com/jetson/archives/r39.2/DeveloperGuide/SD/TestPlanValidation.html#vpi>`_, for each VPI sample application.


