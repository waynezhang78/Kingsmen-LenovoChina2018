.. _lab_data_protection:

---------------------
数据保护实验
---------------------

概览
++++++++

了解如何设置保护域(protection domains)，创建VM快照以及从这些快照恢复数据。

数据保护
+++++++++++++++

在Prism中，数据保护策略称为保护域（PD）。 PD由一组VM和策略组成。可用策略包括快照，复制目标位置和日程计划。

VM快照
............

创建VM快照并从快照还原VM。

在**Prism Element> VM**中，单击**VM**，然后单击**Table**

找到您在上一个实验中创建的Linux VM（Linux_VM- * intials *）
 - 如果VM已开启，请将其关闭

选择VM，然后从VM列表下方的菜单中单击 **Snapshot**。
 
为快照起一个名字

返回VM列表并单击VM的名称以打开其控制台窗口

单击**Snapshots**以查看快照

- 请注意此步骤还有四个可用操作(Details, Clone, Restore, and Delete)（详细信息，克隆，还原和删除）

单击**Details**可查看快照时的所有VM属性

现在我们可以通过删除磁盘模拟我们的测试VM遭到损坏的情况

单击下面菜单中的**Update**并修改您拍摄快照的原始VM

- 向下滚动到磁盘部分，通过点击**X**图标，删除CD-ROM和DISKS
- 点击**Save**确认更改.

现在尝试启动该VM并打开其控制台窗口
 - 注意，VM此时不再有任何可引导的磁盘，并且显示2048游戏。
 
关闭VM电源。

选择VM，然后从VM列表下方的菜单中单击**VM Snapshots**.

点击**Restore**将VM恢复到删除磁盘之前的状态.

尝试打开VM并打开控制台。

验证VM是否已成功引导以及其配置是否已还原

配置保护域（PD）
..................................

在**Prism Element > Data Protection**中, 单击**Data Protection**, 然后点击**Table**.

选择**+ Protection Domain** 以创建PD，然后单击创建Async DR.

提供PD的名称，然后单击**Create**.

选择您希望成为PD成员的VM:

- Filter or scroll to select the VMs you created in this bootcamp to be part of the PD.
- Scroll down and click **Protect Selected Entities**.
- The selected VMs appear in the right-hand side table. Click **Next**.

Configure a local schedule:

- Click **New Schedule**.
- Select a frequency (for example, repeat every one day).

Configure a retention policy:

- Set the retention policy (for example, keep the last two snapshots).

.. note::

  Local is the only option in this lab environment because no remote targets are configured.

  You could setup a remote site with a neighbor cluster.

Click **Create Schedule**.

.. note::

  A Protection Domain can have multiple schedules.

Click **Close** to exit.

Takeaways
+++++++++

- Nutanix offers data protection solutions for virtual datacenters via different strategies including one-to-one or one-to-many replication.
- Nutanix provides data protection functions at the VM, file, and volume group level, so VMs and data remain safe in a crash-consistent environment.
- You can implement a data protection strategy by configuring protection domains and remote sites through the web console.
