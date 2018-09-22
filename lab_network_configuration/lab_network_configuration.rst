.. _lab_network_configuration:

------------------------------
初级实验3:网络配置
------------------------------
预计完成时间: 10分钟
本实验使用Prism Central

实验目的
++++++++

了解如何使用Prism在集群中设置网络：通过如下步骤可以为VM各自的NIC分配适当的网络，从而为VM提供网络接入能力。

AHV网络背景知识
+++++++++++++++++++++++++

AHV可有效简化网络拓扑，通常情况下，节点可以连接一个Trunked的VLAN环境，从而允许多个VM网络可以在环境中共存。 

通过AHV, 您还可以通过DHCP服务器，通过IPAM服务，自动为VM分配IP地址.

虚拟网络
................

- 类似于“分布式端口组“
- 每个虚拟NIC只属于一个虚拟网络
- 每个虚拟网络是一组虚拟NIC的集合
- 物理交换机端口必须配置为中继VLAN

.. figure:: images/network_config_01.png

虚拟网卡
............

- 每个vNIC只属于一个虚拟网络
- 对于支持IPAM的网络，vNIC可获得永久的静态IP分配
- 用户可以将网络池配置为自动分配IP，或手动指定IP

.. figure:: images/network_config_02.png

IP地址管理（IPAM）
............................

- 内置集成DHCP服务器
- AHV会拦截来自IPAM网络上的访客的DHCP请求，并直接回复响应
- 由虚拟化管理员设定并管理一系列IP地址
- 支持任意DHCP选项，并为DNS和TFTP提供UI界面支持

.. figure:: images/network_config_03.png

配置网络
+++++++++++++++++

在本练习中，我们故意使用无效的VLAN，因此无法与现有网络上的VM进行直接通信。

.. note::

  此练习仅用于演示目的，我们会让VM连接到vlan 0以外的网络，VM通过DHCP获取IP，但由于网络无效，不会传输任何流量。

设置用户VM网络
.....................

连接到Prism Central并为用户VM的网络接口创建网络，使用0以外的任何VLAN，不要启用IP地址管理。

在**Prism Central > Explore**界面, 点击**VMs**,并点击**Network Config**

然后点击**VM Networks**, 选择**+ Create Network**.

以下字段根据提示填写并点击**Save**:

- **Name** - Network-*intials*
- **VLAN ID** - Something other than 0
- **Enable IP Address Management** - unchecked

填写完成后的结果会如下图所示.

.. figure:: images/network_config_04.png

使用IPAM设置用户VM网络
...............................


创建另一个网络，但这次启用IPAM。

以下字段根据提示填写并点击 **Save**:

- **Name** - Network_IPAM-*intials*
- **VLAN ID** - Something other than 0
- **Enable IP Address Management** - Checked
- **Network IP Address / Prefix Length** - 10.0.0.0/24
- **Gateway** - 10.0.0.1
- **Configure Domain Settings** - unchecked
- **Create Pool** - 10.0.0.100-10.0.0.150
- **Override DHCP Server** - unchecked

.. note::

   可以支持为网络同时创建多个池范围
   
小贴士
+++++++++

- 在群集中设置网络并建立VM连接非常容易.
- 在网络中设置IPAM非常简单，它可以极大地简化集群内的IP管理.
