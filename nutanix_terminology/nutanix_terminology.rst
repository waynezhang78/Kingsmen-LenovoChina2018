.. _nutanix_terminology:

-------------------
Nutanix术语
-------------------

Nutanix HCI
+++++++++++

物理基础设施
.......................

Nutanix集群由节点(Nodes)和块(Blocks)组成

.. figure:: images/nutanix_terminology_01.png

存储池和容器
............................

utanix将物理磁盘显示为一个**存储池**，可以分为一个或多个**容器**

.. figure:: images/nutanix_terminology_02.png

Nutanix存储
+++++++++++++++

可调冗余因子
..................

它是什么？

- 可以灵活地为不同级别的应用程序配置不同级别的冗余度
- 与EC-X纠删码配合使用可节省容量

主要区别:

- 纯软件定义，无需掌握存储相关知识技能；
- RF-3可以容忍两块磁盘，双节点或双网卡同时故障的场景而不影响业务；
- 可支持RF-2和RF-3在线切换
- 可基于容器级别进行数据集的复制

.. figure:: images/nutanix_terminology_03.png

Nutanix EC-X (Erasure Coding)
.............................

- 数据写入路径无需额外资源损耗
- 底层任务
- 只有冷数据集时采用EC-X
- 更低的数据重建时间（等于或由于RF-2）
- 专利算法（申请中）

.. figure:: images/nutanix_terminology_04.png

重复数据删除
.............

- 在线指纹识别，近线数据去重
- 真正的分布式-完全分布在所有节点
- 集群内全局去重
- SHA-1指纹识别已卸载到英特尔处理器，以提高效率
- 100%软件定义
- 强哈希允许基于元数据匹配进行重复数据删除

.. figure:: images/nutanix_terminology_05.png

压缩
...........

- 在线和近线压缩
- 在线: 数据在写入时压缩
- MapReduce: 在冷数据迁移至低性能存储层后进行压缩
- 对正常IO路径没有影响
- 适用于随机批处理工作负载
- 采用LZH4c压缩算法 (AOS 5+)

数据本地化
.............

- 将数据保存在与VM相同的节点上
- 所有读取操作都位于同一节点上
- ILM transparently moves remote data to local controller
- Reduces network chattiness significantly
- Data follows VM during vMotion/Live Migration

.. figure:: images/nutanix_terminology_06.png

智能分层
...................

自动性能优化

 - 利用多层存储资源
 - 持续监控数据访问模式
 - 最佳地放置数据以获得最佳性能
 - 无需用户干预

热数据 -  SSD

 - 随机数据
 - 持久层
 - 最高性能
 
冷数据 - 硬盘

 - 顺序数据
 - 最大容量
 - 最经济

.. figure:: images/nutanix_terminology_07.png

CVM自动路径
................

- 在CVM故障时仍然能够持续进行数据访问
- 自动更新Hypervisor的路由，以连接其它正在运行的CVM

优势

- 在软件升级和故障期间的保持高可用性

.. figure:: images/nutanix_terminology_10.png

vMotion/DRS or Live Migration
.............................

无缝VM迁移

- 元数据服务可以从任何地方访问数据
- 数据本地化随着时间推移持续改善

.. figure:: images/nutanix_terminology_11.png

Nutanix VM Mobility
+++++++++++++++++++++++++++++++

Acropolis Dynamic Scheduling (ADS)
..................................

- 自动检测，修复CPU和存储热点
- 自动判断VM初始的摆放位置
- 检测异常:
    - CPU热点
    - 存储控制器热点
    - 不符合亲和性规则
- 如果发现异常,会通过以下方式重新调节:
    - 虚拟机的实时迁移
    - ABS iSCSI会话重定向

.. figure:: images/nutanix_terminology_12.png

主机高可用性
......................

- 主机故障后自动重启用户VM
- 直接通过Prism进行政策设定
- AHV选择最佳的admission control policy:
    - Reserved segments (default)
    - Reserved host

优点

- 永远在线的虚拟机
- 减少管理开销

.. figure:: images/nutanix_terminology_13.png

亲和规则 - Host
.....................

VM主机亲和力:

- 放置并始终在选定的主机组上保持已启动的VM

用例:

- 软件许可证合规
- 安全/治理
- 硬件分段

“必须”规则 - 不能违反:

- 初始数据摆放
- 高可用
- 主机维护模式
- ADS
- 手动在线迁移

.. figure:: images/nutanix_terminology_14.png

亲和规则 - VM
...................

VM-VM反关联:

- 摆放并始终将一组已打开电源的VM保留在不同的主机上

用例:

- VM HA故障域分离（例如SQL群集)
- 手动规避热点

“尽量”规则 – 尽最大努力，但可以违反

.. figure:: images/nutanix_terminology_15.png

Nutanix网络
++++++++++++++++++

AHV  - 软件定义网络
.................................

基于开放标准的完全分布式网络简化了部署并确保了配置一致性。

- 基于Open vSwitch
 - 完全分发到所有节点
 - 具有vLAN的虚拟网络
 - IP地址管理（DHCP）
 - Bond/Link Aggregation (端口绑定/链路聚合)
    - Active / Backup
    - Source-NIC Load Balancing
    - LACP
- 每个网桥都有一个绑定，由一个或多个上行链路支持

.. figure:: images/nutanix_terminology_16.png

Flow (网络微分段)
........................

在数据中心内部恢复安全可控和洞察分析的能力

- AHV网络原生功能，无需安装任何组件
- 通过Prism Central中的类别进行逻辑分组
    - 可根据VM或应用进行分组
- 安全策略映射到类别
    - 通过对Category成员进行管理简化流程
- 规则推送路径：PC -> CVM -> AHV -> OVS
    - AHV中的OVS执行规则
- 逻辑层面，规则在在VM(vNIC)层进行执行
    - 以VM为料度的防火墙

.. figure:: images/nutanix_terminology_17.png

Flow适用场景 – 带隔离的环境分区
.................................................

- 通过一键式策略进行分区隔离

- 内置针对不同环境类型的预定义类别，使策略编写变得简单 - 只需将VM添加到所需类别即可

- 移动工作负载非常简单，比如只需将类别从Dev交换到Prod即可完成
.. figure:: images/nutanix_terminology_18.png

Nutanix镜像管理
++++++++++++++++++++++++

镜像服务
.............

- 托管磁盘映射目录(RAW & ISO)
- 通过AHV调有现有镜像
- 通过PE或PC进行镜像管理
- 在线转换到Acropolis分布式存储架构
- 广泛的格式支持:
    - qcow
    - qcow2
    - vmdk
    - VHD
    - VHDx
    - RAW
    - ISO

.. figure:: images/nutanix_terminology_19.png
