title: CCNA 学习笔记：配置 Cisco 设备  
date: 2018-12-24  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

[学习资料](https://www.bilibili.com/video/av12130806/?p=8)  

# 路由器组件

## 接口（背部面板上的借口）

### Console

本地连接配置时用，使用 Console 线缆连接 PC 和 路由器  

### AUX

和 Console 功能类似，但可以通过远程拨号连接  

### 数据接口

连接内网或外网  

## 内存

### ROM（Read only memory，断电不丢失）

1.  Boot strap（启动文件）

2.  POST（Power-On Self Test，开机自检）

3.  Mini IOS（类似于 PE，恢复系统）

4.  ROM monitor

### RAM（Random Access Memory，断电丢失）

RAM 中存放了 IOS 的缓存、ARP 缓存、IP 路由表、数据包缓存等。  

1.  IOS

2.  各种进程

3.  活动配置文件

4.  缓冲区

## NVRam（非易失 RAM）

储存启动配置，包括 IP 地址，路由协议，主机名  

## Flash

主要存放和运行 IOS  

## CPU 总线

# Cisco ISO 模式

主要的模式有：  

-   用户执行模式（User EXEC）  
    有限的路由器检查、远程访问  
    以 `>` 为提示符
-   特权执行模式（Privileged EXEC）  
    详尽的路由器检查、调试和测试。文件操作。远程访问。  
    以 `#` 为提示符
-   全局配置模式  
    
    在 `特权模式` 下使用 `configure terminal` 进入  
    以 `(config)#` 为提示符
-   其他特定配置模式  
    以 `(config-)#` 为提示符

在用户执行模式和特权执行模式之间，使用 `enable` 和 `disable` 进行转换  
在任何配置模式下，使用 `ctrl-z` 来退出配置模式返回特权模式  
`ctrl+^` 中断当前进程，例如 ping 之类的  
`ctrl+c` 放弃当前命令并退出配置模式  

# ?

`?` 是 Cisco IOS 的帮助命令，使用方式如下：  

```
/* 查看 cl 开头的命令有哪些
R2#cl?
clear  clock

/* 查看 clock 空格 后能使用哪些命令
R2#clock ?
  read-calendar    Read the hardware calendar into the clock
  set              Set the time and date
  update-calendar  Update the hardware calendar from the clock

/* 报错：不完整的命令
R2#clock set
% Incomplete command.

/* 查看命令格式
R2#clock set ?
  hh:mm:ss  Current Time

/* 尝试设置时间，还是提示不完整
R2#clock set 19:39:00
% Incomplete command.

/* 查看还缺什么参数，提示差日期和月份
R2#clock set 19:39:00 ?
  <1-31>  Day of the month
  MONTH   Month of the year

/* 添加 24 号，12 月，提示在 ^ 处有语法错误
R2#clock set 19:39:00 24 12
                         ^
% Invalid input detected at '^' marker.

/* 将 12 月改为 Dec，提示命令依旧不完整
R2#clock set 19:39:00 24 Dec
% Incomplete command.

/* 查看还缺什么参数，提示差年份
R2#clock set 19:39:00 24 Dec ?
  <1993-2035>  Year

/* 设置年份 2018，成功
R2#clock set 19:39:00 24 Dec 2018
*Dec 24 19:39:00.003: %SYS-6-CLOCKUPDATE: System clock has been updated from 19:36:00 UTC Mon Dec 24 2018 to 19:39:00 UTC Mon Dec 24 2018, configured from console by console.
```

# 常用命令

更改路由器名称：  

```
Router#configure terminal
Router(config)#hostname R1
R1(config)#no host
R1(config)#no hostname
Router(config)#
```

给 Console 0 口配置密码：  

```
Router(config)#line console 0
Router(config-line)#password 123456
Router(config-line)#login
Router(config-line)#end
```

配置特权模式的密码，可以使用 `password` 和 `secret` 两种，推荐使用 `secret` ，因为 `password` 是明文的。  
如果同时配置了 `password` 和 `secret=，那么密文的 =secret` 生效。  

```
Router#configure terminal
Router(config)#enable secret 123456
Router(config)#exit
```

对明文密码进行 =弱加密=。此加密只是防止查看配置文件中的口令：  

```
Router(config)#service password-encryption
```

设置标语（MOTD），经常用于显示法律信息：  

```
Router(config)#banner motd #This is my router!#
```

将配置文件储存到 NVRAM 中，以便下次启动时加载：  

```
Router#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
[OK]
```

还原到配置之前的状态：  

```
Router#reload
System configuration has been modified, Save? [yes/np]: n
Proceed with reload? [confirm]
```

删除所有配置：  

```
Router#erase startup-config
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
```

## 接口配置

进入接口配置的格式：  

```
Router(config)#interface <type> <slot>/<subslot>/<port>
```

将以太网接口 0/0 关闭再开启：  

```
Router(config)#interface fastEthernet 0/0
Router(config-if)#shutdown
*Dec 24 18:56:42.219: %LINK-5-CHANGED: Interface FastEthernet0/0, changed state to administratively down
*Dec 24 18:56:42.219: %ENTITY_ALARM-6-INFO: ASSERT INFO Fa0/0 Physical Port Administrative State Down
*Dec 24 18:56:43.219: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to down
Router(config-if)#no shutdown
*Dec 24 18:56:54.603: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up
*Dec 24 18:56:54.603: %ENTITY_ALARM-6-INFO: CLEAR INFO Fa0/0 Physical Port Administrative State Down
*Dec 24 18:56:55.603: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
Router(config-if)#exit
```

为以太网接口配置 IP 地址和 子网掩码：  

```
Router(config)#interface fastEthernet 0/0
Router(config-if)#ip address 192.168.10.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
```

查看有哪些接口：  

```
Router#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
FastEthernet0/0            192.168.10.1    YES manual up                    up
FastEthernet0/1            unassigned      YES NVRAM  administratively down down
Serial1/0                  unassigned      YES NVRAM  administratively down down
Serial1/1                  unassigned      YES NVRAM  administratively down down
Serial1/2                  unassigned      YES NVRAM  administratively down down
Serial1/3                  unassigned      YES NVRAM  administratively down down
```

查看接口的详细信息：  

```
Router#show interfaces fastEthernet 0/0
FastEthernet0/0 is up, line protocol is up
  Hardware is i82543 (Livengood), address is ca01.3dc6.0008 (bia ca01.3dc6.0008)
  Internet address is 192.168.10.1/24
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s, 100BaseTX/FX
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input never, output never, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
     0 packets input, 0 bytes
     Received 0 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
     0 watchdog
     0 input packets with dribble condition detected
     73 packets output, 8558 bytes, 0 underruns
     0 output e0 unknown protocol drops
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped outrrors, 0 collisions, 2 interface resets
```

# CDP 思科发现协议

CDP（Cisco Discovery Protocol）是思科私有的协议，提供了直接连接的 Cisco 交换机、路由器，以及其他 Cisco 设备的信息摘要，这个摘要包括：  

-   Device identifiers
-   Address list
-   Port identifiers
-   Capabilities list
-   Platform

可以使用 `show cdp ?` 来查看所有命令。  

全局开启 CDP：  

```
Router(config)#cdp run
```

接口下开启 CDP：  

```
Router(config)#interface fastEthernet 0/0
Router(config-if)#cdp enable
```

查看邻居设备：  

```
Router#show cdp neighbors
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater

Device ID        Local Intrfce     Holdtme    Capability  Platform  Port ID
R2               Fas 0/0            163          R        7206VXR   Fas 0/0
```

表示 Router 的 fastEthernet 0/0 连接到了 R2 的 fastEthernet 0/0 接口  

查看连接设备的详细信息：  

```
Router#show cdp entry *
-------------------------
Device ID: R2
Entry address(es):
Platform: Cisco 7206VXR,  Capabilities: Router
Interface: FastEthernet0/0,  Port ID (outgoing port): FastEthernet0/0
Holdtime : 172 sec

Version :
Cisco IOS Software, 7200 Software (C7200-A3JK9S-M), Version 12.4(25g), RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2012 by Cisco Systems, Inc.
Compiled Wed 22-Aug-12 11:45 by prod_rel_team

advertisement version: 2
Duplex: full
```

# Cisco 设备的备份和升级

## 使用 TFTP 备份 IOS

服务器需要安装 TFTP 服务  

# 路由器密码恢复

启动路由器时使用 `ctrl+break` 中断启动  
设置 `Configuration register` 的值为 `0x2142`  
重新启动  
进入特权模式  
把 startup-config 文件复制到 running-config 文件中  
修改密码  
还原 `Configuration register` 的值为 `0x2102`  
保存配置  
重新启动  
