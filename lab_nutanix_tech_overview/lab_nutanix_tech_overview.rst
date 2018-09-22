.. _lab_nutanix_technology_overview:

---------------------------------
初级实验1:NUTANIX管理界面介绍
---------------------------------
预计完成时间: 15分钟

本实验使用Prism Element和Prism Central

简介
++++++++

本实验将介绍Nutanix两大管理界面，Prism Element＆Prism Central，并让您熟悉它们的界面布局和功能导航。
- Prism Element简称PE，是集成在每个CVM管理虚拟机中，用于单个Nutanix日常管理和操作的界面，
- Prism Central简称PC，是额外部署的一套管理虚拟机，支持分布式集群部署，可以同时管理多个虚拟机功能，并提供诸如报告，预测，搜索等高级功能，Nutanix集  成的很多高级功能诸如Flow，Calm也都需要在Prism Central中使用

Prism Central
+++++++++++++

在浏览器中打开 https://<*Prism-Central-IP*>:9440
建议使用Google Chrome或Firefox浏览器

在界面输入登陆用户名和密码信息，并点击**Enter**:

- **Username** - admin
- **Password** - *HPOC Password*

登录Prism Central后，请尝试各功能，以便熟悉Prism UI

浏览**Home** 页面的各种显示信息:

- Cluster Runway
- Cluster Quick Access
- Impacted Cluster | Alerts
- tasks

浏览**Explore** 页面的所有功能选项:

- VMs
- Images
- Clusters
- Hosts
- Disks
- Storage Containers

.. figure:: images/nutanix_tech_overview_10.png

查看其它功能标签栏，并快速浏览:

- Planning
- Analysis
- Apps (We will configure this later in the workshop)
- Alerts
- Tasks :fa:`circle-o`
- Search :fa:`search`
- Help :fa:`question`
- Configuration :fa:`cog`
- User :fa:`user`

.......................
思考时间
.......................

在PC中通过哪个界面能够显示所有被管理的主机的列表？ 

.. figure:: images/nutanix_tech_overview_11.png

.. note::

如果Prism Central同时管理多个集群，该界面会显示所有集群中可管理的主机列表。

在**Prism Central > Explore**界面中, 在左边菜单中点击**Hosts**.

在哪个界面能够看到所有已部署的虚拟机列表？是不是与下图的界面类似？

.. figure:: images/nutanix_tech_overview_12.png

在**Prism Central > Explore**菜单中, 点击菜单栏左边的**VMs**.

在哪个页面会显示系统中的最新活动？
在此页面上，您可以监控任何任务的进度，并使用时间戳跟踪过去的操作。
你能想出两种不同的方法吗？

第一种方式，在**Prism Central > Home**界面中, 点击**View All Tasks**. 
第二种方式, 点击 :fa:`circle-o`

.. note::

  In ESXi:

  - vCenter Server实例可以通过Prism中的:fa:`cog`中进行注册
  - 将运行ESXi的Nutanix集群注册到vCenter，可以让Prism直接运行核心的VM管理操作，而不需要切换至vCenter服务器。
  - 如果vCenter服务器也在Nutanix集群中，则可以被自动发现,IP地址可以被自动填充，如下图所示：

  vCenter注册到Prism的示例图：

  .. figure:: images/nutanix_tech_overview_15.png

Prism Element
+++++++++++++

使用Google Chrome或Firefox浏览器，使用集群IP登陆到 Nutanix Prism GUI.

Open https://<*NUTANIX-CLUSTER-IP*>:9440

使用以下登陆密钥进行登陆**Enter**:

- **Username** - admin
- **Password** - *HPOC Password*

.. figure:: images/nutanix_tech_overview_01.png

登录Prism Element后，随意浏览一下，熟悉Prism的管理界面。浏览** Home **界面以及其他界面上的信息。

查看Home界面，并找到以下功能项并记录：

- Hypervisor
- Version
- Hardware Model
- Health
- VM Summary
- Warning Alerts
- Data Resiliency Status

.. figure:: images/nutanix_tech_overview_02.png

查看快速导航栏

.. figure:: images/nutanix_tech_overview_03.png

使用导航栏进入Hardware菜单，查看集群的硬件状态.

进入**Prism > Hardware**, 点击**Hardware**, 并点击**Diagram**.

查看硬件信息总结:

- Blocks
- Hosts
- Memory
- CPU
- Disks

.. figure:: images/nutanix_tech_overview_04.png

快速浏览其它的菜单:

- VM
- Health
- Network
- Data Protection
- Storage
- Alerts
- Etc.

检查Prism UI的其它部分：

- Health :fa:`heartbeat`
- Alarms :fa:`bell`
- Tasks :fa:`circle-o`
- Search :fa:`search`
- Help :fa:`question`
- Configuration :fa:`cog`
- User :fa:`user`

.. figure:: images/nutanix_tech_overview_05.png

.......................
思考时间
.......................

1.在哪个界面能找到正在运行的AOX版本？

.. figure:: images/nutanix_tech_overview_06.png

答：您可以在点击**User**的下拉菜单中，单击** About Nutanix **.

2.您如何进入以下界面以查看主机（或节点）数量以及资源容量和当前利用率的摘要？

.. figure:: images/nutanix_tech_overview_07.png

答：在**Prism > Hardware**, 点击**Hardware**, 然后点击**Table**.

3. 您可以在哪个界面检查集群的健康检查状态？

.. figure:: images/nutanix_tech_overview_08.png

答：在**Prism > Health**, 点击**Health**, 然后点击右侧的**Summary**菜单.

4. 在哪个页面能够显示系统中的最新活动？
   在哪个页面上，您可以监控所有任务的进度，并使用时间戳跟踪过去的操作。你能想出两种不同的方法吗？

.. figure:: images/nutanix_tech_overview_09.png

第一种方式，在**Prism > Tasks**, 点击**Tasks**. 
第二种方式, 点击 :fa:`circle-o`.


.. note::

  在ESXi中:

  - 在Prism中创建的容器在vCenter中显示为datastores.

  Prism存储容器的示例视图:

  .. figure:: images/nutanix_tech_overview_13.png

  vCenter中存储容器（Datastore）的示例视图:

  .. figure:: images/nutanix_tech_overview_14.png

小贴士
+++++++++

- Prism是通过精心设计的UI界面
- 关键信息显示在前面和中间
- Prism Central可以同时管理多个集群
