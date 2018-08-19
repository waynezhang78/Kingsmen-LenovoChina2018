.. _authentication:

-------------------------------
用户认证和角色映射
-------------------------------

在大多数情况下，用户都需要将Nutanix集群连接到公司的AD域或LDAP服务器中进行权限验证.

这将允许用户直接采用自己的帐号进行访问，而不需要使用Nutanix本地集群的默认管理员帐户。

.. note::

  以下步骤在PC或PE环境中，操作都完全适用。

在**Prism**, 点击齿轮**> Authentication**

点击**+ New Directory**

根据以下提示输入相关字段并点击**Save**:

- **Directory Type** - Active Directory
- **Name** - NTNXLAB
- **Domain** - ntnxlab.local
- **Directory URL** - ldaps://10.21.XX.40
- **Service Account Name** - administrator@ntnxlab.local
- **Service Account Password** - nutanix/4u

.. figure:: images/authentication_01.png

点击**NTNXLAB**旁边的黄色叹号

.. figure:: images/authentication_02.png

点击**Click Here**，进入到用户映射配置界面

点击**+ New Mapping**

根据以下提示输入相关字段并点击**Save**:

- **Directory** - NTNXLAB
- **LDAP Type** - user
- **Role** - Cluster Admin
- **Values** - administrator

.. figure:: images/authentication_03.png

关闭角色映射和认证窗口，完成配置。
