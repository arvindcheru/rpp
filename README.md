[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![doc](https://img.shields.io/badge/doc-readthedocs-blueviolet)](https://gpuopen-professionalcompute-libraries.github.io/rpp/)
[![Build Status](https://travis-ci.com/GPUOpen-ProfessionalCompute-Libraries/rpp.svg?branch=master)](https://travis-ci.com/GPUOpen-ProfessionalCompute-Libraries/rpp)

# ROCm Performance Primitives Library

AMD ROCm Performance Primitives (**RPP**) library is a comprehensive high-performance computer vision library for AMD processors with `HIP`/`OpenCL`/`CPU` back-ends.

## Latest Release

[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/GPUOpen-ProfessionalCompute-Libraries/rpp?style=for-the-badge)](https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/releases)

## Top level design

<p align="center"><img width="50%" src="https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/raw/master/docs/data/rpp_structure_4.png" /></p>

## Supported Functionalities and Variants

### Supported Functionalities List

<p align="center"><img width="90%" src="https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/raw/master/docs/data/supported_functionalities.png" /></p>

### Supported Functionalities Samples

<p align="center"><img width="90%" src="https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/raw/master/docs/data/supported_functionalities_samples.jpg" /></p>

## Documentation

Run the steps below to build documentation locally.

* sphinx documentation
```bash
cd docs
pip3 install -r .sphinx/requirements.txt
python3 -m sphinx -T -E -b html -d _build/doctrees -D language=en . _build/html
```

* Doxygen 
```
doxygen .Doxyfile
```

## Prerequisites

* **OS**
  + Ubuntu `20.04`/`22.04`
  + CentOS `7`
  + RHEL `8`/`9`
  + SLES - `15-SP4`

* [ROCm supported hardware](https://docs.amd.com/bundle/Hardware_and_Software_Reference_Guide/page/Hardware_and_Software_Support.html)

* [ROCm](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.4.3/page/How_to_Install_ROCm.html) `5.4.3` and above

* Clang Version `5.0.1` and above

  + Ubuntu `20`/`22`
    ```
    sudo apt-get install clang
    ```

  + CentOS `7`
    ```
    sudo yum install llvm-toolset-7-clang llvm-toolset-7-clang-analyzer llvm-toolset-7-clang-tools-extra
    scl enable llvm-toolset-7 bash
    ```

  + RHEL `8`/`9`
    ```
    sudo yum install clang
    ```

  + SLES `15-SP4`
    ```
    zypper -n --no-gpg-checks install clang
    update-alternatives --install /usr/bin/clang clang /opt/rocm-*/llvm/bin/clang 100
    update-alternatives --install /usr/bin/clang++ clang++ /opt/rocm-*/llvm/bin/clang++ 100
    ```
    **NOTE:** Use `ROCm LLVM Clang`

* CMake Version `3.5` and above

* IEEE 754-based half-precision floating-point library - half.hpp

  ```
  wget https://sourceforge.net/projects/half/files/half/1.12.0/half-1.12.0.zip
  unzip half-1.12.0.zip -d half-files
  sudo mkdir /usr/local/include/half
  sudo cp half-files/include/half.hpp /usr/local/include/half
  ```

## Prerequisites for Test Suite

* OpenCV `3.4.0`/`4.5.5` - **pre-requisites**
  ```
  sudo apt-get update
  sudo -S apt-get -y --allow-unauthenticated install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libswscale-dev python-dev python-numpy
  sudo -S apt-get -y --allow-unauthenticated install libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev unzip wget
  ```

* OpenCV `3.4.0` /`4.5.5` - **download**
  ```
  wget https://github.com/opencv/opencv/archive/3.4.0.zip
  unzip 3.4.0.zip
  cd opencv-3.4.0/
  ```
  OR
  ```
  wget https://github.com/opencv/opencv/archive/4.5.5.zip
  unzip 4.5.5.zip
  cd opencv-4.5.5/
  ```

* OpenCV `3.4.0`/`4.5.5` - **installation**
  ```
  mkdir build
  cd build
  cmake -D WITH_GTK=ON -D WITH_JPEG=ON -D BUILD_JPEG=ON -D WITH_OPENCL=OFF -D WITH_OPENCLAMDFFT=OFF -D WITH_OPENCLAMDBLAS=OFF -D WITH_VA_INTEL=OFF -D WITH_OPENCL_SVM=OFF -D CMAKE_INSTALL_PREFIX=/usr/local ..
  sudo -S make -j128 <Or other number of threads to use>
  sudo -S make install
  sudo -S ldconfig
  ```

* TurboJpeg installation
  ```
  sudo apt-get install nasm
  sudo apt-get install wget
  git clone -b 2.0.6.1 https://github.com/rrawther/libjpeg-turbo.git
  cd libjpeg-turbo
  mkdir build
  cd build
  cmake -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=RELEASE  \
        -DENABLE_STATIC=FALSE       \
        -DCMAKE_INSTALL_DOCDIR=/usr/share/doc/libjpeg-turbo-2.0.3 \
        -DCMAKE_INSTALL_DEFAULT_LIBDIR=lib  \
        ..
  make -j$nproc
  sudo make install
  ```

## Build & Install RPP 

The ROCm Performance Primitives (RPP) library has support for three backends: HIP, OpenCL, and CPU:

* Building RPP with the **HIP** backend **(default)**:
```
$ git clone https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp.git
$ mkdir build && cd build
$ cmake -DBACKEND=HIP ../rpp
$ make -j8
$ sudo make install
```

* Building RPP with the **OPENCL** backend:
```
$ git clone https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp.git
$ mkdir build && cd build
$ cmake -DBACKEND=OCL ../rpp
$ make -j8
$ sudo make install
```

* Building RPP with the **CPU** backend:
```
$ git clone https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp.git
$ mkdir build && cd build
$ cmake -DBACKEND=CPU ../rpp
$ make -j8
$ sudo make install
```
## Test Functionalities

### CPU installation

    $ cd rpp/utilities/rpp-unittests/HOST_NEW
    $ ./testAllScript.sh

### OCL installation

    $ cd rpp/utilities/rpp-unittests/OCL_NEW
    $ ./testAllScript.sh

### HIP installation

    $ cd rpp/utilities/rpp-unittests/HIP_NEW
    $ ./testAllScript.sh

## MIVisionX Support - OpenVX Extension

[MIVisionX](https://github.com/GPUOpen-ProfessionalCompute-Libraries/MIVisionX) RPP Extension [vx_rpp](https://github.com/GPUOpen-ProfessionalCompute-Libraries/MIVisionX/tree/master/amd_openvx_extensions/amd_rpp#amd-rpp-extension) supports RPP functionality through OpenVX Framework.

## Technical Support

Please email `mivisionx.support@amd.com` for questions, and feedback on AMD RPP.

Please submit your feature requests, and bug reports on the [GitHub issues](https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/issues) page.

## Release Notes

### Latest Release

[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/GPUOpen-ProfessionalCompute-Libraries/rpp?style=for-the-badge)](https://github.com/GPUOpen-ProfessionalCompute-Libraries/rpp/releases)

### Changelog

Review all notable [changes](CHANGELOG.md#changelog) with the latest release

### Tested configurations

* Linux distribution
  + Ubuntu - `20.04` / `22.04`
  + CentOS - `7`
  + RedHat - `8` / `9`
  + SLES - `15-SP4`
* ROCm: rocm-core - `5.7.0.50700-63`
* OpenCV - [4.6.0](https://github.com/opencv/opencv/releases/tag/4.6.0)
