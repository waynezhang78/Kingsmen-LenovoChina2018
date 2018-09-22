.. _lab_data_protection:

---------------------
中级实验5:数据保护与恢复
---------------------
预计完成时间: \ **15分钟**

本实验使用\ **Prism Element**

在浏览器中打开Prism Element链接，登录Prism Element界面。


实验目的
++++++++

本实验主要学习如何设置保护域(Protection Domains)，创建VM快照以及从这些快照恢复数据。

数据保护
+++++++++++++++

在Prism中，数据保护策略称为保护域（PD）。

PD由一组VM和策略组成，可用策略包括快照，复制目标位置和日程计划。

VM快照与恢复
............

创建VM快照并从快照还原VM。

在**Prism Element> VM**中，单击**VM**，然后单击**Table**

找到您在上一个实验中创建的Linux VM（Linux_VM- *intials*）
 - 如果VM已开启，请将其关闭

选择VM，然后从VM列表下方的菜单中单击 **Snapshot**。

.. figure:: images/data_protection01.png
 
为快照起一个名字(注:intials输入实验人员的姓名全名的拼音）

.. figure:: images/data_protection02.png

点击submit提交

返回VM列表并单击VM的名称以打开其控制台窗口

单击**Snapshots**以查看快照

.. figure:: images/data_protection03.png

- 请注意此步骤还有四个可用操作(Details, Clone, Restore, and Delete)（详细信息，克隆，还原和删除）

.. figure:: images/data_protection04.png

单击**Details**可查看快照时的所有VM属性

现在我们可以通过删除磁盘模拟我们的测试VM遭到损坏的情况

单击VM下面菜单中的Power Off Actions,选择Guest Shutdown关闭虚机

VM前面的绿点变为红点后，单击下面菜单中的**Update**并修改您拍摄快照的原始VM

.. figure:: images/data_protection05.png

向下滚动到磁盘部分，通过点击**X**图标，删除CD-ROM和DISKS

点击**Save**确认更改.

.. figure:: images/data_protection06.png


现在尝试启动该VM并打开其控制台窗口

 .. figure:: images/data_protection07.png

 - 注意，VM此时不再有任何可引导的磁盘，并且显示2048游戏。
 
 .. figure:: images/data_protection08.png


关闭VM电源。

选择VM，然后从VM列表下方的菜单中单击**VM Snapshots**.

点击**Restore**将VM恢复到删除磁盘之前的状态.

 .. figure:: images/data_protection09.png


尝试打开VM并打开控制台。

验证VM是否已成功引导以及其配置是否已还原

配置保护域（PD）
..................................

创建Async DR （数据异步复制）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在**Prism Element > Data Protection**中, 单击**Data Protection**, 然后点击**Table**.

选择**+ Protection Domain** 以创建PD，然后单击创建Async DR.

.. figure:: images/data_protection10.png

输入PD的名称PD-\ *NAME* （注: NAME输入实验人员的姓名全名的拼音）

然后单击**Create**.

选择您希望加入PD的VM:

- 通过过滤或滚动以选择您在此训练营中创建的VM，加入到此PD中
- 向下滚动并单击**Protect Selected Entities**.
- 所选VM显示在右侧表中，点击**Next**.

 .. figure:: images/data_protection11.png


**配置日程计划**:

- 单击**New Schedule**.

-  选择备份频率(如,每两小时做一次快照).

-  设置保留策略(比如, 保持最多12份快照).

-  单击\ **Create Schedule**.

 .. figure:: images/data_protection12.png

-  一个保护域可以有多个计划

-  点击\ **Close**\ 退出.

 .. figure:: images/data_protection13.png

加入远程站点
~~~~~~~~~~~~~~~~~~~~~~~

注:本地备份是此实验室环境中的唯一选项，没有配置远程目标，在有远程站点的情况下，可以单击\ **+Remote Site**\ 进行配置，远程站点可以是Nutanix物理集群环境，或者是公有云环境。
.. note::

  本地备份是此实验室环境中的唯一选项，因为未配置远程目标。
  您可以使用邻居群集设置远程站点
  
单击**Create Schedule**.

.. note::

  一个保护域可以有多个计划
  
点击**Close**退出.

 .. figure:: images/data_protection13.png


小贴士
+++++++++

 -  Nutanix通过不同的策略为虚拟数据中心提供数据保护解决方案，包括一对一或一对多复制。
 -  Nutanix在VM，文件和卷组级别提供数据保护功能，因此VM和数据在崩溃一致的环境中保持安全。
 - 您可以通过Web控制台配置保护域和远程站点来实施数据保护策略。
 
