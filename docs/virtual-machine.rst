===============
Virtual Machine
===============

.. .. contents:: :depth: 2

-----
About
-----

.. sidebar:: Version Available 1.2
    :subtitle: OAR runs on

    - SO Ubuntu 14.04.3 LTS
    - Python 2.7.6
    - Mysql Ver 14.14 Distrib 5.5.46
    - Invenio 1.2.1


The virtual appliance contains a clone of Sci-GaIA Open Access Repositories `Sci-GaIA OAR <http://oar.sci-gaia.eu/>`_, on which NADRE Repository is based so, if you'd like to install your own open access repository based on standard technologies, you can simply download this clone and deploy it on your virtualization environment.

--------------
Deploying OAR
--------------

To deploy your own open access repository, you can download the image from `here <http://oar.sci-gaia.eu/record/19/files/oar-scigaia-template.qcow2>`_, the file size is about 10GB. In this way you download the Sci-GaIA Open Access Repository template that can be deployed on your virtualization environment. The image is in QCOW format, but can be easily converted in other format as you need, using **qemu** utils.

This guide shows you two examples of how to use virtual appliance template in a Openstack based cloud infrastructure and in a local Virtualbox environment.

^^^^^^^^^^^^
First Access
^^^^^^^^^^^^

Before you can do the first access to your newly OAR installation, please `contact us <mailto:nadre-support@ethernet.edu.et>`_ to get the default OAR template credentials. This template allows login only with keys and don't permit SSH root login, for security reasons.
Once you get default credentials, login into the OAR installation from the virtualization environment console and perform the the following steps.

.. warning:: If you don't do this you will get hacked.

1. Add your ssh public keys to the invenio user

.. note:: You can use your preferred way to do this stuff. For example, if you maintain your public keys with the github service you can do the following:

    - wget https://github.com/<github_username>.keys
    - mv <github_username>.keys .ssh/authorized_keys

2. Test remote login:

.. code::

    ssh invenio@<oar_ip_address>
    Welcome to Ubuntu 14.04.3 LTS (GNU/Linux 3.13.0-62-generic x86_64)

     * Documentation:  https://help.ubuntu.com/

     System information disabled due to load higher than 1.0

     Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud

3. Setup firewall according your security requirements, the default rules applied to the the template are the following:

.. code::

    sudo iptables -L -n
    Chain INPUT (policy DROP)
    target     prot opt source               destination
    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    DROP       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp flags:0x3F/0x00
    DROP       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp flags:!0x17/0x02 s...
    DROP       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp flags:0x3F/0x3F
    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:22
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80
    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:443
    REJECT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp flags:0x16/0x02 re...
    REJECT     all  --  0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-...

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination

-------------------
Deployment Examples
-------------------

^^^^^^^^^^^^^^^^^^^^
Openstack deployment
^^^^^^^^^^^^^^^^^^^^

This section shows how to the deploy the OAR image template on an `Openstack <https://www.openstack.org/>`_  cloud based infrastructure.

.. note:: The steps below describe the process using the **Openstack Dashboard**, if you cannot access Openstack Dashboard, you can issue the equivalent Command Line Interface commands.

1. Create a new image in the image service, clicking the *Images* link in the left side menu and then click *Create Image* button
2. Fill all fields with your desidered values (see Figure `1`_ as example) and then click *Save* button.

.. _1:

.. figure:: figures/create-image.png
   :align: center
   :alt: New Image
   :scale: 70%
   :figclass: text

   Create new image.

.. note:: Pay attention to *Minimun disk* value: the OAR template require at least 20GB.

3. Once the image becomes ready, create a new instance:
    a. Click *Instances* link in the left side menu.
    b. Click *Launch Instance* button.
4. Fill all fields with your desidered values for all tabs (see Figure `2`_ as example) and then click *Save* button.

.. _2:

.. figure:: figures/instance.png
   :align: center
   :alt: New Instance
   :scale: 75%
   :figclass: text

   Create new instance.

5. Wait until the new instaces *Power State* becomes *Running*.
6. Open the instance console, and follow the `First Access`_ steps.

.. figure:: figures/instance-ready.png
   :align: center
   :alt: Instance ready
   :scale: 75%
   :figclass: text

   OAR instance console.

^^^^^^^^^^^^^^^^^^^^^
VirtualBox deployment
^^^^^^^^^^^^^^^^^^^^^

    .. warning:: This deployment example is provided just for test or demostrative purposes, don't use for production environment.

    .. note:: Sometimes you could experiment problems deploying OAR on Virtualbox using the provided QCOW image. In this case you can convert the disk format from qcow2 to vdi using qemu utils, as described in the `Troubleshooting`_ section.

In order to deploy the image on Virtualbox you should:

1. create a new vitual machine (see Figure `3`_) specifing your machine name, OS type anchitecture, then click *Next* button;
2. specify the machine RAM size, use at least 2GB of RAM (see Figure `4`_), click *Next* button;
3. attach the downloaded image as disk (see Figure `5`_);
4. finally start the virtual machine. It may take some time before start, depends on your hardware.

.. _3:

.. figure:: figures/virt-new-vm.png
   :align: center
   :alt: New Virtual Machine
   :scale: 88%
   :figclass: text

   Create new Virtual Machine.

.. _4:

.. figure:: figures/virt-set-RAM.png
   :align: center
   :alt: Set RAM size
   :scale: 88%
   :figclass: text

   Specifiy the RAM size.

.. _5:

.. figure:: figures/virt-attach-disk.png
   :align: center
   :alt: Attach oar image
   :scale: 78%
   :figclass: text

   Attach oar image.

Once the virtual machine is up and running provide the default credentials to login into (see Figure `6`_).

.. _6:

.. figure:: figures/virt-opendata-template.png
   :align: center
   :alt: OAR template
   :scale: 88%
   :figclass: text

   OAR template console.

The image is equiped with 20GB dinamically allocated disk, if you need more disk space you can perform the following commmands:

1. shtdown the Virtual machine;
2. from your guest system perform the **VBoxManage modifyhd** specifying the new Hard disk size in MB:

.. code::

    VBoxManage modifyhd /path/to/the/oar.sci-gaia-vm-20150819.vdi --resize <new_size(MB)>
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%

3. restart the Virtual Machine, login into and check the disk size using:

.. code::

    invenio@opendata-template:~$ df -Th

---------------
Troubleshooting
---------------

In this section there are some possible solutions to the problems you could face during the OAR template deployment.


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Cannot access Virtual Machine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Problem**

Although you provide the right credentials you cannot access the Virtual Machine from console, see Figure `7`_

.. _7:

.. figure:: figures/virt-error-access.png
   :align: center
   :alt: Error Accessing the Virtual Machine
   :scale: 88%
   :figclass: text

   Error Accessing the Virtual Machine.

**Solution**

This problem is often related to the keyboard layout loaded, please check the special character typing them temporarly on the username to be sure that you are typing the right password.

^^^^^^^^^^^^^^
Disk extension
^^^^^^^^^^^^^^

**Problem**

If you successfully excuted a disk extension, but when you check the size you still see the default size.

.. code::

 root@opendata-template:~# df -Th
 Filesystem     Type      Size  Used Avail Use% Mounted on
 /dev/sda1      ext4       20G  7.3G   12G  39% /
 none           tmpfs     4.0K     0  4.0K   0% /sys/fs/cgroup
 udev           devtmpfs  997M   12K  997M   1% /dev
 tmpfs          tmpfs     201M  376K  200M   1% /run
 none           tmpfs     5.0M     0  5.0M   0% /run/lock
 none           tmpfs    1001M     0 1001M   0% /run/shm
 none           tmpfs     100M     0  100M   0% /run/user

 root@opendata-template:~# fdisk -l

 Disk /dev/sda: 104.9 GB, 104857600000 bytes
 4 heads, 32 sectors/track, 1600000 cylinders, total 204800000 sectors
 Units = sectors of 1 * 512 = 512 bytes
 Sector size (logical/physical): 512 bytes / 512 bytes
 I/O size (minimum/optimal): 512 bytes / 512 bytes
 Disk identifier: 0x00045d27

    Device Boot      Start         End      Blocks   Id  System
 /dev/sda1   *        2048   204799999   102398976   83  Linux

**Solution**

Problably you need to perform the **resize2fs** to enlarge the file system, as shown below that expands the disk size from 20GB to 100GB:

.. code::

 root@opendata-template:~# resize2fs /dev/sda1
 resize2fs 1.42.9 (4-Feb-2014)
 Filesystem at /dev/sda1 is mounted on /; on-line resizing required
 old_desc_blocks = 2, new_desc_blocks = 7
 The filesystem on /dev/sda1 is now 25599744 blocks long.

 root@opendata-template:~# df -Th
 Filesystem     Type      Size  Used Avail Use% Mounted on
 /dev/sda1      ext4       97G  7.3G   85G   8% /
 none           tmpfs     4.0K     0  4.0K   0% /sys/fs/cgroup
 udev           devtmpfs  997M   12K  997M   1% /dev
 tmpfs          tmpfs     201M  376K  200M   1% /run
 none           tmpfs     5.0M     0  5.0M   0% /run/lock
 none           tmpfs    1001M     0 1001M   0% /run/shm
 none           tmpfs     100M     0  100M   0% /run/user

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Virtualbox instance doen't start
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Problem**

As pointed in the `VirtualBox deployment`_ section you couldn't be able to start the Virtual Machine due to Hard Disk related problems.

**Solution**

In this case you tray to convert the downloaded image format from QCOW2 to VDI. Following the steps to convert image format.

1. Install **qemu-utils**

.. code::

    apt-get install qemu-utils

2. Convert the image format:

.. code::

    qemu-img convert -f qcow2 <qcow2_VM_filename> -O vdi <VDI_file_VM_filename>

3. Use the just created vdi image to start the Virtual Machine.
