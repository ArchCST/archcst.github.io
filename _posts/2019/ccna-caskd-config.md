title: 实现部分 vlan 互通，部分隔离  
date: 2019-02-13  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

# 隔离 vlan

公司的 VMware 虚拟化服务器连接到神州数码的交换机，为了保密需要，没有链接外网。  

交换机设置了四个 vlan：  

192.168.10.0/24    虚拟机的网段  
192.168.20.0/24    跑各类服务的网段  
192.168.30.0/24    职员的瘦客户端网段  
192.168.40.0/24    高管的瘦客户端网段  

<!--more-->

现在需要实现 40 网段和 20 网段互通，这样 40 瘦客户端才能使用 20 网段上跑的服务，但需要将 40 网络和 30、10 网段隔离。  

因为结构简单，只需要屏蔽掉 40 网段和 30、10 网段之间的通讯即可，可以通过配置 ACL 实现：  

```shell
/* 进入配置模式
config

/* 建立 vlan
vlan 40

/* 为 vlan 40 分配 ip
/* 配置物理接口 22
interface ethernet1/0/22

/* 将 22 接口接入 vlan 40
switchport access vlan 40

/* 启用 trunk
switchport mode trunk

/* 设置 ip 屏蔽，no3010 只是屏蔽列表的名称
ip access-list extended no3010

/* 设置屏蔽规则，阻止40和10段之间的所有通讯
deny ip 192.168.40.0 0.0.0.255 192.168.10.0 0.0.0.255

/* 设置屏蔽规则，允许所有其他的通讯
permit ip any-source any-destination

/* 退出规则编辑
exit

/* 将规则应用到对应的 vlan 40
vacl ip access-group no3010 in vlan 40
```
