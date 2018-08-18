.. _lab_nutanix_technology_overview:

---------------------------------
实验室 - Nutanix管理界面介绍
---------------------------------

简介
++++++++

本实验将介绍Nutanix两大管理界面，Prism Element＆Prism Central UI，并让您熟悉它们的界面布局和功能导航。

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

From the Firefox web browser (preferred), log into the Nutanix Prism GUI using the Cluster IP.

Open https://<*NUTANIX-CLUSTER-IP*>:9440

Fill out the following fields and click **Enter**:

- **Username** - admin
- **Password** - *HPOC Password*

.. figure:: images/nutanix_tech_overview_01.png

After you log in to Prism Element, familiarize yourself with the Prism UI. Explore the information on the **Home** screen, as well as the other screens.

Review the Home screen, and identify the following items:

- Hypervisor
- Version
- Hardware Model
- Health
- VM Summary
- Warning Alerts
- Data Resiliency Status

.. figure:: images/nutanix_tech_overview_02.png

Review the UI navigation options.

.. figure:: images/nutanix_tech_overview_03.png

Examine the cluster hardware by using the navigation menu, and go to the Hardware.

In **Prism > Hardware**, click **Hardware**, then click **Diagram**.

Review the hardware summary information:

- Blocks
- Hosts
- Memory
- CPU
- Disks

.. figure:: images/nutanix_tech_overview_04.png

Review the other sections, and do a quick walk through:

- VM
- Health
- Network
- Data Protection
- Storage
- Alerts
- Etc.

Review other sections of the Prism UI

- Health :fa:`heartbeat`
- Alarms :fa:`bell`
- Tasks :fa:`circle-o`
- Search :fa:`search`
- Help :fa:`question`
- Configuration :fa:`cog`
- User :fa:`user`

.. figure:: images/nutanix_tech_overview_05.png

.......................
Prism Element UI Review
.......................

Where would you locate the version of AOS you are running?

.. figure:: images/nutanix_tech_overview_06.png

You can do this by clicking on the **User** drop down :fa:`user`, and clicking **About Nutanix**.

How would you get to the following screen to view a summary of the number of hosts (or nodes) and the resource capacity and current utilization?

.. figure:: images/nutanix_tech_overview_07.png

In **Prism > Hardware**, click **Hardware**, then click **Table**.

How would you get the following screen to see the health of your cluster?

.. figure:: images/nutanix_tech_overview_08.png

In **Prism > Health**, click **Health**, then click **Summary** in the right pane.

What page would show you the latest activity in the system? On this page, you can monitor the progress of any task and keep track of what has been done in the past using time stamps. Can you figure out two different ways to get there?

.. figure:: images/nutanix_tech_overview_09.png

First Way, In **Prism > Tasks**, click **Tasks**. Second Way, click :fa:`circle-o`.

.. note::

  In ESXi:

  - The containers created in Prism appear as datastores in vCenter.

  Example view of storage containers from Prism:

  .. figure:: images/nutanix_tech_overview_13.png

  Example view of storage containers (datastores) from vCenter:

  .. figure:: images/nutanix_tech_overview_14.png

Takeaways
+++++++++

- Prism is thoughtfully laid out UI
- Critical information is displayed front and center
- Prism Central can manage multiple clusters
