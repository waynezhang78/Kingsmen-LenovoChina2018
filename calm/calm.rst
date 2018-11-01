.. _calm:

----------------------------
高级实验3：Calm
----------------------------

概述
++++++++

.. 注意::

  在进行本实验之前请先通过查阅 :ref:`calm_basics` 来让你自己熟悉一下Nutanix Calm的界面和基础术语。

  估计完成时间: **170 分钟**

在实验的过程中，你将会启用Nutanix Calm，配置一个使用蓝图的项目，并通过蓝图进行应用的管理。

启用应用管理
+++++++++++++++++++++++

进行下面的操作来启用Nutanix Calm应用管理。

在浏览器中打开 \https://*<Prism-Central-IP>*:9440/ 并且登陆。

从导航菜单里选择  **Apps** 。

点击 **点击 here to enable Self Service and App management** 这段文字。

.. figure:: images/enable1.png

点击 **New Directory**。填写完以下字段后点击 **Next**:

- **Directory Type** - Active Directory
- **Name** - NTNXLAB
- **Domain** - ntnxlab.local
- **Directory URL** - ldaps://*<DC-VM-IP>*
- **Username** - Administrator@ntnxlab.local
- **Password** - nutanix/4u

.. figure:: images/enable2.png

点击 **+ Add Admins**. 并填写下面的字段:

- **Name** - SSP Admins
- **Default Cluster** - *<Cluster Name>*

在 **Network** 部分, 如果可能的话可以选择  **Primary** 和 **Secondary** 网络. 在 **Primary** 旁边点击  :fa:`star` ，这样就可以为**Calm** 项目的虚拟机设置了默认的虚拟网络。

点击 **Next**.

.. 注意::

  在启用自服务或者应用管理的过程中，你所配置的 Administrators, Default Cluster 和 Network，将成为 **Default** 项目的配置。在后续的章节里会配置新增的项目，用来提醒其它的配置方式。

.. figure:: images/enable3.png

选择 **Enable App Management** 并点击 **Finish**.

.. 注意:: Nutanix Calm 是需要单独的产品许可证的，该许可证可以与 Acropolis Starter, Pro, 或者Ultimate 版本搭配使用。每一个 Prism Central 实例能够免费管理 25 个虚拟机，超出的部分需要购买许可证。

.. figure:: images/enable4.png

下面是成功地完成了所有 **Enable App Management** 配置的屏幕，在导航菜单上选择 **Apps** 刷新后如下图所示.

.. figure:: images/enable5.png

.. 注意:: 首次启动 Calm 的服务可能需要 ~6 分钟左右. 如果 Calm 的界面并没有立刻地重新加载，等待一会之后再次刷新浏览器。

.. note about possibly needing to SSH into PC VM to do 'cluster start' if Epsilon service doesn't start on its own

创建一个项目
++++++++++++++++++

项目是一个逻辑的构造，它集成了Nutanix原生的自服务（SSP）功能，允许管理员为特定的蓝图和应用分配基础架构资源和用于访问该项目的AD中的用户/组。

在边栏中点击  |proj-icon| **Projects** 图标.

.. figure:: images/enable6.png

填写以下字段:

- **Project Name** - Calm
- **Description** - Calm

在 **Users, Groups, 和 Roles**部分, 点击 **+ User**.

填写以下字段并点击 **Save**:

- **Name** - SSP Admins
- **Role** - Project Admin

点击 **+ User**, 填写以下字段并点击 **Save**:

- **Name** - SSP Developers
- **Role** - Developer

点击 **+ User**, 填写以下字段并点击 **Save**:

- **Name** - SSP Power Users
- **Role** - Consumer

点击 **+ User**, 填写以下字段并点击 **Save**:

- **Name** - SSP Basic Users
- **Role** - Operator

在 **Infrastructure**部分, 填写以下字段:
- **Select which resources you want this project to consume** - Local only
- **AHV Cluster** - *<Cluster Name>*

在 **Network** 部分, 在可以的情况下配置 **Primary** 和 **Secondary** 网络. 通过点击网络右侧的 :fa:`star` 图标来为 **Calm** 项目中的虚拟机配置默认的 **Primary** 网络。

点击 **Save**.

.. figure:: images/enable7.png

.. 注意::

  点击 `here <https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v56:nuc-roles-responsibility-matrix-c.html>`_ 去查看完整的SSP默认角色和相关权限的说明文档.

创建蓝图 (MySQL)
++++++++++++++++++++++++++

在这个实验中，你将探索通过部署 Nutanix Calm 的蓝图来安装和配置一个独立的基于CentOS镜像的MySQL服务。

创建蓝图
..................

在 **Prism Central > Apps**的策略中选择 **Blueprints** ，并点击 **+ Create Application Blueprint**.

在**Blueprint Name**字段中，填写 **CalmIntro<INITIALS>** .
在描述字段中填写一个 **Description** .
从 **Project** 的下拉菜单中选择 **Calm** ，并点击 **Proceed**.

点击 **Proceed** 并继续.

点击 **Credentials >** :fa:`plus-circle` 并填写以下字段，然后点击 **Save**:

- **Credential Name** - CENTOS
- **Username** - root
- **Secret** - Password
- **Password** - nutanix/4u

点击 **Back**.

.. 注意::

  对每个蓝图而言 Credential 是唯一的.

  每个蓝图需要至少一个 Credential.

点击 **Save** 保存你的蓝图.

设置变量
.................

变量能使Blueprint具有扩展性, 基于所配置的变量，一个蓝图可以被用在不同的环境和目的。变量既可以作为静态的数值和蓝图保存在一起，也可以在**Runtime** 运行时被指定（当蓝图被启动了以后）。默认情况下，变量在配置页面中以明文的形式保存。配置一个变量为 **Secret** 时，它的指将被遮盖起来，适合用于存储密码。

变量可以在脚本执行的过程中以 **@@{variable_name}@@**的形式引用. 在把变量发送给虚拟机执行前，变量会被Calm扩展和替换成相应的数据。

在 **Configuration Pane** 中的 **Variable List**清单里, 填写以下字段:

+----------------------+------------------------------------------------------+------------+
| **Variable Name**    | **Value**                                            | **Secret** |
+----------------------+------------------------------------------------------+------------+
| Mysql\_user          | root                                                 |            |
+----------------------+------------------------------------------------------+------------+
| Mysql\_password      | nutanix/4u                                           | X          |
+----------------------+------------------------------------------------------+------------+
| Database\_name       | homestead                                            |            |
+----------------------+------------------------------------------------------+------------+
| App\_git\_link       | https://github.com/ideadevice/quickstart-basic.git   |            |
+----------------------+------------------------------------------------------+------------+

.. figure:: images/mysql1.png

点击 **Save**.

添加数据库服务
.................

In **Application Overview > Services**, 点击 :fa:`plus-circle`.

注意 **Service1** 显示在了**Workspace** 和 **Configuration Pane** 中，从而表示了一个所选择的配置。

填写以下字段:

- **Service Name** - MySQL
- **Name** - MySQLAHV

  .. 注意:: 在Calm中名称是一个基础的定义。名称中只能接受数字字母，空格和下划线。

- **Cloud** - Nutanix
- **OS** - Linux
- **VM Name** - MYSQL-@@{calm_array_index}@@-@@{calm_time}@@
- **Image** - CentOS
- **Device Type** - Disk
- **Device Bus** - SCSI
- 选择 **Bootable**
- **vCPUs** - 2
- **Cores per vCPU** - 1
- **Memory (GiB)** - 4
- 选择 :fa:`plus-circle` 在 **Network Adapters (NICs)** 下
- **NIC** - Primary
- **Credential** - CENTOS

.. 注意::

  在处理下一步之前，确保**Credential**中的配置已经最终选定了，选择其它的字段会清除掉 **Credential** 的选择.

在工作区窗口中选中 MySQL 服务的图标, 滚动到 **Configuration Panel**的顶端, 点击 **Package**.

填写以下字段:

- **Package Name** - MYSQL_PACKAGE
- **点击** - Configure install
- **点击** - + Task
- **Name Task** - Install_sql
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

复制并粘贴下面的脚本到 **Script** 字段:

.. code-block:: bash

  #!/bin/bash
  set -ex

  yum install -y "http://repo.mysql.com/mysql-community-release-el7.rpm"
  yum update -y
  yum install -y mysql-community-server.x86_64

  /bin/systemctl start mysqld

  #Mysql secure installation
  mysql -u root<<-EOF

  #UPDATE mysql.user SET Password=PASSWORD('@@{Mysql_password}@@') WHERE User='@@{Mysql_user}@@';
  DELETE FROM mysql.user WHERE User='@@{Mysql_user}@@' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
  DELETE FROM mysql.user WHERE User='';
  DELETE FROM mysql.db WHERE Db='test' OR Db='test\_%';

  FLUSH PRIVILEGES;
  EOF

  sudo yum install firewalld -y
  sudo service firewalld start
  sudo firewall-cmd --add-service=mysql --permanent
  sudo firewall-cmd --reload

  #mysql -u @@{Mysql_user}@@ -p@@{Mysql_password}@@ <<-EOF
  mysql -u @@{Mysql_user}@@ <<-EOF
  CREATE DATABASE @@{Database_name}@@;
  GRANT ALL PRIVILEGES ON homestead.* TO '@@{Database_name}@@'@'%' identified by 'secret';

  FLUSH PRIVILEGES;
  EOF

.. 注意::

  你能点击在脚本字段上的 **Pop Out** 图标来窗口放大后进行查看和编辑脚本。

  仔细查看这段脚本，你可以看到它会安装 MySQL 数据库，配置账号密码，基于之前所配置的变量创建一个数据库。

在工作区域中再次选中 MySQL 服务图标，滚动到 **Configuration Panel**的顶部, 点击 **Package**.

- **点击** - Configure Uninstall
- **点击** - + Task
- **Name Task** - Uninstall_sql
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

复制并粘贴下面的脚本到 **Script** 字段:

.. code-block:: bash

  #!/bin/bash
  echo "Goodbye!"

.. 注意:: 卸载脚本可以用于删除软件包、更新DHCP和DNS之类的网络服务，删除AD中额记录，等等。并不是像这个简单的例子这样。

点击 **Save**. 如果在任何字段中输入了非法/不可接受的字符，或者缺少字段，当你点击保存按钮的时候，你会受到出错的提示信息。

启动蓝图
.......................

从蓝图编辑器的工具栏顶端， 点击 **Launch**.

在 **Name of the Application** 字段中, 填写一个唯一的名称 (例如 CalmIntro*<INITIALS>*-1).

.. 注意::

  在Calm中的同一个环境中，一个蓝图可以多次启动，但是启动的每一个实例需要使用一个唯一的 **Application Name** .

点击 **Create**.

然后就会跳转到监控你的蓝图制备的 **Applications** 页面。

选中 **Audit > Create** 来查看你的应用的创建进度。在 **MySQLAHV - Check Login** 的任务完成了之后, 选择 **PackageInstallTask** 去查看安装脚本的实时输出。

注意，在蓝图被成功的制备了以后，应用的状态就变成了 **Running** 。

.. figure:: https://s3.amazonaws.com/s3.nutanixworkshops.com/calm/lab1/image25.png

Calm 蓝图 (LAMP)
+++++++++++++++++++++

在这个实验中，你将把之前创建的MySQL数据库蓝图扩展成为一个基本的LAMP堆栈，如下图所示，它的web层是可以扩展的。 

.. figure:: images/lamp1.png

创建Web服务器
.......................

从侧栏里 **Prism Central > Apps** 选择之前实验中你使用的 **Blueprints** .

在 **Application Overview > Services**, 点击 :fa:`plus-circle`.

注意 **Service1** 显示在了 **Workspace** 中，在 **Configuration Pane** 的部分显示了所选服务的配置信息。你可以通过点击和拖拽工作区里服务图标的方式重新布局这张蓝图。

在工作区的窗口里，保持Apache服务的图标处于选中状态，滚动到 **Configuration Panel** 的顶部， 点击 **Package**.

- **Service Name** - APACHE_PHP
- **Name** - APACHE_PHP_AHV
- **Cloud** - Nutanix
- **OS** - Linux
- **VM Name** - APACHE_PHP-@@{calm_array_index}@@-@@{calm_time}@@
- **Image** - CentOS
- **Device Type** - Disk
- **Device Bus** - SCSI
- 选择 **Bootable**
- **vCPUs** - 2
- **Cores per vCPU** - 1
- **Memory (GiB)** - 4
- 选择 :fa:`plus-circle` under **Network Adapters (NICs)**
- **NIC** - Primary
- **Credential** - CENTOS

滚动到 **Configuration Panel** 的顶部， 点击 **Package**.

再次点击 Apache 服务的图标并填写以下字段:

- **Package Name** - APACHE_PHP_PACKAGE
- **点击** - Configure install
- **点击** - + Task
- **Name Task** - Install_Apache
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS


在 **Script** 字段里复制并粘贴以下脚本:

.. code-block:: bash

  #!/bin/bash
  set -ex
  # -*- Install httpd and php
  sudo yum update -y
  sudo yum -y install epel-release
  sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
  sudo yum install -y httpd php56w php56w-mysql

  echo "<IfModule mod_dir.c>
          DirectoryIndex index.php index.html index.cgi index.pl index.php index.xhtml index.htm
  </IfModule>" | sudo tee /etc/httpd/conf.modules.d/dir.conf

  echo "<?php
  phpinfo();
  ?>" | sudo tee /var/www/html/info.php
  sudo systemctl restart httpd
  sudo systemctl enable httpd

.. code-block:: bash

在工作区里再次选中 Apache 服务的图标并滚动到 **Configuration Panel** 的顶部, 点击 **Package**.

填写以下字段:

- **点击** - Configure uninstall
- **点击** - + Task
- **Name Task** - Uninstall_apache
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

在 **Script** 字段里复制并粘贴以下脚本:

.. code-block:: bash

  #!/bin/bash
  echo "Goodbye!"

.. code-block:: bash

点击 **Save**.

添加依赖关系
...................

由于 web 服务器的应用在启动之前需要数据库先运行起来，这需要在蓝图中加入这种相互依赖的顺序关系。

在 **Workspace** 工作区中，选中 **APACHE_PHP** 服务并点击位于服务图标之上的 **Create Dependency** 图标.

选中 **MySQL** 服务. 这会保证 **APACHE_PHP** 的安装脚本的执行，在 **MySQL** 服务运行了以后。

点击 **Save**.

添加副本数
...............

通过Calm为一个特定的服务扩容多个虚拟机是很简单的，这对横向扩容web服务这样的需求很有帮助。

在工作区 **Workspace**中, 选中 **APACHE_PHP** 服务.

在 **Configuration Pane** 中, 选中 **Service**  标签.

在 **Deployment Config** 的下面, 将 **Max** 副本最大数从 1 改为 2.

创建负载均衡
..........................

为了实现横向扩容 web 层应用服务的效果，我们需要将接入负载的连接分发到web服务器的虚拟机上。HAProxy是一个免费、开源的 TCP/HTTP 负载均衡器，能用于工作负载流量的分发。它不仅能用于小型的简单环境，还能用于大规模互联网规模的公司如 GitHub, Instagram 和 Twitter.

在 **Application Overview > Services** 菜单, 点击 :fa:`plus-circle`.

选中 **Service1** 图标并在 **Configuration Pane** 填写以下字段 :

- **Service Name** - HAProxy
- **Name** - HAPROXYAHV
- **Cloud** - Nutanix
- **OS** - Linux
- **VM Name** - HAProxy
- **Image** - CentOS
- **Device Type** - Disk
- **Device Bus** - SCSI
- Select **Bootable**
- **vCPUs** - 2
- **Cores per vCPU** - 1
- **Memory (GiB)** - 4
- Select :fa:`plus-circle` under **Network Adapters (NICs)**
- **NIC** - Primary
- **Credential** - CENTOS

滚动 **Configuration Panel** 到顶部, 点击 **Package**.

填写以下字段:

- **Package Name** - HAPROXY_PACKAGE
- **点击** - Configure install
- **点击** - + Task
- **Name Task** - install_haproxy
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

在 **Script** 字段中复制并粘贴以下代码:

.. code-block:: bash

  #!/bin/bash
  set -ex

  sudo setenforce 0
  sudo sed -i 's/permissive/disabled/' /etc/sysconfig/selinux

  port=80
  sudo yum update -y
  sudo yum install -y haproxy

  echo "global
    log 127.0.0.1 local0
    log 127.0.0.1 local1 notice
    maxconn 4096
    quiet
    user haproxy
    group haproxy
  defaults
    log     global
    mode    http
    retries 3
    timeout client 50s
    timeout connect 5s
    timeout server 50s
    option dontlognull
    option httplog
    option redispatch
    balance  roundrobin
  # Set up application listeners here.
  listen stats 0.0.0.0:8080
    mode http
    log global
    stats enable
    stats hide-version
    stats refresh 30s
    stats show-node
    stats uri /stats
  listen admin
    bind 127.0.0.1:22002
    mode http
    stats uri /
  frontend http
    maxconn 2000
    bind 0.0.0.0:80
    default_backend servers-http
  backend servers-http" | sudo tee /etc/haproxy/haproxy.cfg

  sudo sed -i 's/server host-/#server host-/g' /etc/haproxy/haproxy.cfg

  hosts=$(echo "@@{APACHE_PHP.address}@@" | sed 's/^,//' | sed 's/,$//' | tr "," "\n")

  for host in $hosts
  do
     echo "  server host-${host} ${host}:${port} weight 1 maxconn 100 check" | sudo tee -a /etc/haproxy/haproxy.cfg
  done

  sudo systemctl daemon-reload
  sudo systemctl enable haproxy
  sudo systemctl restart haproxy

.. code-block:: bash

在工作区窗口中再次选中 HAProxy 服务图标，并且滚动到 **Configuration Panel** 的顶部, 点击 **Package**.

填写以下字段:

- **点击** - Configure install
- **点击** - + Task
- **Name Task** - uninstall_haproxy
- **Type** - Execute
- **Script Type** - Shell
- **Credential** - CENTOS

在 **Script** 字段中复制并粘贴以下代码:

.. code-block:: bash

  #!/bin/bash
  echo "Goodbye!"

.. code-block:: bash

点击 **Save**.

在 **Workspace**工作区中, 选中 **HAProxy** 服务图标并在服务图标的上方点击 **Create Dependency** 图标.

选中 **Apache_PHP** 服务。这会确保 **HAProxy** 的安装脚本的执行，只会发生在 **APACHE_PHP** 服务运行了之后.

点击 **Save**.

点击 **Launch**. 填写一个全局唯一的 **Application Name**  (例如 CalmIntro*<INITIALS>*-2) 然后点击 **Create**.

Calm应用市场--第一部分
+++++++++++++++++++++++

在这个练习中，你将学习如何在 Nutanix Marketplace中管理Calm蓝图。作为本练习的一部分，你将发布一个预配置的蓝图到本地市场中，从市场中克隆并编辑一个蓝图，然后启动这个应用。

从Marketplace Manager发布蓝图
..............................................

默认情况下，Calm自带了一些预定义的、经过验证的开源和企业应用蓝图。Marketplace Manager是默认自带的和用户自开发应用的存储区域，用作本地的应用商店。应用商店可以保存各种应用，用户可以查询到能部署的应用。

从 **Prism Central > Apps**中, 的侧栏里选中 |mktmgr-icon| **Marketplace Manager** 图标.

在 **Marketplace Blueprints**中, 选中 **Mongo**.

注意-蓝图描述中一般会包含关于许可证、硬件需求，支持的操作系统平台和限制等重要的信息. 点击 **Publish**.

.. figure:: images/marketplace_p1_1.png

等待蓝图的 **Status** 状态显示为 **Published**.

.. figure:: images/marketplace_p1_2.png

在 **Projects Shared With** 之下, 选中 **Calm** 项目并点击 **Apply**.

.. figure:: images/marketplace_p1_3.png

.. 注意::

  如果 **Projects Shared With** 的下拉菜单是不可选择的，请刷新你的浏览器。

从Marketplace中克隆蓝图
...................................

选择 **Prism Central > Apps** , 从侧栏里选中 |mkt-icon| **Marketplace** . 所有已经发布的蓝图都可以在 Marketplace Manager 的这个部分看到.

.. figure:: images/marketplace_p1_4.png

选中 **Mongo** 蓝图并点击 **Clone**.

.. 注意::

  蓝图在 **Actions Included** 中将显示所有该蓝图中所实施的可以执行的动作，例如创建、启动、停止、删除、更新、扩容、缩容等等。

.. figure:: images/marketplace_p1_5.png

填写以下字段并点击 **Clone**:

- **Blueprint Name** - MongoDB*<INITIALS>*
- **Project** - Calm

编辑克隆的蓝图
........................

在侧栏中选择 |bp-icon| **Blueprints** 并点击你的 **MongoDB<INITIALS>** 蓝图来打开蓝图编辑器.

.. figure:: images/marketplace_p1_6.png

点击 :fa:`exclamation-circle` 来查看错误提示信息，这些错误会阻止蓝图的成功部署。

.. figure:: images/marketplace_p1_7.png

点击 **Credentials** 并选择 **CENTOS (Default)**.

填写以下字段并点击 **Back**:

- **Username** - root
- **Secret** - Password
- **Password** - nutanix/4u

选择 **Mongo_ConfigSet** 服务，在 **Configuration Pane**中进行如下配置:

- 更新 **VM Configuration > Image** 为 **CentOS**.
- 更新 the **Network Adapters > NIC** 为 **Primary**.
- 更新 the **Connection > Credential** 为 **CENTOS**.

对 **Mongo_Router** 和 **Mongo_ReplicaSet** 服务进行上述的相同操作.

点击 **Save**.

点击 **Launch**. 指定一个唯一的 **Application Name** (例如 MongoDB*<INITIALS>*-1) 并点击 **Create**.

.. figure:: images/marketplace_p1_8.png

Calm应用市场第二部分
+++++++++++++++++++++++

.. 注意::

  这个实验的前提条件是你在上一个实验中做了一个可用蓝图。
在这个练习中，你将会学习如何在Nutanix Marketplace中管理Calm蓝图。在练习中你将通过蓝图编辑器发布一个蓝图，使用 Marketplace Manager 来审批，分配角色和项目，并发布到应用市场。最后你会编辑一个项目环境，这样你的蓝图就能够从应用市场中直接启动。


发布蓝图
.....................

选择 **Prism Central > Apps**, 在侧栏中选中 |bp-icon| **Blueprints** .

通过对点击任一 **Active** 蓝图的 **Name**来打开它.

.. figure:: images/marketplace_p2_1.png

点击 **Publish**.

.. figure:: images/marketplace_p2_2.png

提供如下细节:

- **Name** (例如 Blueprint Name *<INITIALS>*)
- **Publish as a** - New Marketplace blueprint
- **Initial Version** - 1.0.0
- **Description** - Finished MySQL app

点击 **Submit for Approval**.

.. figure:: images/marketplace_p2_3.png

批准蓝图
....................

选择 **Prism Central > Apps**, 在侧栏中选择 |mktmgr-icon| **Marketplace Manager**.

.. 注意:: 你登陆Marketplace Manager的访问权限必须是 Cluster Admin 用户.

注意你的蓝图还没有显示在 **Marketplace Items** 清单中.

选择 **Approval Pending** 标签.

.. figure:: images/marketplace_p2_4.png

选择你 **Pending** 状态的蓝图.

.. figure:: images/marketplace_p2_5.png

查看所有可以操作的选项:

- **Reject** - 阻止蓝图在应用市场中被启动或者发布。在这个蓝图能被发布以前，这个而蓝图必须被再次提交。
- **Approve** - 批准蓝图发布到应用市场。
- **Launch** - 启动一个应用的蓝图，与在蓝图编辑器中启动类似。

点击 **Approve**.

一旦应用被成功地批准之后，就可以配置合适的 **Category** 和 **Project Shared With**. 点击 **Apply**.

.. figure:: images/marketplace_p2_6.png

选中 **Marketplace Blueprints** 标签并且选中你的蓝图。 点击 **Publish**.

校验蓝图的状态 **Status** 现在已经显示为 **Published**.

.. figure:: images/marketplace_p2_7.png

选择 **Prism Central > Apps**, 从侧栏中选择 |mkt-icon| **Marketplace** . 校验你的蓝图已经成为了一个可以启动的应用。

.. figure:: images/marketplace_p2_8.png

配置项目环境
...............................

为了从应用市场中直接启动一个蓝图，我们需要确保你的环境与蓝图所需的所有细节需求相匹配。

选择 **Prism Central > Apps**, 从侧栏中选择 |proj-icon| **Projects** .

选中你所用的发布名称 **Name**  (例如  **Calm** 项目之前被分配的 **Project Shared With**).

.. figure:: images/marketplace_p2_9.png

选择 **Environment** 标签.

在 **Credential**下, 点击 :fa:`plus-circle` 来增加新的账户信息.

填写以下字段:

- **Credential Name** - CENTOS
- **Username** - root
- **Secret** - Password
- **Password** - nutanix/4u
- 选择 **Use as default**


在 **VM Configuration**下

- 选择 **AHV on NUTANIX**.
- **VM Name** - add prefix "default" to the name
- **Image** - CentOS
- **vCPUs** - 2
- **Cores per vCPU** - 1
- **Memory** - 4GiB

.. figure:: images/marketplace_p2_010.png


在 **Network Adapters (NICs)**下, 点击 :fa:`plus-circle` 并选择 **Primary**.

.. figure:: images/marketplace_p2_10.png

点击 **Save**.

从Marketplace启动蓝图
........................................

选择 **Prism Central > Apps**, 从侧栏中选择 |mkt-icon| **Marketplace** .

.. figure:: images/marketplace_p2_11.png

选择在实验中所发布的蓝图并点击 **Launch**.

.. figure:: images/marketplace_p2_12.png

选择 **Calm** 项目并点击 **Launch**.

.. figure:: images/marketplace_p2_13.png

指定一个唯一的 **Application Name** (例如. Marketplace*<INITIALS>*) 并点击 **Create**.

.. 注意::

  为了查看预配置的 **Environment** 细节, 展开 **VM Configurations** 部分.

.. figure:: images/marketplace_p2_14.png

监控蓝图的整个配置过程直到完成为止。

.. figure:: images/marketplace_p2_15.png

关键要点
+++++++++

- Nutanix Calm 是一个Nutanix堆栈中完全集成化的组建。它容易启用，从 Prism Central中可以横向扩展的，并具有高可靠性，并且支持一键式的而升级和补丁，并且不会中断服务。
- 通过为不同的项目配置不同的群集和用户、管理员，可以保证每次工作负载都被正确的部署。例如，一个开发人员可以被项目管理员分配到一个开发/测试项目中，这样他们就能够集群和云的环境中具有部署应用的全部控制力，可是对于生产环境只有只读的权限，能够访问生产环境的日志，而不能修改生产环境的工作负载。
- 蓝图编辑器为编辑和建模复杂的应用系统提供了简洁的图形界面。
- 蓝图是和SSP项目紧密关联在一起的，这样就具有了配额管理和基于角色的访问控制能力。
- 使用蓝图安装和配置二进制应用意味着再也不用为某个应用制作特殊的镜像文件。应用反而可以通过修改蓝图或者安装脚本来修改，这些都别保存在了源代码库里。
- 变量提供了多个定制化应用的维度，而不用修改底层的蓝图。
- 应用的状态能够被实时监控。
- 应用通常由多个虚拟机组成，每个虚拟机负责不同的服务。Calm具有自动化和编排全套应用的能力。
- 服务之间的依赖关系能在蓝图编辑器中容易的编辑。
- 用户能在生产环境中部署全套的应用堆栈，或者重复的得到测试结果，而并不需要大量的手工配置工作。
- 通过使用Nutanix Marketplace中内置的蓝图，用户可以快速地尝试新的应用。
- 应用市场的蓝图能够被克隆和修改，从而满足用户的需求。例如，内置的LAMP蓝图可以是开发者的一个起点，能容易地把PHP应用换成Go应用服务器。
- 应用市场朗图能使用本地的磁盘镜像或者自动地下载相关的磁盘镜像。用户可以创建自己的密钥，将密钥封装到蓝图中（cloud-init）。
- 开发人员能发布蓝图到应用市场中，用户从而能够容易的被快速使用。
- 用户可以在不需要进行多余配置的情况下从应用市场中启动蓝图，为最终用户交付了一种类似与共有云中的SaaS服务的使用体验。
- 管理员能控制发布到市场中的蓝图，不同的项目能访问不同的蓝图。

.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |proj-icon| image:: ../images/projects_icon.png
