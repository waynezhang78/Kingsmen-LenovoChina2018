.. _ssp:

-------------------
高级实验1：自助服务门户
-------------------

预计完成时间: 45分钟
本实验使用Prism Central
在浏览器中打开Prism Central链接，使用admin用户登录Prism Central界面

实验目的
++++++++

在本练习中，您将会用到通过Prism Central来配置自助服务门户(SSP),并为不同的用户组创建不同的项目。
该实验应该在进行Calm功能相关实验之前完成。

在Prism Central中设置身份验证和角色映射
++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. 注 ::

  如果您已在Prism Central中配置了** Authentication **，请跳过此部分。
  
在 **Prism Central** 中，点击 :fa:`cog` **>认证**

点击 **+ New Directory**

#需根据实际实验环境修改
填写以下字段，然后单击**Save**:

- **Directory Type** - AD活动目录
- **Name** - NTNXLAB
- **Domain** - ntnxlab.local
- **Directory URL** - ldaps://10.21.XX.40 
- **Service Account Name** - administrator@ntnxlab.local
- **Service Account Password** - nutanix/4u

  .. figure:: images/ssp01.png

点击黄色叹号，紧挨着**NTNXLAB**

  .. figure:: images/ssp28.png

点击**点击 Here** 进入到角色映射界面

点击**+ New Mapping**

填写以下字段，然后单击**Save**:

- **Directory** - NTNXLAB
- **LDAP Type** - user
- **Role** - Cluster Admin
- **Values** - administrator

  .. figure:: images/ssp29.png

关闭角色映射和身份验证窗口

配置自助服务门户
+++++++++++++++++++++++++++++

.. note::

  如果已经在Prism Central配置了 **SSP**，请跳过此部分

我们将使用以下用户信息

+-----------------+-----------------------+--------------------------------+
| **Group**       | **Usernames**         | **Password**                   |
+-----------------+-----------------------+--------------------------------+
| SSP Admins      | adminuser01-20        | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Developers  | devuser01-20          | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Power Users | poweruser01-20        | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Basic Users | basicuser01-20        | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+

在**Prism Central**中, 点击 :fa:`cog` **> Self-Service Admin Management**.

  .. figure:: images/ssp02.png

填写以下字段，然后单击 **Next**:

- **Domain** - ntnxlab.local
- **Username** - administrator@ntnxlab.local
- **Password** - nutanix/4u

  .. figure:: images/ssp03.png

点击 **+Add Admins**

  .. figure:: images/ssp04.png

输入 **SSP Admins**, 并点击 **Save**

  .. figure:: images/ssp05.png

点击 **Save**

  .. figure:: images/ssp06.png

创建项目
+++++++++++++++

在本练习中，我们将创建3个项目。每个项目都将设置为不同的Active Directory组权限。

在**Prism Central**中, 点击 **Explore**

点击 **Projects**

创建**Developers**项目
.............................

点击 **Create Project**

填写以下字段:

- **Project Name** - Developers
- **Description** - SSP Developers
- **AHV Cluster** - *Assigned HPOC*

在**Users, Groups, and Roles**右下方，点击蓝色 **+User**链接

填写以下字段并点击 **Save**:

- **NAME** - SSP Developers
- **ROLE** - Developer

  .. figure:: images/ssp08.png

 在**Network**中选择适合的网络，并设置为默认
 
  .. figure:: images/ssp09.png

在**Quotas**选项前打勾

填写以下字段:

- **VCPUS** - 10 VCPUs
- **Storage** - 200 GiB
- **Memory** - 40 GiB

确认所有字段配置填写完毕，然后点击 **Save**

  .. figure:: images/ssp10.png

创建**Power Users**项目
..............................

点击 **Create Project**

填写以下字段:

- **Project Name** - Power Users
- **Description** - SSP Power Users
- **AHV Cluster** - *Assigned HPOC*

在**Users, Groups, and Roles**右下方，点击 **+User** 

填写以下字段并点击 **Save**:

- **NAME** - SSP Power Users
- **ROLE** - Developer

在**Network**中选择适合的网络，并设置为默认

在**Quotas**选项前打勾

填写以下字段:

- **VCPUS** - 10 VCPUs
- **Storage** - 200 GiB
- **Memory** - 40 GiB

确认所有字段配置填写完毕，然后点击 **Save**

  .. figure:: images/ssp11.png

创建**Calm**项目（如需要选做Calm实验的话）
.......................

点击 **Create Project**

填写以下字段:

- **Project Name** - Calm
- **Description** - Calm
- **AHV Cluster** - *Assigned HPOC*

在**Users, Groups, and Roles**右下方，点击 **+User** 

填写以下字段并点击 **Save**:

- **NAME** - SSP Admins
- **ROLE** - Project Admin

填写以下字段并点击 **Save**:

- **NAME** - SSP Developers
- **ROLE** - Developer

填写以下字段并点击 **Save**:

- **NAME** - SSP Power Users
- **ROLE** - Consumer

填写以下字段并点击 **Save**:

- **NAME** - SSP Basic Users
- **ROLE** - Operator

在**Network**中选择适合的网络，并设置为默认

确认所有字段配置填写完毕，然后点击 **Save**

  .. figure:: images/ssp12.png

使用自助服务门户
+++++++++++++++++++++++

在本练习中，我们将以不同AD组的不同用户身份登录Prism Central。然后我们可以比较一下我们在SSP中看到的界面的区别，以及我们可以在不同权限下做什么操作。

我们先在Prism Central中登出现有管理员帐户

使用SSP Admin角色访问自助服务门户
......................................

使用以下凭据登录Prism Central：

- **Username** - adminuserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp13.png

登录后，在顶部功能区中只有两个选项卡， **Explore**和**Apps**

在**Explore**界面中点击查看**VMs**, 您应该能看到**adminuserXX**对所有VM拥有访问工具

点击**Projects**,您可以看到**adminuserXX**所属的所有项目列表

  .. figure:: images/ssp14.png

现在让我们在**Catalog**中增加一些镜像, 点击 **Images**

  .. figure:: images/ssp15.png

选择**Windows2012**, 然后在**Actions**下拉菜单中点击 **Add Image to Catalog**

  .. figure:: images/ssp16.png

填写以下字段并点击 **Save**:

- **NAME** - Windows2012 Image
- **Description** - Windows2012 Image

  .. figure:: images/ssp17.png

对CentOS映像重复这些步骤

点击**Catalog Items**, 您将看到刚刚添加的两个镜像文件：

- CentOS Image
- Windows2012 Image

  .. figure:: images/ssp18.png

使用Developer角色访问自助服务门户
......................................

使用以下凭据登录Prism Central：

- **Username** - devuserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp19.png

登录后，在顶部功能区中只有两个选项卡， **Explore**和**Apps**

在**Explore**界面中点击查看**VMs**, 您应该能看到**devuserXX**对所有VM拥有访问工具

点击**Projects**,您可以看到**devuserXX**所属的所有项目列表


  .. figure:: images/ssp20.png

点击**VMs**,然后点击 **Create VM**

确认勾选了**Disk Images**, 然后点击 **Next**

  .. figure:: images/ssp21.png

选择**CentOS Image**,并点击 **Next**

  .. figure:: images/ssp22.png

填写以下字段并点击 **Save**:

- **Name** - Developer VM 001
- **Target Project** - Developers
- **Disks** - Select **Boot From**
- **Network** - Select **Primary**
- **Advance Settings** - Check **Manually Configure CPU & Memory**
- **CPU** - 1 VCPU
- **Memory** - 2 GB

  .. figure:: images/ssp23.png

您应该可以看到在VM列表中存在**Developer VM 001**

让我们看看当我们以不同组的用户身份登录时会发生什么

使用Power User角色访问自助服务门户
.......................................

使用以下凭据登录Prism Central：

- **Username** - poweruserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp24.png

登录后，在顶部功能区中只有两个选项卡， **Explore**和**Apps**

在**Explore**界面中点击查看**VMs**, 您应该能看到**poweruserXX**对所有VM拥有访问工具

请注意，您无法看到** Developer VM 001 **，这是因为** SSP Power Users **不是该项目的成员。

点击 **Create VM**

确认已选中**Disk Images**, 并点击 **Next**

  .. figure:: images/ssp21.png

选择**CentOS Image**, 然后点击 **Next**

  .. figure:: images/ssp22.png

填写以下字段并点击 **Save**:

- **Name** - Calm VM 001
- **Target Project** - Calm
- **Disks** - Select **Boot From**
- **Network** - Select **Primary**
- **Advance Settings** - Check **Manually Configure CPU & Memory**
- **CPU** - 1 VCPU
- **Memory** - 2 GB

  .. figure:: images/ssp25.png

您应该可以看到在VM列表中存在**Calm VM 001**

登出，并用**devuserXX@ntnxlab.local**帐户重新登陆

您应该可以同时看到**Developer VM 001**和**Calm VM 001**两台虚拟机，这是因为**SSP Developers**帐户同时是两个项目的成员

  .. figure:: images/ssp26.png

单击** Projects **，您将看到** Developer VM 001 **的资源使用情况与** Developer **项目配额相对应。
  .. figure:: images/ssp27.png

小贴士
+++++++++++

-  Nutanix提供原生集成服务，为不同的群组分离资源，同时为他们提供使用这些资源的自助服务方法。

- 使用目录组轻松将资源分配给不同的项目

- 通过配额，可以轻松分配成组资源，以更好地管理群集资源或进行回收

