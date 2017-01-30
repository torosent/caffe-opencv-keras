# How To Install Caffe OpenCV Keras

Tested on Azure NC6 GPU VM with Ubuntu 16.04 and Python 3.5

##General

`curl http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb > cuda-repo-ubuntu1604_8.0.44-1_amd64.deb`

`sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb`

`sudo apt-get update`

`sudo apt-get install cuda`

`sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler`

`sudo apt-get install --no-install-recommends libboost-all-dev`

`sudo apt-get install python-dev python-pip gfortran`

`sudo apt-get install libatlas-base-dev`

`sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev`

`curl https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh > Anaconda3-4.2.0-Linux-x86_64.sh`

`bash Anaconda3-4.2.0-Linux-x86_64.sh`

`wget https://bootstrap.pypa.io/ez_setup.py -O - | python - --user`

##OpenCV

`sudo apt-get autoremove libopencv-dev python-opencv`

`curl https://raw.githubusercontent.com/milq/scripts-ubuntu-debian/master/install-opencv.sh > install-opencv.sh`

`bash install-opencv.sh`

If an error occurs in graphcuts.cpp

try this: in graphcuts.cpp (where your error is thrown) change this:

`#include "precomp.hpp"`

`#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER)`
to this:

`#include "precomp.hpp"`

`#if !defined (HAVE_CUDA) || defined (CUDA_DISABLER)  || (CUDART_VERSION >= 8000)`

##Caffe

`git clone https://github.com/BVLC/caffe.git`

`cd caffe`

`cp Makefile.config.example Makefile.config`

###Makefile.config

Append /usr/include/hdf5/serial/ to INCLUDE_DIRS at line 85 in Makefile.config.

--- INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include

+++ INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/

To use OpenCV 3.X with Caffe, you should uncomment this line 21 `# OPENCV_VERSION := 3`

Uncomment to use Python 3 (default is Python 2. DON'T FORGET TO COMMENT IT) 

PYTHON_LIBRARIES := boost_python3 python3.5m 

PYTHON_INCLUDE := /usr/include/python3.5m \ 
    /usr/lib/python3.5/dist-packages/numpy/core/include 

###Makefile

Modify hdf5_hl and hdf5 to hdf5_serial_hl and hdf5_serial at line 181 in Makefile

--- LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5

+++ LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial leveldb snappy lmdb boost_system opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs

`cd python`

`for req in $(cat requirements.txt); do pip install $req; done` 

`for req in $(cat requirements.txt); do pip3 install $req; done` 

`make all`

`make test`

`make runtest`

`cd /usr/lib/x86_64-linux-gnu`

`sudo ln -s libboost_python-py35.so libboost_python3.so`

##Keras

`git clone https://github.com/fchollet/keras.git`

`cd keras`

`sudo python setup.py install`




