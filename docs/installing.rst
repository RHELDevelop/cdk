.. contents::
   :local:
   :depth: 2
   :backlinks: none

======================================
Installing the Atomic Developer Bundle
======================================

------------------------------------
1. Install a Virtualization Provider
------------------------------------

Two virtualization providers have been tested with the ADB; VirtualBox and Libvirt.

The following virtualization providers are suggested for the respective operating systems:

* Microsoft Windows and Mac OS X

  The suggested virtualization provider for Microsoft Windows and Mac OS X is `VirtualBox`_. Installation
  instructions are available `online`_. While the latest stable shipping release
  should work, the majority of testing has been done with version 5.0.0 on Mac
  OS X and 5.0.8 on Microsoft Windows.

.. _VirtualBox: https://www.virtualbox.org
.. _online: https://www.virtualbox.org/manual/UserManual.html

* Fedora

  Two different virtualization providers are supported on Linux; `VirtualBox`_
  and `libvirt <http://libvirt.org/>`_. The choice as to which to use should be
  driven by your preferences and environmental concerns and is outside of the
  scope of this document. Both will work equally well in their default
  configuration. You may wish to read the section on `file synchronization`_ when
  making this decision.

  * VirtualBox installation instructions are available `online at the VirtualBox
    website`_. Testing has been done with versions 4.3.34 and 5.0.0.

    A summary of the installation is listed below::

      $ sudo dnf -y install dkms
      $ curl -O http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
      $ sudo mv virtualbox.repo /etc/yum.repos.d/
      $ sudo dnf -y install VirtualBox-4.3
      $ sudo /etc/init.d/vboxdrv setup

    While the latest stable release should work, the majority of testing has
    been done with version 4.3.30.

  * Installing libvirt dependencies can be skipped as they are automatically installed together with the ``vagrant-libvirt`` package.
    Testing has been done with libvirt version 1.2.18 and vagrant-libvirt
    version 0.0.32.

.. _file synchronization: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/docs/using.rst#vagrant-bi-directional-folder-sync

* CentOS

  Two different virtualization providers are supported on Linux; `VirtualBox`_
  and `libvirt <http://libvirt.org/>`_. The choice as to which to use should be
  driven by your preferences and environmental concerns and is outside of the
  scope of this document. Both will work equally well in their default
  configuration. You may wish to read the section on `file synchronization`_ when
  making this decision.

  * VirtualBox installation instructions are available `online at the VirtualBox
    website`_. `CentOS specific instructions`_ are also available.

    While the latest stable release should work, the majority of testing has
    been done with version 4.3.30.

    A summary of the installation is listed below::

      $ sudo yum -y install epel-release
      $ sudo yum -y install dkms
      $ curl -O http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo
      $ sudo mv virtualbox.repo /etc/yum.repos.d/
      $ sudo yum -y install VirtualBox-4.3
      $ sudo /etc/init.d/vboxdrv setup

  * Installing libvirt dependencies can be skipped as they are automatically
    installed together with the CentOS software collections build.

.. _CentOS specific instructions: https://wiki.centos.org/HowTos/Virtualization/VirtualBox
.. _online at the VirtualBox website: https://www.virtualbox.org/manual/ch02.html#startingvboxonlinux
.. _VirtualBox: https://www.virtualbox.org
.. _file synchronization: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/docs/using.rst#vagrant-bi-directional-folder-sync

------------------
2. Install Vagrant
------------------

* Microsoft Windows

  1. Follow the directions at `vagrantup.com`_. Testing has been done with
     version 1.7.4.

  2. Install an ``ssh`` client. Two good options are:

     * `Cygwin <https://cygwin.com/install.html>`_
     * `mingw <http://www.mingw.org/>`_
     Please note, Putty is not recommended as it doesn't currently interface with Vagrant.

* Mac OS X

  Follow the directions at `vagrantup.com`_. Testing has been done with version
  1.7.4.

.. _vagrantup.com: https://docs.vagrantup.com/v2/installation/index.html

* Fedora 22/23/Rawhide(24)

  Testing has been done with version 1.7.2.

  * To install Vagrant with VirtualBox in Fedora 22/23/24 (Rawhide)::

    $ sudo dnf install -y vagrant

  * To install Vagrant with libvirt in Fedora 22/23/24 (Rawhide)::

      $ sudo dnf -y install vagrant-libvirt

      # Start libvirtd
      $ sudo systemctl start libvirtd

      # Set libvirtd to start automatically on system boot
      $ sudo systemctl enable libvirtd

    This would install both Vagrant and Libvirt.

* CentOS

  Vagrant packages are not available directly in CentOS core. However, they are
  available through official CentOS `Software Collections
  <http://softwarecollections.org>`_ builds.

  Here are the commands to get Vagrant in CentOS::

    $ sudo yum -y install centos-release-scl
    $ sudo yum -y install sclo-vagrant1
    $ sudo scl enable sclo-vagrant1 bash

  To add `libvirt` support use this::

    # Start libvirtd
    $ sudo systemctl start libvirtd

    # Set libvirtd to start automatically on system boot
    $ sudo systemctl enable libvirtd

----------------------------------
3. Install additional dependencies
----------------------------------

For some operating systems, you might need to install additional dependencies before you install the Vagrant plugins.

* Fedora 22/23/24

  Run the following commands to install the additional dependencies::

  $ sudo dnf install @'Development Tools'
  $ sudo dnf install rpm-build zlib-devel ruby-devel gcc-c++

--------------------------
4. Install Vagrant plugins
--------------------------

Run the following commands to install the `vagrant-service-manager`_, `vagrant-sshfs`_, and `landrush`_ plugins::

$ vagrant plugin install vagrant-service-manager
$ vagrant plugin install vagrant-sshfs
$ vagrant plugin install landrush

.. _vagrant-service-manager: https://github.com/projectatomic/vagrant-service-manager
.. _vagrant-sshfs: https://github.com/dustymabe/vagrant-sshfs
.. _landrush: https://github.com/vagrant-landrush/landrush

-------------------
4. Download the ADB
-------------------

There are two ways to download the ADB.

* Vagrantfiles Initiated Download

  The ADB project provides customized Vagrantfiles, which will download the ADB and automatically set up provider specific container development environments. They are listed below and more details are available, in the `Using Custom Vagrantfiles for Specific Use Cases`_ section of the `Using the Atomic Developer Bundle`_ document and the `Installation document`_.

  To download ADB and set up a provider specific container development environment:


  1. Create a directory for the Vagrant box

     ``$ mkdir directory && cd directory``

  2. Download any of the following vagrantfiles, based on your requirements, to download the ADB and use it with host-based tools or via ``vagrant ssh``.

     * For `Docker Vagrantfile`_ use::

        $ curl -sL https://raw.githubusercontent.com/projectatomic/adb-atomic-developer-bundle/master/components/centos/centos-docker-base-setup/Vagrantfile > Vagrantfile


     * For `Kubernetes Vagrantfile`_ use::

        $ curl -sL https://raw.githubusercontent.com/projectatomic/adb-atomic-developer-bundle/master/components/centos/centos-k8s-singlenode-setup/Vagrantfile > Vagrantfile

     * For `OpenShift Origin Vagrantfile`_ use::

        $ curl -sL https://raw.githubusercontent.com/projectatomic/adb-atomic-developer-bundle/master/components/centos/centos-openshift-setup/Vagrantfile > Vagrantfile


     * For `Apache Mesos Marathon Vagrantfile`_ use::

        $curl -sL https://raw.githubusercontent.com/projectatomic/adb-atomic-developer-bundle/master/components/centos/centos-mesos-marathon-singlenode-setup/Vagrantfile > Vagrantfile

  3. Start the ADB

     ``vagrant up``

  This will download the ADB and set it up to work with Docker, for use with host-based tools or via ``vagrant ssh``.

  You may wish to review the `Using the Atomic Developer Bundle`_ documentation before
  starting the ADB, especially if you are using host-based tools.

.. _Using Custom Vagrantfiles for Specific Use Cases: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/docs/using.rst#using-custom-vagrantfiles-for-specific-use-cases
.. _Using the Atomic Developer Bundle: using.rst
.. _Installation document: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/docs/installing.rst
.. _Docker Vagrantfile: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/components/centos/centos-docker-base-setup/Vagrantfile
.. _Kubernetes Vagrantfile: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/components/centos/centos-k8s-singlenode-setup/Vagrantfile
.. _OpenShift Origin Vagrantfile: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/components/centos/centos-openshift-setup/Vagrantfile
.. _Apache Mesos Marathon Vagrantfile: https://github.com/projectatomic/adb-atomic-developer-bundle/blob/master/components/centos/centos-mesos-marathon-singlenode-setup/Vagrantfile

* Manually Downloading the Vagrant Box Image

  Alternatively, you can manually download the vagrant box from
  `cloud.centos.org <http://cloud.centos.org/centos/7/atomic/images/>`_ using
  your web browser or curl. For example::

    # To get the libvirt image
    $ wget http://cloud.centos.org/centos/7/atomic/images/AtomicDeveloperBundle-<latest>.box

    # To get the virtual box image
    $ wget http://cloud.centos.org/centos/7/atomic/images/AtomicDeveloperBundle-<latest>.box

  Once you have downloaded the image, you can add it to ``vagrant`` with this
  command::

    # Add the image to vagrant
    $ vagrant box add adb <local path to the downloded image>

At this point your Atomic Developer Bundle installation is complete. You can
find `ADB Usage Information <using.rst>`_ in the documentation directory.
