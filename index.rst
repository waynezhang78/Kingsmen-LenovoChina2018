.. title:: Introduction to Nutanix AHV

.. toctree::
  :maxdepth: 2
  :caption: Nutanix技术概览
  :name: _technology_overview
  :hidden:

  what_is_nutanix/what_is_nutanix
  nutanix_terminology/nutanix_terminology

.. toctree::
  :maxdepth: 2
  :caption: 初级实验-初始配置
  :name: _nutanix_configuration_labs
  :hidden:

  lab_nutanix_tech_overview/lab_nutanix_tech_overview
  lab_storage_configuration/lab_storage_configuration
  lab_network_configuration/lab_network_configuration

.. toctree::
  :maxdepth: 2
  :caption: 初级实验-AHV部署与管理
  :name: _deploying_and_managing_workloads
  :hidden:

  lab_deploy_workloads/lab_deploy_workloads
  lab_manage_workloads/lab_manage_workloads
  
.. toctree::
  :maxdepth: 2
  :caption: 中级实验-数据保护与恢复
  :name: _monitoring_and_managing_the_environment
  :hidden:
  lab_data_protection/lab_data_protection
  backup_and_dr/backup_and_dr
  
.. toctree::
  :maxdepth: 2
  :caption: 中级实验-集中监控和管理
  :name: _monitoring_and_managing_the_environment
  :hidden:

  monitoring_and_managing_env/monitoring_and_managing_env
  lab_monitoring_env/lab_monitoring_env
  

.. toctree::
  :maxdepth: 2
  :caption: 高级实验
  :name: _optional_labs
  :hidden:

  ssp/ssp
  flow/flow
  calm/calm
  

.. toctree::
  :maxdepth: 2
  :caption: 附录
  :name: _appendix
  :hidden:

  appendix/glossary
  appendix/basics

.. _前言:

---------------
前言
---------------

欢迎来到Nutanix Hands on Lab技术训练营！本实验手册将与实验讲师的指导相配合，重点对Nutanix技术和许多常见的管理任务进行演示和介绍。每个章节都有对应的知识点介绍和动手练习，为您提供实践练习。实验讲师将负责解释并回答您可能在动手实验中遇到的任何其他问题。

在训练营结束时，与会者应该可以对Nutanix企业云堆栈的基本概念和技术有所了解，并且能够有能力进行现场或利用远程环境进行POC或日常进行系统管理的能力。

更新日志
++++++++++

- 动手实验室环境已经针对以下软件版本进行更新:
    - AOS 5.8
    - PC 5.8.1

- 高级实验部分的动手实验室更新:
    - SSP自助服务门户
    - Flow软件定义网络


实验目录
++++++

-
初级实验1:NUTANIX管理界面介绍 
初级实验2:存储容器配置  
初级实验3:网络配置  
初级实验4:AHV的虚机部署与管理 
中级实验5:数据保护与恢复 
中级实验6:监控与管理 
高级实验7:自助服务门户
高级实验8：FLOW网络虚拟化



实验环境细节
+++++++++++++++++++

Nutanix实验室将会在Nutanix HPOC或现场实验环境中运行，实验讲师将为您的群集配置完成练习所需的所有必要镜像，连接网络和VM。


网络环境
..........

托管POC集群环境的通用命名规则:

- **群集名称** - POC\*XYZ*
- **子网** - 10.**21**.\*XYZ*\ .0
- **群集IP** - 10.**21**.\*XYZ*\ .37

例如:

- **群集名称** - POC055
- **子网** - 10.21.55.0
- **群集IP** - 10.21.55.37

在动手实验中，有多个实验场景需要用*XYZ*替换子网的正确八位字节:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP地址
     - 说明
   * - 10.21.\ **XYZ**\ .37
     - Nutanix群集虚拟IP
   * - 10.21.\ **XYZ**\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ **XYZ**\ .40
     - **DC** VM IP, NTNXLAB.local 域控制器

每个群集配置有2个VLAN，可用于VM:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
      - VLAN
      - DHCP Scope
  * - 主地址
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - 次地址
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

密码
...........

.. 注意::

  *<Cluster Password>* 对每个群集都是唯一的，将由Workshop的负责人提供

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - 密码
     - Username
     - Password
   * - Prism Element
     - admin
     - *nx2Tech308!*
   * - Prism Central
     - admin
     - *nx2Tech308!*
   * - Controller VM
     - nutanix
     - *nx2Tech308!*
   * - Prism Central VM
     - nutanix
     - *nx2Tech308!*

每个群集都有一个专用的域控制器VM，**DC**，负责为 **NTNXLAB.local** 域提供AD服务。该域包含以下用户和组：


.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser32
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser32
     - nutanix/4u
   * - SSP Power Users
     - poweruser01-poweruser32
     - nutanix/4u
   * - SSP Basic Users
     - basicuser01-basicuser32
     - nutanix/4u

访问说明
+++++++++++++++++++

可以通过多种不同方式访问Nutanix Hosted POC环境:

1)Citrix XenDesktop桌面(推荐)
.................

https://citrixready.nutanix.com - *Accessible via the Citrix Receiver client or HTML5*

**Nutanix内部员工** - 使用Nutanix公司SSO域帐户
**非Nutanix员工** - **用户名:** POCxxx-User01 (up to POCxxx-User20), **密码:** *<培训讲师提供>*

2)Employee Pulse Secure VPN
..........................

https://sslvpn.nutanix.com - 使用Nutanix域帐户登陆

Non-Employee Pulse Secure VPN
..............................

https://lab-vpn.nutanix.com - **用户名:** POCxxx-User01 (up to POCxxx-User20), **密码:** *<培训讲师提供>*

在**Client Application Sessions**界面, 找到右下角的 **Pulse Secure** 图标右侧的**Start** 按钮，下载客户端。

下载并安装**Pulse Secure**客户端软件.

按照如下信息添加一个连接:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - HPOC VPN
- **Server URL** - lab-vpn.nutanix.com


