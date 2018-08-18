.. _lab_manage_workloads:

------------------------
工作负载管理实验
------------------------

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

The Prism search function makes it easier to identify problems or find feature documentation in Prism Central. Use Prism Central’s search capabilities by typing a few search queries to see how easy this can make the tasks above.


Suggestions:

- vm cpu > 1
- vm mem > 2
- vm iops
- create vm
- powered on
- powered on cpu = 8

In **Prism Central >** :fa:`search`.

- Note the result types: Entity, Alerts, and Help.
- Click the star icon to save a search.

.. note::

  The search hot key (a slash mark, or /) can be used from anywhere in the Prism Central UI to bring up the search function.

Clone a VM
..........

In **Prism Central > Explore > VMs**.

Find and clone four copies of the CentOS-base virtual machine.

Select the VM, then click **Clone** from the **Actions** drop-down menu.

Fill out the following fields and click **Save**:

- **Number of Clones** - 4
- **Prefix Name**  - Flow-*initials*-Clone
- **Starting Index Number** - 1

.. figure:: images/manage_workloads_02.png

Leave them powered off as they are used in the optional Flow Lab.

Migrate a VM Between Hosts
..........................

In **Prism Central > Explore > VMs**.

Locate the Linux Vm from the previous lab (Linux_VM-*initials*).

- If the VM is powered on, power it Off

You should see that it has no entry in the **Host** column when it is powered off.

Power on the VM, and make note of the **Hosts Name** in the **Host** column.

.. figure:: images/manage_workloads_03.png

Select the VM, then click **Migrate** from the **Actions** drop-down menu.

You can either choose one of the other hosts in the cluster as a migration target for the VM, or accept the default and let AHV automatically select a location.

Click **Migrate** to finalize the action.

When the task completes, verify that your VM host location has changed from the host recorded above to the new location you selected.

.. figure:: images/manage_workloads_04.png

Configure VM-to-Host Affinity Policies
......................................

In **Prism Central > Explore > VMs**.

Locate the Linux Vm from the previous lab (Linux_VM-*initials*).

- If the VM is powered on, power it Off

Select the VM, then click **Configure VM Host Affinity** from the **Actions** drop-down menu.

Select one **Host** to which the VM can have affinity, and click Save to finish.

Power On the VM, and verify it is on the **Host** you selected in the affinity policy.

Select the VM, then click **Migrate** from the **Actions** drop-down menu.

- This VM has host affinity set to host NTNXAHV-2. It cannot be migrated to any other host without setting the host affinity to that host.

Click **Cancel** to exit migration.

Select the VM, then click **Configure VM Host Affinity** from the **Actions** drop-down menu.

Select another **Host** to which the VM can have affinity, and click Save to finish.

Select the VM, then click **Migrate** from the **Actions** drop-down menu.

- There is now a drop-down menu displaying the available hosts.

Either select a host manually or allow AHV to select it, then click **Migrate**.

You should see that the VM has moved to the other host.

.. figure:: images/manage_workloads_05.png

High Availability
.................

High availability is enabled by default for AHV and will restart VMs in a best-effort manner in the event of a host failure. Additional configuration can set resource reservations to ensure there is capacity during an HA event.

VMware HA works by providing high availability for virtual machines by pooling the virtual machines and the hosts they reside on into a cluster. The hosts in that cluster are then monitored and in case there is a failure, the VMs residing on the failed host would get restarted on alternate hosts. This feature must be turned on in vSphere, as opposed to AHV where it’s on by default without reservation.

Takeaways
+++++++++

- In this lab you got to experience first hand how AHV provides a complete set of tools and actions that can be done manage the VMs in the cluster.
- It is possible to register an ESXI cluster to Prism and be able to perform some of the basic VM management tasks right from Prism as well.
