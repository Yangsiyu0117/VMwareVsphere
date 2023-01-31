# VMware vsphere 8.0

[TOC]

## <u>文档标识</u>

| 文档名称 | VMware vsphere 8.0 |
| -------- | ------------------ |
| 版本号   | <V1.0.0>           |

## <u>文档修订历史</u>

| 版本   | 日期      | 描述   | 文档所有者 |
| ------ | --------- | ------ | ---------- |
| V1.0.0 | 2023.1.19 | create | 杨丝雨     |
|        |           |        |            |
|        |           |        |            |

## vSphere 安装和设置工作流

![首先安装并设置至少一个 ESXi 主机，然后部署或安装 vCenter Server。](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/GUID-5FE2EE35-63AD-4EA4-8C02-15E4407DB135-high.png)

## 安装部署

### 一、环境规划与准备

#### 1.  环境规划

| 角色 | CPU  | 内存 | 磁盘 | IP            | 说明                                                         |
| ---- | ---- | ---- | ---- | ------------- | ------------------------------------------------------------ |
| EXSI | 2    | 8    | 100G | 192.168.1.201 |                                                              |
| VCSA | 4    | 16   | 1T   | 192.168.1.200 | 需要在windows系统中安装（本文是在windows server 2019 数据中心版本安装） |

#### 2、软件包准备

[EXSI与VCSA下载]: https://customerconnect.vmware.com/cn/downloads/info/slug/datacenter_cloud_infrastructure/vmware_vsphere/8_0

### 二、安装EXSI 8.0

#### 1、[ESXi](https://so.csdn.net/so/search?q=ESXi&spm=1001.2101.3001.7020) Standard Boot Menu

![image-20230119153411574](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119153411574.png)

#### 2、等待镜像加载完成

![image-20230119153438608](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119153438608.png)

#### 3、服务初始化中

![image-20230119153531354](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119153531354.png)

#### 4、ESXi安装导向

![image-20230119153609179](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119153609179.png)

#### 5、最终用户许可协议

> 按【F11】接受协议，进入下一步。

![image-20230119153651360](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119153651360.png)

#### 6、系统会自动扫描可用的磁盘

![image-20230119154109561](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119154109561.png)

#### 7、选择安装的磁盘位置，回车继续

![image-20230119155207938](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155207938.png)

#### 8、选择键盘类型

![image-20230119155239299](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155239299.png)

#### 9、设置root密码

![image-20230119155308123](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155308123.png)

#### 10、ESXi 告警

> 因为在刚开始设置ESXi 资源时，没有开启CPU[虚拟化](https://so.csdn.net/so/search?q=虚拟化&spm=1001.2101.3001.7020)功能，所以在安装时会有告警提示。可以暂时忽略，待安装完成后，再关机勾选开启CPU虚拟化功能。

![image-20230119155342205](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155342205.png)

#### 11、确认系统盘

![image-20230119155423562](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155423562.png)

ESXi 8.0 安装中，等待2-3分钟

![image-20230119155554606](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155554606.png)

#### 12、安装完成

![image-20230119155620169](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155620169.png)

#### 13、ESXi 系统初始化配置中

![image-20230119155700939](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155700939.png)

#### 14、系统初始化完成

![image-20230119155724538](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155724538.png)

#### 15、配置服务器IP地址

按下F2键弹出登陆界面

![image-20230119155940747](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155940747.png)

选择 "Configure Management Network"

> **选项作用：**
>
> Configure Password 配置root密码
>
> Configure Management Network 配置网络
>
> Restart Management Network  重启网络
>
> Test Management Network 使用ping测试网络
>
> Network Restore Options  还原配置
>
> Troubleshooting Options 故障排查选项
>
> View System Logs 查看系统日志
>
> Reset System Conf iguration ESXi 出厂设置

![image-20230119155959563](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119155959563.png)

修改IP地址

![image-20230119160102166](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160102166.png)

此时ESXi 8.0 系统安装完成。

#### 16、验证

通过跳板机访问 ESXi Host Client。

![image-20230119160213173](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160213173.png)



### 三、安装VCSA 8.0 for win

#### 1、说明

VCSA安装有两个阶段：

- 阶段1安装的是VAMI的服务，也就是5480端口的WEB UI。
- 阶段2安装的是vSphere Client的服务。

#### 2、打开安装程序（Installer）

打开一台Windows宿主机（虚拟机），在上面挂载关于vcsa 8的镜像文件（安装程序）

（1）进入vcsa镜像目录

打开文件管理器，找到挂载好的VCSA8镜像，双击进入DVD目录中。

![image-20230119160605517](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160605517.png)

（2）打开图形化安装目录

选择`vcsa-ui-installer`文件夹，该文件夹下存放着GUI方式安装VCSA的执行程序。

![image-20230119160648812](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160648812.png)

（3）选择操作系统

根据安装程序运行的操作系统选择相应的安装格式。在这里宿主机是windows系统，因此打开【win32】目录。

- lin64：Linux安装目录。
- mac：Mac OS安装目录。
- win32：Windows安装目录。

![image-20230119160724359](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160724359.png)

（4）打开安装程序

![image-20230119160748131](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160748131.png)

#### 3、第1阶段安装

（1）点击【Install】

右上角可以选择安装时使用的语言，默认使用英语

![image-20230119160831343](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160831343.png)

（2）安装介绍

上方黄色告警框提示：外部PSC部署已经被弃用。点击【NEXT】进行下一步。

![image-20230119160902950](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160902950.png)

（3）最终用户许可协议

勾选【I accept the terms of the license agreement】接受后，点击【NEXT】进行下一步。

![image-20230119160926595](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119160926595.png)

（4）指定安装目标主机

这里指定将[vCenter](https://so.csdn.net/so/search?q=vCenter&spm=1001.2101.3001.7020) Server Appliance 8.0 安装在哪个ESXi主机上，输入ESXi IP或FQDN，用户名和密码后，点击【NEXT】进行下一步。

![image-20230119161005311](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161005311.png)

（5）证书警告

当安装程序与ESXi主机正常通信后，为了安全起见，会向用户核实目标ESXi主机的SHA1指纹。确认无误后，点击【YES】进行下一步。

![image-20230119161035183](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161035183.png)

数据验证中

![image-20230119161110851](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161110851.png)

（6）设置vCenter Server 信息

设置vc的名称和密码。

![image-20230119161141844](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161141844.png)

（7）选择部署大小

默认为最小配置，这里选择默认即可，点击【NEXT】进行下一步。

VC7的最小默认配置是2核vCPU和**12**GB内存。

VC8的最小默认配置为2核vCPU和**14**GB内存。

![image-20230119161211935](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161211935.png)

（8）指定数据存储

这里的数据存储也是ESXi连接的数据存储，选择合适的数据存储后，勾选精简制备【Enable Thin Disk Mode】，点击【NEXT】进行下一步。

在VC7中，会有【Install on a new vSAN cluster containing the target host】的存储选择。
![image-20230119161331728](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161331728.png)

（9）设置vCenter Server网络

建议设置静态IP和FQDN。

![image-20230119161417618](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161417618.png)

（10）第1阶段配置确认

确认安装第1阶段的配置无误后，点击【FINISH】进行第2阶段的部署。

![image-20230119161459499](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161459499.png)

开始vCenter Server 8.0 第一阶段的部署。

![image-20230119161523811](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161523811.png)

第一阶段的安装大约需要等待30分钟。

![image-20230119161543275](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161543275.png)

此时在ESXi8主机上会自动创建一台名为vc8-2的VM虚拟机。如下图所示，VM正在创建中。

![image-20230119161610775](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161610775.png)

#### 4、第2阶段安装

当第1阶段部署完成后，vCenter Server Management Interface安装完成，可以根据提示通过浏览器进行访问。点击【CONTINUE】进入第2阶段部署。

（1）第2阶段安装介绍

阅读后，点击【NEXT】进行下一步。

![image-20230119161724790](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161724790.png)

（2）设置[NTP](https://so.csdn.net/so/search?q=NTP&spm=1001.2101.3001.7020)服务

如果实验环境中有设置NTP服务器，选择自定义NTP，否则默认配置，同时开启SSH访问。点击【NEXT】进行下一步。

![image-20230119161838404](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230119161838404.png)

（3）设置SSO 单点登录

创建新的SSO域，这里的SSO与AD域是两个不同的概念。SSO域是vCenter Server的登录系统。在这里我们选择加入已有的SSO域。

输入已有SSO域的vCenter Server的IP或域名和密码，点击【NEXT】进行下一步。

![image-20230129091256857](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091256857.png)

假如使用IP无法加入SSO域，那么改用FQDN。

![image-20230129091328232](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091328232.png)

（4）配置CEIP

根据实际情况确认是否加入[VMware](https://so.csdn.net/so/search?q=VMware&spm=1001.2101.3001.7020)客户体验提升计划，点击【NEXT】进行下一步。

![image-20230129091408981](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091408981.png)

（5）第2阶段配置确认

确认安装第2阶段的配置无误后，点击【FINISH】进行第2阶段的部署。

![image-20230129091431538](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091431538.png)

弹出警告框，提示第2阶段开始安装后将无法停止。点击【OK】进行最后的部署。

![image-20230129091503104](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091503104.png)

此时vCenter Server第2阶段的安装正在进行中。

![image-20230129091519814](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091519814.png)

等待大概40-60min后，网页会自动跳转到VAMI界面。此时说明VCSA第二阶段已经部署完成。

（6）安装完成

打开浏览器新标签页输入：`https://ip`，点击【LAUNCH VSPHERE CLIENT】。

![image-20230129091558527](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091558527.png)

（7）登录 VSPHERE CLIENT

用户名为已有SSO域的administrator账户。

![image-20230129091630328](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/image-20230129091630328.png)





### 安装VCSA 8.0 for linux







### 四、创建虚拟机

#### 一、上传系统镜像

1. 1. 打开数据中心

2. ![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468178353-2daf5115-e8bc-4f31-80f8-205c5571627f.png)

1. 2. 新建文件夹，存放镜像

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468180005-0cb139da-9417-47a2-ba3f-8d2a699d5f03.png)

1. 3. 点击上传文件按钮

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468179368-83dde442-557f-4463-8703-e0d9fe3a0f3a.png)

1. 4. 找到本地镜像上传

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468181759-5dd8d59b-8c30-4ae7-81b9-a096ff075213.png)

#### 二、安装虚拟机

1. 1. 创建虚拟机

2. ![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468143004-f0c14a89-ba3e-4a0c-a85a-a0a98adcc3df.png)

1. 2. 选择创建类型

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468142613-c4fd5aa9-da56-441e-99d8-af89b5ee469a.png)

1. 3. 为虚拟机命名并选择虚拟机安装的所在位置

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468144365-7fd22c31-99d7-411e-94fa-5baff5a86cb0.png)

1. 4. 选择计算资源

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468142760-04846295-069f-42ab-a910-988576a615a7.png)

1. 5. 选择存储

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468145172-11fa4741-8acb-4a20-aba0-1209d343d66a.png)

1. 6. 选择兼容性

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468142811-e5c3b1d4-0246-42e4-aa1e-92dd8b0002bd.png)

1. 7. 选择虚拟机操作系统

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468144863-1c5c2524-cc2f-4f26-b0ae-4a6948fa2897.png)

1. 8. 配置虚拟机硬件

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468147405-b32a51bc-45de-4817-9582-ad86d42ffcb2.png)

1. 9. 在新CD/DVD驱动器项选择内容库ISO文件

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468130456-6c738f5f-683e-437a-afb9-1c41d035e963.png)



1. 10. 选择要挂载的ISO镜像

2. ![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468134307-eb313214-45a5-4c99-99c7-b10aa1bd2cac.png)

3. 

1. 11. 之后会显示配置概要，确认后点击“完成”即可

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468144729-f0687523-41a3-4d6a-844c-95fac76d13ba.png)

1. 12. 之后右键点击该虚拟机进行启动

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468133011-c7a9cd85-3f61-462f-a3d8-aa4cd18db69e.png)

1. 13. 启动之后打开控制台

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468154298-459e77f2-3a5b-4c8f-bd37-8a7f86a07273.png)

1. 14. 配置安装该虚拟机系统

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468158167-a579b73b-5a5c-426b-ae22-902079a286dc.png)

#### 三、注册虚拟机

- 平时使用中很有可能会得到一些在其他环境上创建的虚拟机。例如：只给了一些虚拟机文件（ VMX和 VMDK文件等），我们就可以通过这些文件部署虚拟机。

1. 1. 打开登录vsphere web client，找到存储列表

2. ![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468147021-12aba631-9c75-46a1-8ca9-0814e8cb9b3e.png)

1. 2. 选择要把虚拟机文件放置的数据存储

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468152345-3464fffd-2b73-4760-8821-a709fcba9751.png)

1. 3. 将虚拟机文件上传至该数据存储，点击存储根目录，上传文件夹（将包含 VMX、 VMDK等虚拟机文件的文件夹上传）。

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468155588-b1677c2f-ded4-49e2-9a4d-eb08fc2f7131.png)

1. 4. 确认虚拟机文件上传成功，确认 vmdk、 vmx文件（其他文件后期在关于虚拟机备份时会详细讲到）。

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468153075-dc502d80-61b8-479c-ad4e-ce1f24a352c5.png)

1. 5. 注册虚拟机，选中 vmx文件，"注册虚拟机"由灰色变亮色

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468155582-0e297828-7c6f-441f-a387-cc908cecf5a4.png)

1. 6. 指定名称（可以不变也可以修改）和虚拟机放置位置

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468149235-4a7f7ba2-b344-4371-bab7-df75aa17f842.png)

1. 7. 选择计算资源，可以直接选择群集（会将虚拟机自动分配至某个主机上），也可以指定某个主机。

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468147115-820dfe14-8511-420c-a111-c9c5b6bf8acf.png)

1. 8. 确认配置，完成注册

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468147115-915aa4f1-21a0-43dd-bf87-5ac45164bf9d.png)

#### 四、虚拟机硬件以及软件版本

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468165352-1866b3f6-d051-4b5e-97c7-88541e84928f.png)

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468149845-06e63c8e-1d84-4125-a99b-db15c1946eec.png)

1. 虚拟机cpu、内存限制

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468172235-36c16ae2-092a-4c5d-b1a1-6342cf56f7bf.png)

1. 虚拟磁盘

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468170547-5f13fd2e-8af0-4498-9121-34fb16a74630.png)

1. 厚配置虚拟磁盘

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468177370-43788049-ae6a-4ea9-9a59-a12c721196ad.png)

#### 五、vmware  tools

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468170843-bece0f61-87cf-4375-b6f8-6be7a5b4640a.png)

#### 六、vmware控制台

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468163280-b7bb11dd-65c5-4ba5-b289-1ae3e56c4026.png)



### 五、配置和管理虚拟网络

#### 一、什么是虚拟网络？什么是虚拟交换机

1. 虚拟网络：可为使用虚拟交换机的主机和虚拟机提供网络连接。
2. 虚拟交换机：

- 用于引导虚拟机之间的网络流量并链接到外部网络。
- 用户合并多个网络适配器的带宽并均衡他们之间的流量。此外还可用于处理物理网卡的故障切换。
- 模仿物理以太网交换机

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468015003-f0a227a3-9fb9-489e-9e45-4870f62dde2d.png)

#### 二、常见组件和术语

1. vsphere标准交换机：基于软件的交换机，负责管理虚拟机的流量。用户必须在各个ESXI主机独立管理vsphere标准交换机（即，每台主机都至少有一个vsphere标准交换机）
2. Sphere分布式交换机：基于软件的交换机，由位于一个vsphere数据中心中的ESXi主机和集群来分享和管理。（即，多个主机可共用一个分布式交换机）
3. 端口/端口组：vswitch的逻辑对象，一个虚拟交换机可以包含一个VMkernel端口或一个虚拟机端口组。
4. VMkerneI端口：一种特殊的虚拟交换机端口类型，配置有一个ip地址，用于支持虚拟机管理程序管理流量、vMotion、vSAN流更、NAS、NFS访问等。
5. 虚拟机端口组：虚拟交换机端口，共享一组相同的配置，并且允许虚拟机访问在相同的端口组、可访问PVLAN上或物理网络上配置的其他虚拟机。
6. 虚拟LAN:虚拟或物理交换机上配置的逻辑LAN,可以提供高效的流量分段、广播控制和安全性，同时指向为特定虚拟LAN(VLAN)配置的端口传输流量，从而提高带宽使用率。
7. 中继端口：物理交换机端口，负责监听和执行多个VLAN的流量传输。采用的方法是保存将流量从中继端口传输到所连接设备的802·IqVLAN标记。中继端口通常用于交换机与交换机的通信，使不同交换机上的多个VLAN能够自由通信。虚拟交换机支持VLAN，并且使用VLAN中继实现VLAN与虚拟交换机的自由连接。

#### 三、虚拟交换机

1. 虚拟交换机分类及功能

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468006262-fed64064-228e-4c5a-ae55-14214ba281fa-20230129093629162.png)

2. 虚拟交换机使用类型

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468007173-92d1c550-6911-4c2b-8dc3-749afc7b3ad5-20230129093641555.png)

3. 虚拟交换机vlan

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468017987-62afc9f0-ba9d-4677-b288-50cea8641a35.png)

4. 虚拟交换机类型

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467966783-870a955f-0ca1-48aa-8798-9000015f3c9d.png)

#### 四、分布式虚拟交换机

1. 逻辑结构

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467969546-c4274da4-bb39-4266-b037-6b4676cde226.png)

2. 分布式交换机与标准交换机区别

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467994848-db8441fb-042d-467b-a62e-3874b893c34a.png)

#### 五、安全策略

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467915350-c1590cde-541d-40a0-b42f-39cb9cd7cd08.png)

1. 混杂模式

在同一个VLAN里的虚拟枧能收到该VLAN的所有数据包。假如客户安装Wireshark或者其他抓包工具，就可以看到目标是其它虚拟机的数据包和广播包。

2. MAC地址更改

ESXi主机发现虚拟机篡改了MAC地址（网卡MAC地址改为与\/M)(文件中定义的MAC地址不同）。

- 拒绝：如果虚拟机修改了MAC地址，与该虚拟机连接的虚拟交换机端口就被禁用。（执行者是虚拟交换机）
- 接受：虚拟机修改了MAC地址，不会禁用端口。（如果使用网络负载均衡，或连接iSCSl存储，就需要设置为"接受"）

3. 伪传输

修改MAC地址后，发出的帧的源MAC相应也会改变，但有些软件（或者是木·马）会直接修改以太网帧的源MAC。此时正在传输的帧的源MAC与虚拟网卡的MAC不同，本策略关注的就是这点：是否有软件修改了帧的源MAC地址，使其与网卡的'有效MAC地址"不符。有软件以伪造的源MAC地址向外发送数据帧，虚拟网卡就删除该帧，但放行合法的帧。这说明本策略的执行动作是'过滤"，而非一刀切的"断网'。

#### 六、流量调整

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467921616-8bae07c7-f3e0-4c94-b929-a0f1bc825163.png)

1. 流量调整是一种用于控制虚拟机网络带宽的机制。在vswitch中，平均带宽、峰值带宽、突发大小均可配置。
2. 默认禁用状态

适用于标准虚拟交换机中的每个虚拟网卡

3. 流量调整仅控制出站流量

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467931346-01eb6034-f997-468d-8b48-1fb2591e3df5.png)

#### 七、绑定与故障切换

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468015039-98deb52d-3e48-4327-acef-1cbb6a9d5dac.png)

1. 负载平衡

- 基于IP哈希的路由

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468015039-98deb52d-3e48-4327-acef-1cbb6a9d5dac-20230129093927983.png)

![img](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468015039-98deb52d-3e48-4327-acef-1cbb6a9d5dac-20230129093934251.png)

虚拟交换机可根据每个数据包的源和目标IP地址选择虚拟机的上行链路。任何虚拟机都可根据源和目标|P地址使用网卡组中的任何上行链路。因此，每台虚拟机都可以使用网卡组中任何上行链路的带宽。

如果虚拟机在包含大量独立虚拟机的环境中运行，则IP哈希算法可在组中的网卡之间均匀地分布流量。当虚拟机与多个目标IP地址通信时，虚拟交换机可为每个目标|P生成不同的哈希。因此，数据包可以使用虚拟交换机上的不同上行链路，从而可能实现更高的吞吐

如果环境中包含的IP地址较少，则虚拟交换机可能会始终通过组中的一个上行链路传递流量。例如，如果一个应用程序服务器访问一个数据库服务器，则虚拟交换机会始终计算同一个上行链路，因为只存在一个源-目标对。

- 基于源虚拟端口的路由

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468023806-41f42c62-1aa1-4051-871e-184e4f95d49c.png)

将每一个虚拟交换机端口绑定到vswitch关联的一个特定上行链路。算法会尝试在所有链路上保持相同数量的端口与上行链路配对，从而实现负载平衡。

该策略可保证来自一个连接虚拟交换机端口的特定虚拟网络适配器的流量总是使用同一个物理网络适配器。如果有一条上行链路出现问题，那么出现故障的上行链路的流量会转移到另外一个物理网络适配器上。

所以该策略并不支持动态负载均衡，但是支持冗余性。

- 基于源MAC哈希的路由

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468013645-0913e40b-3bc9-411a-b19d-2b108f7fc614.png)

该策略也容易出现与基于虚拟交换机端口策略相同的问题，因为来源MAC地址和vswitch端口一样都是静态的。当虚拟网络适配器比物理网络适配器多的时候，最好也是使用基于来源

MAC策略。当虚拟机使用多个虚拟网络适配器时会有多个来源MAC地址，因此可以使用多个物理网络适配器。

1. 网络故障检测

- 仅链路状态

通过物理网络适配器提供的链路状态判断链路的故障。物理交换机上的一些事件会判断故

障，例如，线路断开或电源故障。链路状态故障恢复检测设置的缺点是无法判断错误配置，

也无法交换机线路错误连接到其他网络设备。

其他方法：例如：思科产品中有链路状态跟踪特性，可以让交换机检测到上游端口断开和重新响应状态。可以减少信号检测的使用，甚至

完全取代信号检测。

- 信标探测

采用信标探测的故障恢复设置也会使用链路状态，会给所有物理网络适配器发送广播帧。这些广播帧可以帮助vswitch检测上有网络连接故障。

#### 八、上链路端口冗余操作

1. exsi主机添加四块网卡

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467981808-a389ac6f-3961-4a51-a738-2dda208a3935.png)

2. 进入到esxi主机web管理界面，查看网卡信息

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468020709-7a968f73-7ae8-414a-8d4c-cc234678d02d.png)

3. 进入到vcenter，主机——配置——虚拟交换机——添加主机网络

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468020636-8902a2fd-f498-4a59-a895-34e067772127.png)

4. 选择物理网络适配器

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467971001-74a5469a-2f66-46ee-9957-c01624730532.png)

5. 选择目标设备

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467950330-fc8628b6-3131-4708-966a-38d1ecd4250f.png)

6. 选择物理网卡

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467970033-ca08472d-dc6a-4fae-b37a-84f187a88931.png)

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467982836-e4b1fb85-0415-4845-9beb-1a77308da64f.png)

7. 即将完成

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467947607-9358b1c3-6f4b-4ba9-8c4a-45cc0efb68a9.png)

8. 查看网络拓扑

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467984397-28262cc1-bd18-432e-aa8c-a739ef2ae90c.png)

#### 九、添加vlan端口组（物理交换机也要有对应的vlan id）

1. 选择操作的虚拟交换机，点添加主机网络

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468021624-f17c5feb-d735-4bd8-ba03-78e61b17fd60.png)

2. 选择标准交换机虚拟机端口组

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467964928-38524036-a94b-414e-b247-188f07f16b16.png)

3. 选择交换机

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467948196-c627255d-410b-4cf2-90e0-def87ce11357.png)

4. 创建vlan

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467949380-d2f91845-c998-4a29-a0aa-7f48bcde06e0.png)

5. 查看网络拓扑

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467977598-e8f96181-7f06-4529-878e-bdb2de476734.png)

6. 将虚拟机切换到vlan2

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467972007-dec1c3e7-b3a7-4500-a3bd-be5aa85e9ade.png)

#### 十、创建分布式交换机

1. 新建分布式交换机

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468018282-4a7196d9-694c-4234-acca-1c41dad75645.png)

2. 设置名称和位置

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467954766-220323cb-2085-4ccc-b27d-3274527bfaf6.png)

3. 选择dvs版本

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467986257-46a5dbdd-2699-4c6a-bd78-8d356fc670e9.png)

4. 编辑设置

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467946964-2f2c2359-9fda-45ae-b529-27f742d29081.png)

#### 十一、分布式交换机添加上行链路网卡

1. 选中分布式交换机——添加管理主机

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468017381-4bd103fe-7eeb-4835-913c-25a753984756.png)

2. 选择添加主机任务

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467969094-fcac5ca9-febc-4205-b937-a2929d55373f.png)

3. 选择主机

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467963386-015bf182-97c5-438c-a3ef-402814f084ac.png)

4. 选择网络适配器任务

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467995804-d66ec067-b250-4443-b83d-dce9b3262d0f.png)

5. 选中网卡，分配上行链路

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467981502-40b69bd5-302e-4360-85d7-c06ef145e374.png)

6. 完成操作

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468016718-2ed0232d-fbca-4963-bcbf-215683caa11d.png)

#### 十二、分布式交换机添加vlan端口组

1. 点击新建分布式端口组

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468018960-b36f0be0-76da-40e7-95ff-23b431fb3da6.png)

2. 设置名称

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467945939-17349901-98a1-4835-b2ac-c1d25a80d857.png)

3. 设置vlan

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467963757-76d04436-118a-4da7-b280-68cad71567aa.png)

4. 编辑虚拟机设置，选择更多网络

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468009568-1ef98396-956d-4757-983c-df1b93de6ba2.png)

#### 十三、vmotion网络配置

1. 虚拟机迁移是报错

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468023853-115270ad-80cc-452e-b6af-ba6645ffa70d.png)

2. 主机——配置——vmkernel适配器——vmotion

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613468016755-dca66d92-f963-447a-a52c-733610958e8f.png)

3. 编辑设置——勾选vmotion

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467946182-b73e078f-1dcc-4681-bc30-67772fbc3728.png)



### 六、配置和管理虚拟存储

#### 一、虚拟机文件

1. 1. 一台vmware虚拟机文件夹中包括的文件

| .log   | 这个文件记录了VMware Workstation对虚拟机调节运行的情况。当你碰到问题时，这些文件对我们做出故障诊断非常有用。这个文件和虚拟机的配置文件（.vmx）储存在一个目录里面。 |
| ------ | ------------------------------------------------------------ |
| .nvram | 这是一个储存虚拟机BIOS状态信息的文件。                       |
| .vmdk  | 这是一个虚拟磁盘文件，它储存了虚拟机硬盘驱动器里的内容。一台虚拟机可以由一个或几个虚拟磁盘文件组成。如果你已经特别指定了虚拟磁盘每2GB为一单独文件的话，虚拟磁盘的大小就决定了虚拟磁盘文件的数量。随着数据写入虚拟磁盘，虚拟磁盘文件将变大，直到这些文件为2GB。（如果你在创建虚拟磁盘时已经把所有的空间都分配了，那么这些文件将在初始时就具有最大尺寸并且不再变大了）。几乎所有的虚拟磁盘文件内容关于虚拟机里的磁盘数据，仅仅一小部分是虚拟机的分区信息。如果虚拟机是直接与物理硬盘所连接而不是虚拟磁盘的话，虚拟磁盘文件则保存着虚拟机能够访问的分区信息。早期版本的VMware产品用.dsk扩展名来表示虚拟磁盘文件。<disk    name>-<###>.vmdk 这是一个再次命名文件，当虚拟机有一个或多个快照时，就会自动创建它。当虚拟机运行时，这个文件就用来储存对虚拟磁盘作更改的内容。可能这样的文件有多个。虚拟机通过加###这种文件名不重复出现的后缀的命名方式以避免文件重名。 |
| .vmem  | 虚拟机页面文件，它用来备份客户机保存在宿主机上主内存信息。这个文件只有在虚拟机运行时或崩溃后存在。每个虚拟机运行时所建立的快照对应一个.vmem文件，它包含了客户机的驻内存信息，它是快照的一部分。 |
| .vmsd  | 这是一个集中储存了快照的相关信息和元数据的文件。在它的目录中，可能其它一些文件只有在虚拟机运行时才存在。（而它不会消失） |
| .vmsn  | 这是一个快照状态信息文件，它记录了你在建立快照时虚拟机的状态信息 |
| .vmss  | 这是一个储存虚拟机挂起状态信息的文件。一些早期版本的VM产品用.std来表示这个文件。 |
| .vmtm  | 这是含有虚拟机组资料的配置文件。                             |
| .vmx   | 这是一个初始的配置文件，它储存着创建虚拟机向导或虚拟机编辑器对虚拟机的一些设置。如果你用的是Linux下的VM虚拟机，这个文件的扩展名将是.cfg。 |
| .vmxf  | 这个文件是虚拟机组中补充的配置文件。注意当虚拟机组被移除后，这个文件将保留下来。 |

#### 二、vmware存储

1. 1. vmware支持的存储类型

2. ![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467736952-3be9f3e7-c071-41d2-b16e-6ac87c5400cc.png)

1. 2. 存储协议

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467666663-f07d6c83-13a5-4904-928f-f7ae05251027.png)

1. 3. 数据存储类型

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467711296-e3cb798f-f44d-4eec-89de-8f17f000d08e.png)

1. 4. RDM裸设备映射（写入数据不经过vmfs封装，性能好，不可迁移）

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467735893-530e7219-a8e4-484a-8d13-18d845cfdabc.png)

1. 5. 虚拟机添加RDM盘

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467735346-c6f6e2cb-0f0b-4cb5-87b1-9661b7b75234.png)

1. 6. 光纤fc-san

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467709842-c8e2443f-a002-451e-88d4-e8d4f4977396.png)

1. 7. 光纤通道多路径（冗余）

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467712709-a6e7c130-cf7f-4ea0-9305-78d9c203395a.png)

1. 8. FCoE适配器（ipsan、fcsan共存组网情况使用）

![](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467735346-c6f6e2cb-0f0b-4cb5-87b1-9661b7b75234-20230129095539918-20230129095649352.png)

1. 9. ip-san

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467709368-039f6daf-969e-4931-9abf-b15e42956d56.png)

1. 10. iscsi适配器

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467735080-2b88c003-6b89-4485-bdd2-2b2155144953.png)

1. 11. ip-san网络配置

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467729173-c5375a14-3dbd-4475-a293-508c1966d800.png)

1. 12. 创建与发现iscsi服务

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467699624-fa1a6db1-1aca-41df-a019-60752c9dae16.png)

1. 13. nfs

![image.png](https://raw.githubusercontent.com/Yangsiyu0117/images/main/markdown/pictures/1613467708150-468de5af-83ff-41ae-9daf-da746f49347c.png)

























## <u>附录：不同ESXi主机版本所支持的最高虚拟机的硬件版本</u>

| 功能                             | ESXi 8.0 及更高版本 | ESXi 7.0 Update 3 及更高版本 | ESXi 7.0 Update 2 及更高版本 | ESXi 7.0 Update 1 及更高版本 | ESXi 7.0 及更高版本 | ESXi 6.7 Update 2 及更高版本 | ESXi 6.7 及更高版本 | ESXi 6.5 及更高版本 |
| :------------------------------- | :------------------ | :--------------------------- | :--------------------------- | :--------------------------- | :------------------ | :--------------------------- | :------------------ | :------------------ |
| 硬件版本                         | 20                  | 19                           | 19                           | 18                           | 17                  | 15                           | 14                  | 13                  |
| 内存最大值 (GB)                  | 24560               | 24560                        | 24560                        | 24560                        | 6128                | 6128                         | 6128                | 6128                |
| 最大逻辑处理器数目               | 768                 | 768                          | 768                          | 768                          | 256                 | 256                          | 128                 | 128                 |
| 每个插槽的最大内核数（虚拟 CPU） | 64                  | 64                           | 64                           | 64                           | 64                  | 64                           | 64                  | 64                  |
| 最大 SCSI 控制器数               | 4                   | 4                            | 4                            | 4                            | 4                   | 4                            | 4                   | 4                   |
| Bus Logic 控制器                 | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| LSI Logic 控制器                 | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| LSI Logic SAS 控制器             | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| VMware 准虚拟控制器              | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| SATA 控制器                      | 4                   | 4                            | 4                            | 4                            | 4                   | 4                            | 4                   | 4                   |
| NVMe 控制器                      | 4                   | 4                            | 4                            | 4                            | 4                   | 4                            | 4                   | 4                   |
| 虚拟 SCSI 磁盘                   | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| SCSI 直通                        | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| SCSI 热添加支持                  | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| IDE 节点                         | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| 虚拟 IDE 磁盘                    | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| 虚拟 IDE CD-ROM                  | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| IDE 热添加支持                   | N                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| 最大网卡数                       | 10                  | 10                           | 10                           | 10                           | 10                  | 10                           | 10                  | 10                  |
| PCNet32                          | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| VMXNet                           | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| VMXNet2                          | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| VMXNet3                          | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| E1000                            | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| E1000e                           | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| USB 1.x 和 2.0                   | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| USB 3.1 SuperSpeed               | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| USB 3.1 SuperSpeedPlus           | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| 最大显存 (MB)                    | 256                 | 256                          | 256                          | 256                          | 128                 | 128                          | 128                 | 128                 |
| 3D 显存最大值 (GB)               | 8                   | 8                            | 8                            | 8                            | 4                   | 2                            | 2                   | 2                   |
| SVGA 显示器                      | 10                  | 10                           | 10                           | 10                           | 10                  | 10                           | 10                  | 10                  |
| SVGA 3D 硬件加速                 | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| VMCI                             | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| PCI 直通                         | 32                  | 16                           | 16                           | 16                           | 16                  | 16                           | 16                  | 16                  |
| 动态 DirectPath                  | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| 增强型 DirectPath I/O            | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| 供应商设备组                     | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| PCI 热添加支持                   | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| 虚拟精度时钟设备                 | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| 虚拟监视程序定时器设备           | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| 虚拟 SGX 设备                    | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| 嵌套 HV 支持                     | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| vPMC 支持                        | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | Y                   |
| 串行端口                         | 32                  | 32                           | 32                           | 32                           | 32                  | 32                           | 32                  | 32                  |
| 并行端口                         | 3                   | 3                            | 3                            | 3                            | 3                   | 3                            | 3                   | 3                   |
| 软盘设备                         | 2                   | 2                            | 2                            | 2                            | 2                   | 2                            | 2                   | 2                   |
| PVRDMA                           | 10                  | 10                           | 10                           | 1                            | 1                   | 1                            | 1                   | 1                   |
| PVRDMA 原生端点（不带 vMotion）  | Y                   | Y                            | Y                            | Y                            | N                   | N                            | N                   | N                   |
| PVRDMA 原生端点（带 vMotion）    | Y                   | Y                            | Y                            | N                            | N                   | N                            | N                   | N                   |
| NVDIMM 控制器                    | 1                   | 1                            | 1                            | 1                            | 1                   | 1                            | 1                   | N                   |
| NVDIMM 设备                      | 64                  | 64                           | 64                           | 64                           | 64                  | 64                           | 64                  | N                   |
| vGPU                             | 8                   | 4                            | 4                            | 4                            | 4                   | 4                            | 4                   | 4                   |
| WDDM 1.2                         | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| 虚拟 I/O MMU                     | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | N                   |
| 虚拟可信平台模块                 | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | N                   |
| Microsoft VBS                    | Y                   | Y                            | Y                            | Y                            | Y                   | Y                            | Y                   | N                   |
| Direct3D 10.1                    | Y                   | Y                            | Y                            | Y                            | Y                   | N                            | N                   | N                   |
| Direct3D 11.0                    | Y                   | Y                            | Y                            | N                            | N                   | N                            | N                   | N                   |
| OpenGL 4.3                       | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| AMD SEV-ES                       | Y                   | Y                            | Y                            | Y                            | N                   | N                            | N                   | N                   |
| 虚拟 NUMA 拓扑                   | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| 数据集服务                       | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| vMotion 应用程序通知             | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| 虚拟超线程                       | Y                   | N                            | N                            | N                            | N                   | N                            | N                   | N                   |
| UEFI                             | 2.7A                | 2.4                          | 2.3.1                        | 2.3.1                        | 2.3.1               | 2.3.1                        | 2.3.1               | 2.3.1               |



## <u>附录：vCenter Server Appliance(VCSA)和vcenter(VC)的具体区别</u>

**一、系统不同**

​	1、VCSA：vCenter Server Appliance 基于嵌入式 linux 系统。

​	2、vcenter：vcenter 基于 Windows Server 系统。

**二、虚拟化环境不同**

​	1、VCSA：vCenter Server Appliance 应用于较小的虚拟化环境，主机小于 50 台，虚拟机少于 1000 个。

​	2、vcenter：vcenter 应用于较大的虚拟化环境，主机大于 50 台，虚拟机多于 1000 个。

**三、部署不同**

​	1、VCSA：vCenter Server Appliance 需要安装 ” 客户端集成插件 ” 并执行光盘中的 vcsa-setup.html，以 HTML 的方式部署。

​	2、vcenter：vcenter 支持使用 vSphere Client 或 vSphere Web Client 部署。
