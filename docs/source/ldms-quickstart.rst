LDMS Quick Start
###########################
This is an example of the quickstart page. 

v4 page is under construction.

Prerequisites
***********************
* Ubuntu 16.04
* openssl-dev
* gnu compiler
* swig
* autoconf
* libtool
* libreadline6
* libreadline6-dev
* libevent
* libevent-dev
* autogen
* python-yams
* doxygen
* gettext
* python-2.7-dev
* libglib2.0-dev

Getting the Source
***********************
* This example shows cloning into ~/Source/ovis-4 and putting the build in ~/Build/OVIS-4

.. code-block:: RST
 
 mkdir $HOME/Source
 mkdir $HOME/Build
 cd $HOME/Source
 git clone https://github.com/ovis-hpc/ovis.git ovis-4
 
Building the Source
***********************
* Go to your source directory


cd $HOME/Source/ovis-4
git checkout OVIS-4

* Run autogen.sh

.. code-block:: RST
 
 ./autogen.sh

* Configure and Build (Builds default linux samplers. Build installation directory is prefix):
.. code-block:: RST
 
 mkdir build
 cd build
 ../configure --prefix=$HOME/Build/OVIS-4.x
 make
 make install

Basic Configuration and Running
***********************
* Set up environment:
