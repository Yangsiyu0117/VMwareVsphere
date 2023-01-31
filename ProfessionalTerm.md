# ProfessionalTerm

[TOC]

## <u>文档标识</u>

| 文档名称 | ProfessionalTerm |
| -------- | ---------------- |
| 版本号   | <V1.0.0>         |

## <u>文档修订历史</u>

| 版本   | 日期      | 描述   | 文档所有者 |
| ------ | --------- | ------ | ---------- |
| V1.0.0 | 2023.1.19 | create | 杨丝雨     |
|        |           |        |            |
|        |           |        |            |

## vsphere 组件功能

### 一、计算

| 组件名字             | 功能说明                                                     |
| -------------------- | ------------------------------------------------------------ |
| VMware   virtual smp | 一台虚拟机使用多颗物理CPU的技术                              |
| VMware EVC           | 是VMware群集功能的一个参数。EVC   允许在不同代 CPU 之间迁移虚拟机 |

### 二、网络

| 组件名字         | 功能说明                                                     |
| ---------------- | ------------------------------------------------------------ |
| Virtual   Switch | 其也是VMKernel的一个组件，主要给ESX主机上面所有虚拟机提供网络支持。在功能方面。除了不 支持STP（Spanning ree protocol，生成树协议）和无需通过检测网络流量来获得之外，其他基本和物理交换机类似。 在vSphere中， 还有Virtual Switch的更高功能的升级版本Distributed Virtual Switch。 |

### 三、存储

| 组件名字                        | 功能说明                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| vSphere   虚拟机文件系统 (VMFS) | 一个针对 ESXi 虚拟机的高性能群集文件系统。                   |
| vSphere   存储 DRS              | 在数据存储集合之间动态分配和平衡存储容量和 I/O。该功能包括管理功能，将降低虚拟机性能的空间不足风险和 I/O 瓶颈风险降到最低。 |
| vSphere   Storage vMotion       | 可以在数据存储之间迁移虚拟机文件而无需中断服务。可以将虚拟机及其所有磁盘放置在同一位置，或者为虚拟机配置文件和每个虚拟磁盘选择单独的位置。虚拟机在   Storage vMotion 期间保留在同一主机上。通过 Storage vMotion 迁移的功能，使您能够在虚拟机运行时将虚拟机的虚拟磁盘或配置文件移动到新数据存储。通过 Storage vMotion 迁移，可以在不中断虚拟机可用性的情况下，移动虚拟机的存储器 |

### 四、组件

| 组件名字                               | 功能说明                                                     |
| -------------------------------------- | ------------------------------------------------------------ |
| VMware   ESXi                          | 一个在物理服务器上运行的虚拟化层，它将处理器、内存、存储器和资源虚拟化为多个虚拟机。 |
| VMware   vCenter Server                | 配置、置备和管理虚拟化 IT 环境的中央点。它提供基本的数据中心服务，如访问控制、性能监控和警报管理功能。 |
| VMware   vSphere Client                | 一个允许用户从任何 Windows PC 远程连接到  vCenter Server 或 ESXi 的界面。 |
| VMware   vCenter Server Appliance      | 通过web界面批量管理ESXI主机的套件。                          |
| Vmware   vSphere Update Manager        | vSphere环境升级，打补丁的工具                                |
| Vmware   vRealize Orchestrator         | 自动化引擎，建立工作流的工具---任务编排器                    |
| VMware   Data Protection               | 数据保护-----备份还原虚拟机                                  |
| vSphere   with Operations Management   | 监控和管理的工具                                             |
| vSphere   Replication                  | 分析日志来定位故障。提供的详细的日志有助于对潜在问题进行内部自查 |
| VMware NSX                             | 网络虚拟化平台管理工具，具备防火墙功能                       |
| Vmware   vSphere Integrated Containers | 容器管理工具                                                 |
| Platform   Services Controller         | vSphere 环境提供通用基础架构服务。服务包括许可、证书管理和进行 vCenter Single Sign-On 身份验证。 |

### 五、功能

| 组件名字                                       | 功能说明                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| vSphere   Distributed Switch (VDS)             | 虚拟交换机可以跨多个 ESXi 主机，使当前网络维护活动显著减少并提高网络容量。效率获得提升，可使虚拟机在跨多个主机进行迁移时确保其网络配置保持一致。 |
| vSphere   vMotion                              | 可以将打开电源的虚拟机从一台物理服务器迁移到另一台物理服务器，同时保持零停机时间、连续的服务可用性和事务处理完整性。但不能将虚拟机从一个数据中心移至另一个数据中心。 |
| VMware   vSphere SDK                           | 一种为 VMware 和第三方解决方案提供标准界面以访问 VMware vSphere 的功能。 |
| vSphere   Distributed Resource Scheduler (DRS) | 通过为虚拟机收集硬件资源，动态分配和平衡计算容量。此功能包括可显著减少数据中心功耗的 Distributed Power Management (DPM) 功能。 |
| vSphere   High Availability (HA)               | 可为虚拟机提供高可用性的功能。如果服务器出现故障，受到影响的虚拟机会在其他拥有多余容量的可用服务器上重新启动。 |
| VMKernel                                       | 虚拟化管理内核，功能是将主机硬件资源进行虚拟化，是提供虚拟化能力的核心，它将处理器、内存、存储器和资源（网络 等）虚拟化为多个虚拟机；其本质上是由VMware开发的基于POSIX协议的一个操作系统。在ESXI版本的VMkernel包含了大部分的原  Service Console的功能。 |
| Vmware FT                                      | FT是新技术,在某些条件下可以超越"双机热备"的功能，可以对运行中的虚拟机主机，在另外一台虚拟主机上，保 持一个实时同步的镜像备机，当发生宕机时，接近实时的切换到镜像备机上,几乎没有Downtime.不过对设备环境要求非常高,如虚拟主机必须支持硬件虚 拟化等 |



## vCenter Server Appliance(VCSA)和vcenter(VC)的具体区别

一、系统不同

1、VCSA：vCenter Server Appliance 基于嵌入式 linux 系统。

2、vcenter：vcenter 基于 Windows Server 系统。

二、虚拟化环境不同

1、VCSA：vCenter Server Appliance 应用于较小的虚拟化环境，主机小于 50 台，虚拟机少于 1000 个。

2、vcenter：vcenter 应用于较大的虚拟化环境，主机大于 50 台，虚拟机多于 1000 个。

三、部署不同

1、VCSA：vCenter Server Appliance 需要安装 ” 客户端集成插件 ” 并执行光盘中的 vcsa-setup.html，以 HTML 的方式部署。

2、vcenter：vcenter 支持使用 vSphere Client 或 vSphere Web Client 部署。



## 虚拟化相关理论

### 虚拟化本质特点

- 分区：每个虚拟机相互独立，互不影响
- 隔离：各个虚拟机计算、存储、网络资源隔离，互不影响
- 封装：每个虚拟机封装成vmfs独立的文件
- 解耦：将硬件解耦，提高兼容性

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613469215248-243350dc-6145-45a7-a6ca-ae079d178aa7.png)



### VCenter Server管理平台

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613469214434-24b11690-3665-400f-a22f-a843ea841c11.png)

### VCenter Client

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613469213992-dc53244f-e249-4afd-bc9b-ce9cf3c98262.png)