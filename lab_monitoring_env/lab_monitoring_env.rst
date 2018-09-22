.. _lab_monitoring_env:

----------------
中级实验6:监控与管理 
----------------
预计完成时间: 20分钟

本实验使用Prism Central,在浏览器中打开Prism Central链接，使用admin用户登录Prism Central界面

实验目的
++++++++

了解Prism Central的监控和资源规划功能，这些功能可帮助您掌握集群利用率并更准确地预测集群扩展。

Prism Central报告功能
+++++++++++++++++++++

Prism Central允许您生成有关群集环境的历史报告。
此类报告可包括资源消耗，异常行为和其他有价值的技术建议。

在**Prism Central > Explore > Reports**.

我们先尝试运行**Cluster Efficiency Summary**报告.

选择**Cluster Efficiency Summary**,然后从**Actions**下拉菜单中单击**Run**.

.. figure:: images/monitoring_01.png

然后，按提示输入以下字段，并点击**Run**按钮:

- **Report instance Name** - Cluster Efficiency Summary - *initials*
- **Time Period for Report** - Last 24 Hours

我们再运行一下**Environment Summary**报告。

选择**Environment Summary**,然后从**Actions**下拉菜单中单击**Run**.

接下来，填写以下字段，然后单击**运行**:

- **Report instance Name** - Environment Summary - *initials*
- **Time Period for Report** - Last 24 Hours

报告完成后，选择要查看的报告项目，然后执行以下操作：

点击**Actions**下拉菜单中的**View Instances**。

- 要在单独的选项卡中查看报告，请单击报告的名称.
- 要下载报告，请选中其复选框，然后单击屏幕右上角的**Download**.

查看您在本练习中创建的报告的内容。

Capacity规划
...............

使用Prism Central的Capacity Runway功能来了解群集资源规划和建议。

在**Prism Central > Planning > Capacity Runway**菜单.

- 注意规划总结摘要中显示的是每个群集资源用尽所需的剩余天数.
- 查查看我们实验环境的集群，到内存，CPU和存储空间资源耗尽还剩下多长时间？

单击其中一个群集
.. note::

  资源最紧张的项目会在左侧突出显示
  
.. note::

  单击“存储”，“CPU”或“内存”资源利用率趋势将分别显示该资源的图表

单击**Optimize Resources**以查看用于资源重新分配的建议管理任务列表，例如优化过度配置的VM，删除非活动VM或向受约束的VM添加资源。

关闭Capacity Runway视图.

假设预估规划
................

基于根据现有容量规划任务的初始猜测，再添加一些假设的新型工作负载规划，查看预测报告会如何变化。

在**Prism Central> Planning> Scenarios**中，然后单击**New Scenario**。

接下来，按提示填写以下字段：
- **Cluster** - Select a Cluster Model
- **Target** - 6 months
- **Vendor Type** - Nutanix

现在，我们可以添加150个座席的Citrix XenDesktop工作负载。

点击**+ Add Workload**.

接下来，根据提示填写以下字段，然后单击**Add Workload**：
- **Workload** - VDI
- **Vendor** - XenDesktop
- **User Type** - Power Users
- **Provision Type** - Machine Creation Services (MCS)
- **Number of Users** - 150
- **On** - One Month from now

重复此过程，继续添加工作负载，直到资源无法满足六个月的目标。
点击**Save**保存此场景.

请注意**Resources**部分，显示的是现有硬件配置。

单击**Recommend**以查看建议的NX配置以扩展集群来满足未来需求。

现在让我们再尝试更改目标和工作负载并生成新的建议：

- 三个月内增加150个座位.
- 每三个月发生新的需求变化.

生成PDF报告以查看详细的容量规划信息


开放实验: 创建自定义报告
...................................

要从头开始创建新的自定义报告，请单击**Actions**旁边的加号，然后从左侧的窗格中添加所需的度量标准。

保存自定义报告后，您可以像运行任何其他报告一样运行它。

如果想要将报告设置为自动运行，请为其添加日程计划即可。


小贴士
+++++++++

 -  Prism Central报告管理功能使您能够根据配置的计划配置和提供包含有关基础结构资源的信息的历史报告。
 -  Planning仪表板中的Capacity Runway视图允许您查看已注册集群的摘要资源用尽预测，并访问有关每个集群的详细资源消耗预测信息。
 - “规划”仪表板中的“方案”视图允许您创建“假设”方案，以评估您指定的潜在工作负载的未来资源要求。
 -  您必须拥有Prism Pro许可才能使用资源规划工具。
