.. _lab_storage_configuration:

---------------------------
初级实验2:存储容器配置
---------------------------
预计完成时间: 10分钟

本实验使用Prism Element，在浏览器中打开Prism Element链接，登录Prism Element界面


实验目的
++++++++

在本实验中，我们将使用Prism Element软件进行基本的存储容器设置.

Prism Element存储配置项
+++++++++++++++++++++++++++++++++++++++++

配置存储容器Container
............................

**Containers** 是软件定义的逻辑结构，通过设定存储策略来实现存储管理的简化。容器的概念相当于ESXi的datastore

我们先用Prism Element来开始基本的Container创建操作.

在菜单中选择**Prism > Storage**, 点击**Storage**标签栏, 点击**Table**,然后单击**+ Storage Container**.

使用以下参数进行本次实验配置 (部分项目需要点击高级设置**Advance Settings**), 然后点击 **Save**:

- **Name** - container-*intials*(
- **Advertised Capacity** - 5 GiB
- **Compression** - Enabled (inline 0 mins)
- **Deduplication** - Cache Only
- **Erasure Coding** - Disabled

.. figure:: images/storage_config_01.png

您可以使用不同的策略创建多个不同用途的容器

.. 注::

  容器不会预先保留任何实际的磁盘空间，只是会设置能够触发警报的软限制策略，但不会阻止新的数据写入到容器。

进一步了解容器配置的技巧：

返回上面创建的容器并对其进行编辑，并尝试增加10Gb的容量。 
此时请查看在上一个任务中创建的冗余因子是多少？ 

.. figure:: images/storage_config_02.png

冗余因子
.................

我们已经讨论了Nutanix集群如何处理数据：读取，写入，CVM自动路径，数据位置，智能分层和无缝VM迁移。在利用这些特性和功能的同时，Nutanix集群可持续监控和处理数据摆放，以优化性能并允许集群在软件升级和故障期间保持高可用性。

您可以在Prism中找到容器和集群的冗余级别。

.. figure:: images/storage_config_03.png

在**Prism> Home**中，单击Data Resiliency Status框中的**green OK**。这将打开“数据弹性状态”窗口

上图显示了Data Resiliency Status窗口，您可以在其中查看，诸如可以容忍多少ZooKeeper节点故障而不会影响群集。
列出的每个服务都在群集中具有特定功能，Zookeeper节点是用来维护集群的配置信息。

可以通过单击齿轮菜单中的**Redundancy State**来配置Prism Element中的RF冗余度。

.. 注::

  在本练习中，请将冗余因子配置为2.

在**Prism >**菜单中:fa:`cog`, 点击 **Redundancy State**.

小贴士
+++++++++

- 默认群集冗余因子设置为2.冗余因子为2的群集占总可用空间的1/2（30 TB = 15 TB可用空间），因为保留了两个数据副本。
- ZooKeeper容忍度为1，意味着群集中的一个组件（一个CVM，一个NIC，一个磁盘等）可以关闭而不会影响数据弹性；容忍度为2的故障意味着群集中的两个组件可以关闭而不会影响数据弹性。这两个组件可以是不同类型。
