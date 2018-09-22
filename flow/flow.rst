.. _enable_flow:

-------------
高级实验2：FLOW网络虚拟化
-------------
本实验预计完成时间:40分钟

本实验使用Prism Central,在浏览器中打开Prism Central链接，使用admin用户登录Prism Central界面

实验目的
++++++++

在本实验中您将启用Nutanix Flow的功能，也称为微分段。
首先您需要创建一系列用于Flow实验的虚拟机，然后我们会将虚拟机放置在隔离区并进行观察。您还需要检查隔离策略中的所有可配置选项，并根据设定的需求，创建不同用途的类别。
然后，我们通过使用新创建的类别的创建隔离安全策略，以限制未经授权的访问。
最后，您将创建一个名为**app-<initials>**的应用程序类别，将**AppType：app-<initials>**类别分配给您的应用程序VM，在本练习中使用**flow-<initials>-5**，并创建安全策略以限制应用程序VM从**programs-<initials>：sales-<initials>**类别之外的VM接收ICMP ping请求。

启用网络微分段(Microsegmentation)
++++++++++++++++++++++++++

在浏览器中打开PC并登陆， https://<Prism-Central-IP>:9440/.

在导航栏中，单击右上角的问号，然后展开菜单中的**New in Prism Central**部分

点击**Microsegmentation**.

在**Enable Microsegmentation** 对话框中选中**Enable Microsegmentation**复选框.

.. figure:: images/enable_flow.png

.. note::

   每个Prism Central实例只能启用一次Flow功能。如果您发现在**Microsegmentation**旁边显示绿色复选标记，则表示已经有人为本Prism Central实例启用了Microsegmentation功能。

点击**启用**

.. figure:: images/enable.png


创建五个虚拟机
+++++++++++++++

.. note::

  如果您已在实验 - 部署工作负载练习中创建了Flow虚拟机，请跳过此虚拟机创建部分

现在，您将创建**五个**虚拟机，用于测试Nutanix Flow的功能。我们可以直接使用Prism Central中的名为CentOS的模板虚拟机中创建这些虚拟机。

在**Prism Central> Explore> VMs**中，单击**Create VM**。

填写以下字段，然后单击**保存**：

- **Name** - flow-<your_initials>-1
- **Description** - Flow testing VM
- **vCPU(s)** - 2
- **Number of Cores per vCPU** - 1
- **Memory** - 4 GiB
- Select **+ Add New Disk**

  - **Operation** - Clone from Image Service
  - **Image** - CentOS
  - Select **Add**
- Remove **CD-ROM** Disk
- Select **Add New NIC**

  - **VLAN Name** - Primary
  - **IP Address** - *10.21.XX.42*
  - Select **Add**

克隆其他四个VM:
-------------------------

通过此VM并克隆四次，完成如下5个VM的创建:

flow-<your_initials>-1
flow-<your_initials>-2
flow-<your_initials>-3
flow-<your_initials>-4
flow-<your_initials>-5

选择**flow-<your_initials>-1** 然后单击 **Actions > Clone**.

- **Number of Clones** - 4
- **Prefix Name** - flow-<your_initials>-
- **Starting Index Number** - 2

选择五个新创建的Flow VM，然后单击**Actions> Power on**进行开机。

.. figure:: images/flow_vms_2.png

隔离虚拟机并探索隔离策略
+++++++++++++++++++++++++++++++++++++++++++++++++

确认flow-<your_initials>-1和flow-<your_initials>-2之间的通信

.......................................................

登录Prism Central环境并进入**Explore>VMs**界面。

通过复选框选中flow-<your_initials>-1和flow-<your_initials>-2，并打开两个虚拟机的控制台

点击**Actions > Launch Console**.

.. figure:: images/quarantine_pings.png

使用以下用户凭据登录两个VM：
- **Username** - root
- **Password** - nutanix/4u

通过命令*ifconfig*查找VM的IP，并从**flow-<initials>-1** VM开始连续ping到**flow-<initials>-2** VM。

隔离VM并编辑隔离策略
..............................................

在**Explore > VMs**菜单中找到flow-<your_initials>-2虚拟机.

选择**flow-<your_initials>-2>Actions>Quarantine VMs**。选择**Forensic**并单击**Quarantine**。

.. figure:: images/select_forensic.png

观察一下，VM1到VM2的连续ping操作发生了什么？

导航到**Explore > Security Policies > Quarantine**.

选择右上角的**Update**，然后选择**+ Add Source** 隔离策略（Quarantine policy）.

将flow-<your_initials>-1的IP地址增加到**Subnet/IP**栏中，子网掩码选择**/32**。

单击**Forensic**类别附近的加号（+），并允许任何端口上的任何协议到Forensic隔离类别


思考：
1）该资源可以连接到哪些目标？
2）Forensic和Strict quarantine模式有什么区别?

选择**Next > Apply Now**以保存策略.

在添加source源之后，**flow-<your_initials>-1**和**flow-<your_initials>-2**之间的ping会发生什么？

在界面**Explore > VMs > **flow-<your_initials>-2 > Actions > Unquarantine VM**取消对**flow-<your_initials>-2的隔离.

使用Flow进行环境隔离

++++++++++++++++++++++++++++++

创建一个新类别
.....................

登录Prism Central环境并导航至**Explore > Categories**.

.. note::
   可能存在默认类别或者其它实验人员创建的类别，不要对修改其它的类别，您需要创建一个自定义类别以添加到列表中。

单击**New Category**.

填写以下字段，然后单击**保存**：

- **Name** - Programs-<intials>.
- **Purpose** - This category will be used to tag VMs belonging to the program called "Programs-<intials>", as an example. This category will have "intern" and "sales" values in order to differentiate intern and sales VMs within the **programs-<intials>** category.
- **Values** - interns-<intials>.
- **Values** - sales-<intials>.

.. figure:: images/create_category.png

创建新的安全策略
............................

在Prism Central中导航到 **Explore > Security Policies**.

单击**Create Security Policy**，选择**Isolate Environments**.

填写以下字段，然后单击**Apply Now**:

- **Name** - isolate-interns-sales-<intials>
- **Purpose** - Isolate intern vm traffic from sales.
- **Isolate This Category** - programs-<initials>:interns-<intials>.
- **From This Category** - programs-<initials>:sales-<intials>.
不要选中**Apply the isolation only within a subset of the data center**的复选框。 


.. note::
  “Save and Monitor”按钮允许您保存配置并监视安全策略的工作方式，但不会真正应用，以防止误操作。

.. figure:: images/create_isol_pol.png

应用新的安全策略
.............................

在将类别应用于VM之前，确保通讯是正常的
---------------------------------------------------------------------------

导航到**Explore > VMs**.

通过复选框选中flow-<your_initials>-1和flow-<your_initials>-2，并打开两个虚拟机的控制台

单击**Actions > Launch Console**.

通过命令*ifconfig*查找VM的IP，并从**flow-<initials>-3** VM开始连续ping到**flow-<initials>-4** VM。

.. note::
  ping应该成功，因为这两个VM还没有分配任何类别。
  
为VM flow-3和flow-4分配一个类别：
-------------------------------------------------------
导航到**Explore > VMs**.

选择**flow-<initials>-3**并单击**Actions > Manage Categories**.

在UI左侧的“设置类别”文本框中, 键入关键词"intern"并从自动完成中选择**programs-<initials>:interns-<initials>** 点击Save.

选择**flow-<initials>-4**并单击**Actions > Manage Categories**.

在UI左侧的“设置类别”文本框中, 键入关键词"sales"并从自动完成中选择**programs-<initials>:sales-<initials>** 点击Save.

.. note::
    ping不应该成功，因为这两个VM现在属于program-<initials>：intern-<initials>和programs-<initials>：sales-<initials>两个不同的类别，且属于之前创建的策略isolate -interns-sales-<initials>，策略会阻止两个不同类别之间的虚拟机进行通讯。
    
通过网络微分段确保应用安全
++++++++++++++++++++++++++++++++++++++++++

创建和分配类别
............................

使用新的**app-<initials>**类别来更新**AppType**
------------------------------------------------------

登录Prism Central环境并导航至**Explore > Categories**.

单击**AppType**旁边的复选框. 单击**Actions > Update**.

向下滚动并单击最后一个条目旁边的加号

输入**app-<initials>**,并单击**Save**.

将VM **flow-<initials>-5**分配给类别**AppType: app-<initials>**.
--------------------------------------------------------------

在Prism Central的**Explore > VMs**视图中，单击**flow-<initials>-5** 旁边的复选框.

单击**Actions > Manage Categories**.

在“设置类别”文本框中，键入**AppType**并从自动完成中选择**AppType: app-<initials>**并点击**Save**保存.

.. figure:: images/set_app_category.png


分配VM**flow-<initials>-1**到默认类别**Environment: Dev**
------------------------------------------------------------------

在Prism Central的**Explore>VMs**视图中，单击**flow-<initials>-5** VM旁边的复选框.

单击**Actions > Manage Categories**.

在“设置类别”文本框中，键入**Dev**并从自动完成中选择**Environment: Dev**然后单击**Save**.


保护应用程序VM
.........................

创建新的安全策略以保护**app-<initials>**应用程序
--------------------------------------------------------------------

导航到**Explore > Security Policies**.

单击**Create Security Policy > Secure an Application**.

填写以下字段，然后单击 **Next**:

- **Name** - Protect-app-<initials>, replacing <initials> with your initials.
- **Purpose** - Protect app-<initials> from ICMP outside of sales VMs.
- **Secure this app** - AppType: app-<initials>.
不要选中**Filter the app type by category**选项的复选框.

.. figure:: images/create_app_vm_sec_pol.png

在“Inbound rules”入站规则部分中，使用以下步骤允许传入流量：

- Leave **Whitelist Only** selected.
- Select **+ Add Source**.
- Leave **Add source by: Category** selected.
- Type **sales** and select **programs-<initials>:sales-<initials>**. Click Add.

完成上述步骤后，单击**AppType: app-<initials>**左侧显示的+。

这将打开创建“Inbound Rule”的窗口.

在“协议”列中，选择**ICMP**以允许此应用程序的入站ping请求，并将所有其他字段留空。点击**Save**。

在右侧，**Outbound**应设置为**Allow All**，对所有**All Destinations**放开访问权限。

.. figure:: images/create_inbound_rule.png

单击**Next**然后单击**Save and Monitor**.

确认属于**programs-abc：sales-abc**类别的VM可以ping属于**AppType：app-abc**类别的应用程序VM。

PC导航到**Explore> VMs**并打开以下三个VM的控制台窗口：

 - The designated AppType: app-<initials> VM, flow-<initials>-5.
 - The Sales VM (a VM in the programs-<initials>:sales-<initials> category, flow-<initials>-4).
 - The Dev VM (a VM in Environment: Dev, flow-<initials>-1).

从Sales VM (4)向AppType: app-<initials> VM (5)发送Ping指令.

此ping请求应该成功

从Dev VM（1）发送ping到AppType：app-abc VM（5）

即使Environment：Dev不是允许的策略的一部分，此ping也会成功。为什么？


Flow可视化
++++++++++++++++++

使用Flow可视化功能为Flow添加策略
..............................................

查看来自Environment：Dev的检测到的流量
-----------------------------------------------------

导航到**Explore > Security Policies > Protect-app-<initials>**，查看在**Environment: Dev**中检测到的流量；

确认**Environment：Dev**被列为**AppType：app-<initials>*的source源。这可能需要几分钟才能显示。

.. figure:: images/flow_viz.png

将鼠标悬停在黄色流线上，从**Environment：Dev**到**AppType：app-<initials>**，查看协议和连接信息。

单击黄色流线，查看更细致的连接尝试统计图。

.. figure:: images/network_flows.png


将检测到的流添加到安全策略
--------------------------------------------

选择右上角的**Update**以编辑策略。

单击**Next**并查看检测到的流量。

将鼠标悬停在入站列表中的**Environment：Dev**源上.

选中绿色复选框以将此源添加到入站允许列表

.. figure:: images/add_env_flow.png

选择确定以添加到规则

将鼠标悬停在蓝色**Environment: Dev**源上，然后选择铅笔图标以编辑规则。

点击** AppType：app-<initials>**上的铅笔来定义特定的端口和协议。

由于在上一个任务中检测到ping，因此当前已经允许使用ICMP协议。

选择**Save**以保存ICMP规则。

选择**Next**以查看对策略的更改。

将策略从**Monitoring**模式移至**Applied**模式
------------------------------------------------------------

既然策略已完成，请将其从监控模式移至应用模式。

选择**Apply Now**以保存策略并将其移至应用模式

导航到**Explore > Security Policies > Protect-app-<initials>**.

确认**Environment: Dev**以蓝色显示为允许的来源.

请尝试将来自其他来源，例如将**flow-<initials>-2**发送到**flow-<initials>-5**.

请问此流量是否会被阻止？ 

小贴士
+++++++++

- Microsegmentation是一个分散的安全框架，并内置包含在Prism Central内部。
- 它对数据中心提供额外的保护，防范来自在数据中心内部，从一台机器到另一台机器横向传播的恶意和安全威胁。
- 在群集中启用Microsegmentation后，可以通过Prism Central UI中创建的安全策略轻松保护VM。它们可用作标签，无需任何其他网络设置即可轻松应用于VM。
- 在本练习中，您已经使用Flow隔离策略的两种模式隔离环境中的VM，分别为strict（严格）和forensic（取证）模式。
- 取证模式是允许您研究进出VM的连接模式的关键字段，以便在隔离VM时确定允许或拒绝哪些连接。
- 在本练习中，您还可以轻松创建类别和隔离安全策略，而无需更改或更改任何网络配置。
- 使用创建的类别标记VM后，VM只根据其所属的策略执行操作。
- 您还创建了一个类别来保护特殊应用程序VM。然后，您创建了安全策略以将ICMP流量限制到该应用程序VM。
- 请注意，创建的策略处于**Save and Monitor**模式，这意味着在应用策略之前，流量实际上不会被阻止。这有助于研究连接并确保没有意外阻止真正的流量。
- 流可视化允许您可视化策略中发生的流。从那里开始编辑策略非常容易，以便添加或删除应该或不应该发生的流。
