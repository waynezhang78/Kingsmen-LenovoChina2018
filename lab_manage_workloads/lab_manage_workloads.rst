.. _lab_manage_workloads:

------------------------
初级实验4-B:AHV的虚机管理 
------------------------

预计完成时间: 15分钟
本实验使用Prism Central
在浏览器中打开Prism Central链接，使用admin用户登录Prism Central界面

实验目的
++++++++

学习并掌握通过Prism进行VM管理任务的经验，包括电源操作，搜索，克隆和迁移。

工作负载管理
+++++++++++++++++++

现在您已经成功部署了几个虚拟机，让我们碰探索一些在AHV平台上如何进行虚拟机管理的乐趣。

电源操作和控制台访问
................................

在**Prism Central > Explore > VMs**菜单.

找到您在上一个实验中创建的Linux VM (Linux_VM-*initials*). (必要时可以使用Prism的搜索功能)

.. note::

  请注意，如果该VM的Power State列显示一个红点，表示VM已关闭
  
现在我们来启动VM:

选择VM，然后从**Actions**下拉菜单中单击**Power On**

.. note::

  请同时查看所有可用操作列表（包手更新，删除，克隆，启动控制台，开机，暂停/挂起，快照，迁移等）
  
  请注意，此步骤Launch Console功能带有阴影，表明此操作不可用，因为此时VM正处于关闭状态。
  
接下来打开一个控制台会话:

选择VM，然后从**Actions**下拉菜单中单击**Launch Console**.

.. note::

  - 控制台窗口打开时，请注意控制台中有三个可用操作(发送CTRL-ALT-DEL，截屏和电源操作).
  - 一旦VM启动，“Actions”菜单中的“powered on”将更改为“Power Off”。
  - 您还可以单击VM的名称以打开特定VM的控制台窗口。
  - 此控制台窗口包含“操作”菜单下可用的所有选项，与性能相关的信息以及其他相关的VM详细信息。

.. figure:: images/manage_workloads_01.png

.. note::

 在ESXi环境:

  - 对于ESXi环境，VMware vCenter实例注册到Prism，本实验的所有步骤同样有效。 
  - 下图显示了在ESXi中托管的VM与AHV中托管的VM之间的“操作”下拉菜单的横向比较.

  .. figure:: images/manage_workloads_06.png

Prism搜索功能
............

Prism搜索功能可以更轻松地识别问题或在Prism Central中查找相关文档资料。
通过键入一些搜索条件来使用Prism Central的搜索功能，以体验如何轻松完成上述任务。

一些建议尝试的搜索条件如下（可以根据需求自定义):

- vm cpu > 1
- vm mem > 2
- vm iops
- create vm
- powered on
- powered on cpu = 8

在**Prism Central >** 点击放大镜图标:fa:`search`.

- 请注意结果类型：实体，警报和帮助
- 可通过单击星形图标以保存搜索.

.. note::

  搜索功能有快捷键(斜杠符号"/"),在Prism Central的任何界面上都可以通过'/'快捷键直接调用搜索功能.

VM克隆
..........

在**Prism Central > Explore > VMs**.

查找并基于CentOS的虚拟机克隆四个副本

选择VM，然后从**Actions**下拉菜单中单击**Clone**

按如下内容填写以下字段，然后单击**保存**

- **Number of Clones** - 4
- **Prefix Name**  - Flow-*initials*-Clone
- **Starting Index Number** - 1

.. figure:: images/manage_workloads_02.png

这些虚拟机可以保持关闭状态，他们将在可选高级实验-Flow实验中使用。

在主机之间进行VMotion迁移
..........................

在**Prism Central > Explore > VMs**菜单中.

找到上一个实验中创建的虚拟机(Linux_VM-*initials*).

- 如果VM已开机，请将其关机

您应该可以看到在关闭电源后,VM的**Host**列中没有条目。

启动VM，并记下**Host**列中的**Hosts Name**
.. figure:: images/manage_workloads_03.png

选择VM，然后从**Actions**下拉菜单中单击**Migrate**

您可以选择群集中的其他主机之一作为VM的迁移目标，也可以接受默认值并让AHV自动选择位置。

单击**Migrate**以完成操作.

任务完成后，请验证您的VM主机位置是否已从上面记录的主机更改为您选择的新位置。

.. figure:: images/manage_workloads_04.png

配置VM到主机的关联策略
......................................

在**Prism Central > Explore > VMs**.

找到上一个实验中创建的虚拟机(Linux_VM-*initials*).

- 如果VM已开机，请将其关机

选择VM，然后从**Actions**下拉菜单中单击**Configure VM Host Affinity**

选择一个可以与VM关联的**Host**，如NTNX-AHV-2,然后单击“Save”完成

启动VM，并验证它是否在您在关联策略中选择的**Host**上。

选择VM，然后从**Actions**下拉菜单中单击**Migrate**

此时会看到类似如下提示：

-此VM已将主机关联设置为主机NTNXAHV-2，如果不将主机关联设置为该主机，则无法将其迁移到任何其他主机
 （This VM has host affinity set to host NTNXAHV-2. It cannot be migrated to any other host without setting the host affinity to that host.）

单击**Cancel**以退出迁移

选择VM，然后从**Actions**下拉菜单中单击**Configure VM Host Affinity**。

选择VM可以与之关联的另一个**Host**，然后单击“Save”完成。

选择VM，然后从**Actions**下拉菜单中单击**Migrate**。

- 现在有一个显示可用主机的下拉菜单

手动选择主机或允许AHV自动选择，然后单击**Migrate**

您应该看到VM已移至另一台主机

.. figure:: images/manage_workloads_05.png

高可用性
.................

AHV默认启用高可用性，并在主机发生故障时以尽力而为的方式重启VM，我们可以通过额外的配置以预留足够资源，并确保在故障事件期间始终能够保证HA有充足资源实现。

VMware HA的工作原理是通过将虚拟机及其驻留的主机集中到群集中来为虚拟机提供高可用性，然后监视该群集中的主机，如果发生故障，驻留在故障主机上的VM将在备用主机上重新启动，VmwareHA功能必须在vSphere中手动开启，AHV默认情况下HA处于打开状态且无需进行资源保留即可生效。



小贴士
+++++++++

 - 在本实验中，您应该体验了在AHV上如何提供一套完整的工具和操作流程，以便管理群集中的VM
 - 可以将ESXI集群注册到Prism，并且可以直接从Prism执行一些基本的VM管理任务
