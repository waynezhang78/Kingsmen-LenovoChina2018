.. _ssp:

自助门户网站
-------------------

概观
++++++++

  预计完成时间: **45分钟**

在本练习中，您将会用到通过Prism Central来配置自助服务门户(SSP),并为不同的用户组创建不同的项目。
该实验应该在进行Calm功能相关实验之前完成。

在Prism Central中设置身份验证和角色映射
++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. 注 ::

  如果您已在Prism Central中配置了** Authentication **，请跳过此部分。
  
在 **Prism Central** 中，点击:fa:`cog` **>认证**

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
| SSP Admins      | adminuser01-25        | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Developers  | devuser01-25          | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Power Users | poweruser01-25        | nutanix/4u                     |
+-----------------+-----------------------+--------------------------------+
| SSP Basic Users | basicuser01-25        | nutanix/4u                     |
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

输入 **SSP Admins**, and 点击 **Save**

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

In this exercise we will login into Prism Central as different users from different AD groups. Then we can compare what we see in SSP, and what we can do.

Lets Start by logging out of Prism Central

Use Self Service Portal as a SSP Admin
......................................

Log into Prism Central with the following credentials:

- **Username** - adminuserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp13.png

After you login you only have two tabs inthe top ribbon, **Explore** & **Apps**

You start on **VMs**, and should see all VMs the **adminuserXX** has access Tools

点击 on **Projects**, and you will see what Projects **adminuserXX** is a member of

  .. figure:: images/ssp14.png

Now lets add some images to a **Catalog**, 点击 on **Images**

  .. figure:: images/ssp15.png

Select the box for **Windows2012**, and 点击 **Add Image to Catalog** from the **Actions** dropdown

  .. figure:: images/ssp16.png

填写以下字段 and 点击 **Save**:

- **NAME** - Windows2012 Image
- **Description** - Windows2012 Image

  .. figure:: images/ssp17.png

Repeat these steps for the CentOS Image

点击 on **Catalog Items**, and you will see the two images you just added:

- CentOS Image
- Windows2012 Image

  .. figure:: images/ssp18.png

Use Self Service Portal as a Developer
......................................

Log into Prism Central with the following credentials:

- **Username** - devuserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp19.png

After you login you only have two tabs inthe top ribbon, **Explore** & **Apps**

You start on **VMs**, and should see all VMs the **devuserXX** has access Tools

点击 on **Projects**, and you will see what Projects **devuserXX** is a member of

  .. figure:: images/ssp20.png

点击 on **VMs**, then 点击 **Create VM**

Verify **Disk Images** is selected, and 点击 **Next**

  .. figure:: images/ssp21.png

Select **CentOS Image**, and 点击 **Next**

  .. figure:: images/ssp22.png

填写以下字段 and 点击 **Save**:

- **Name** - Developer VM 001
- **Target Project** - Developers
- **Disks** - Select **Boot From**
- **Network** - Select **Primary**
- **Advance Settings** - Check **Manually Configure CPU & Memory**
- **CPU** - 1 VCPU
- **Memory** - 2 GB

  .. figure:: images/ssp23.png

You should now see VM **Developer VM 001** listed

Lets see what happens when we log in as a user from a different group

Use Self Service Portal as a Power User
.......................................

Log into Prism Central with the following credentials:

- **Username** - poweruserXX@ntnxlab.local (replace XX with 01-05)
- **Password** - nutanix/4u

  .. figure:: images/ssp24.png

After you login you only have two tabs inthe top ribbon, **Explore** & **Apps**

You start on **VMs**, and should see all VMs the **poweruserXX** has access Tools

Notice you do not see **Developer VM 001**, that is because **SSP Power Users** is not a memeber of that project.

点击 **Create VM**

Verify **Disk Images** is selected, and 点击 **Next**

  .. figure:: images/ssp21.png

Select **CentOS Image**, and 点击 **Next**

  .. figure:: images/ssp22.png

填写以下字段 and 点击 **Save**:

- **Name** - Calm VM 001
- **Target Project** - Calm
- **Disks** - Select **Boot From**
- **Network** - Select **Primary**
- **Advance Settings** - Check **Manually Configure CPU & Memory**
- **CPU** - 1 VCPU
- **Memory** - 2 GB

  .. figure:: images/ssp25.png

You should now see VM **Calm VM 001** listed

Logout, and log back in as **devuserXX@ntnxlab.local**

You should see both **Developer VM 001** & **Calm VM 001**. That is because **SSP Developers** is a member of both **Projects**

  .. figure:: images/ssp26.png

点击 on **Projects**, and you will see the resource usage of **Developer VM 001** against the **Developer** project quota.

  .. figure:: images/ssp27.png

Takeaways
+++++++++++

- Nutanix provides a native service to seperate out resources for different groups, while giving them a Self-Service approach to using those resources.

- Easy to assign resources to different projects using directory groups

- Easy to assign a set of resources (quotas) to better manage cluster resources, or for show back
