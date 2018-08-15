.. title:: Introduction to Nutanix AHV

.. toctree::
  :maxdepth: 2
  :caption: Technology Overview
  :name: _technology_overview
  :hidden:

  what_is_nutanix/what_is_nutanix
  nutanix_terminology/nutanix_terminology

.. toctree::
  :maxdepth: 2
  :caption: Nutanix Configuration Labs
  :name: _nutanix_configuration_labs
  :hidden:

  lab_nutanix_tech_overview/lab_nutanix_tech_overview
  lab_storage_configuration/lab_storage_configuration
  lab_network_configuration/lab_network_configuration

.. toctree::
  :maxdepth: 2
  :caption: Deploying and Managing Workloads
  :name: _deploying_and_managing_workloads
  :hidden:

  backup_and_dr/backup_and_dr
  lab_deploy_workloads/lab_deploy_workloads
  lab_manage_workloads/lab_manage_workloads
  lab_data_protection/lab_data_protection

.. toctree::
  :maxdepth: 2
  :caption: Monitoring and Managing the Environment
  :name: _monitoring_and_managing_the_environment
  :hidden:

  monitoring_and_managing_env/monitoring_and_managing_env
  lab_monitoring_env/lab_monitoring_env

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:

  authentication/authentication
  ssp/ssp
  calm/calm
  flow/flow


.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  appendix/basics

.. _前言:

---------------
前言
---------------

Welcome to the Nutanix Technology Bootcamp! This workbook accompanies an instructor-led session that introduces Nutanix technologies and many common management tasks. Each section has a lesson and an exercise to give you hands-on practice. The instructor explains the exercises and answers any additional questions that you may have.

At the end of the bootcamp, attendees should understand the basic concepts and technologies that make up the Nutanix Enterprise Cloud stack and should be well prepared for a hosted or onsite proof-of-concept (POC) engagement.

欢迎来到Nutanix Hands on Lab技术训练营！本实验手册将与实验讲师的指导相配合，重点对Nutanix技术和许多常见的管理任务进行演示和介绍。每个章节都有对应的知识点介绍和动手练习，为您提供实践练习。实验讲师将负责解释并回答您可能在动手实验中遇到的任何其他问题。

在训练营结束时，与会者应该可以对Nutanix企业云堆栈的基本概念和技术有所了解，并且能够有能力进行现场或利用远程环境进行POC或日常进行系统管理的能力。

更新日志
++++++++++

- 训练营的环境已经针对以下软件版本进行更新:
    - AOS 5.8
    - PC 5.7.1

- 可选部分的动手实验室更新:
    - Calm
    - Flow

实验目录
++++++

- 实验室环境介绍
- Nutanix技术概述
- 如何进行配置
- 如何部署和管理工作负载
- 如何监测和管理环境


实验环境细节
+++++++++++++++++++

Nutanix实验室将会在Nutanix HPOC或现场实验环境中运行，实验讲师将为您的群集配置完成练习所需的所有必要镜像，连接网络和VM。


网络环境（待更新）
..........

托管POC集群环境的通用命名规则:

- **群集名称** - POC\ *XYZ*
- **子网** - 10.**21**.\ *XYZ*\ .0
- **群集IP** - 10.**21**.\ *XYZ*\ .37

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
   * - 10.21.\ *XYZ*\ .37
     - Nutanix群集虚拟IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
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
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

Each cluster has a dedicated domain controller VM, **DC**, responsible for providing AD services for the **NTNXLAB.local** domain. The domain is populated with the following Users and Groups:
每个群集都有一个专用的域控制器VM，** DC **，负责为** NTNXLAB.local **域提供AD服务。该域包含以下用户和组：


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
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Power Users
     - poweruser01-poweruser25
     - nutanix/4u
   * - SSP Basic Users
     - basicuser01-basicuser25
     - nutanix/4u

访问说明
+++++++++++++++++++

可以通过多种不同方式访问Nutanix Hosted POC环境:

Citrix XenDesktop
.................

https://citrixready.nutanix.com - *Accessible via the Citrix Receiver client or HTML5*

**Nutanix Employees** - Use your NUTANIXDC credentials

**Non-Employees** - **Username:** POCxxx-User01 (up to POCxxx-User20), **Password:** *<Provided by Instructor>*

Employee Pulse Secure VPN
..........................

https://sslvpn.nutanix.com - Use your CORP credentials

Non-Employee Pulse Secure VPN
..............................

https://lab-vpn.nutanix.com - **Username:** POCxxx-User01 (up to POCxxx-User20), **Password:** *<Provided by Instructor>*

Under **Client Application Sessions**, click **Start** to the right of **Pulse Secure** to download the client.

Install and open **Pulse Secure**.

Add a connection:

- **Type** - Policy Secure (UAC) or Connection Server
- **Name** - HPOC VPN
- **Server URL** - lab-vpn.nutanix.com


