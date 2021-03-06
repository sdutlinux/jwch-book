LVM (逻辑卷管理器)
===========================================

:作者: zhwei http://zhangweide.cn

介绍
-------------------------------------------

LVM *Logical Volume Manager*

LVM是Linux环境下对磁盘分区进行管理的一种机制，LVM是建立在硬盘和分区之上的一个逻辑层，来提高磁盘分区管理的灵活性。

能解决那些问题
--------------

+ 在线通过增加或减少物理卷组改变逻辑卷组的大小

+ 在小型系统上，比如个人电脑，不必在安装系统的时候费脑筋估算分区的大小，lvm上以后可以按需求轻易调整分区的大小。

+ 能够对逻辑分区实现一致性备份

+ 可以在多个物理卷或者整个硬盘上创建单个分区，有点像RAID 0, 但更像JBOD, 允许动态调整卷的大小。

创建LVM
-------------------------------------------

如果要在/dev/sda3, /dev/sda4, /dev/sda5上创建lvm

#. 创建物理卷

.. code-block:: bash

    pvcreate /dev/sda{3..5}

#. 创建为卷组

.. code-block:: bash

   vgcreate test0 /dev/sda{3..5}

#. 在卷组test0上创建10G的逻辑卷 lv_0

.. code-block:: bash

   lvcreate -L 10G -n lv_0 test0

#. 创建文件系统并挂载

.. code-block:: bash

   mkfs.ext4 /dev/test0/lv_0


扩容
------------------------------------------

需求： 将物理卷/dev/sda6 加入lvm并扩大为20G


#. 创建物理卷

.. code-block:: bash

   pvcreate /dev/sda6


#. 添加到卷组test0

.. code-block:: bash

   vgextend test0 /dev/sda6


#. 扩展逻辑卷, 扩展到20G

.. code-block:: bash

   lvextend -L 20G /dev/test0/lv_0


#. 使增加的容量生效

.. code-block:: bash

   resize2fs /dev/test0/lv_0

搞定

减容
------------------

需求： 将逻辑卷减小成10G

#. 先将该分区卸载

.. code-block:: bash

    umount /data

#. 检查逻辑卷

.. code-block:: bash

   e2fsck -f /dev/test0/lv_0

#. 调整文件系统大小

.. code-block:: bash

   resize2fs /dev/test0/lv_0 10G

#. 对逻辑卷进行调整

.. code-block:: bash

   lvreduce /dev/test0/lv_0 10G


删除lvm
--------------------

.. code-block:: bash

    lvremove /dev/test0/lv_0

    vgremove /dev/test0

    pvremove /dev/sda{3..5}

更多用法
----------------------

.. code-block:: bash

    $ sudo lvm

    lvm> help
      Available lvm commands:
      Use 'lvm help <command>' for more information

      dumpconfig      Dump active configuration
      formats         List available metadata formats
      help            Display help for commands
      lvchange        Change the attributes of logical volume(s)
      lvconvert       Change logical volume layout
      lvcreate        Create a logical volume
      lvdisplay       Display information about a logical volume
      lvextend        Add space to a logical volume
      lvmchange       With the device mapper, this is obsolete and does nothing.
      lvmdiskscan     List devices that may be used as physical volumes
      lvmsadc         Collect activity data
      lvmsar          Create activity report
      lvreduce        Reduce the size of a logical volume
      lvremove        Remove logical volume(s) from the system
      lvrename        Rename a logical volume
      lvresize        Resize a logical volume
      lvs             Display information about logical volumes
      lvscan          List all logical volumes in all volume groups
      pvchange        Change attributes of physical volume(s)
      pvresize        Resize physical volume(s)
      pvck            Check the consistency of physical volume(s)
      pvcreate        Initialize physical volume(s) for use by LVM
      pvdata          Display the on-disk metadata for physical volume(s)
      pvdisplay       Display various attributes of physical volume(s)
      pvmove          Move extents from one physical volume to another
      pvremove        Remove LVM label(s) from physical volume(s)
      pvs             Display information about physical volumes
      pvscan          List all physical volumes
      segtypes        List available segment types
      vgcfgbackup     Backup volume group configuration(s)
      vgcfgrestore    Restore volume group configuration
      vgchange        Change volume group attributes
      vgck            Check the consistency of volume group(s)
      vgconvert       Change volume group metadata format
      vgcreate        Create a volume group
      vgdisplay       Display volume group information
      vgexport        Unregister volume group(s) from the system
      vgextend        Add physical volumes to a volume group
      vgimport        Register exported volume group with system
      vgmerge         Merge volume groups
      vgmknodes       Create the special files for volume group devices in /dev
      vgreduce        Remove physical volume(s) from a volume group
      vgremove        Remove volume group(s)
      vgrename        Rename a volume group
      vgs             Display information about volume groups
      vgscan          Search for all volume groups
      vgsplit         Move physical volumes into a new or existing volume group
      version         Display software and driver version information

