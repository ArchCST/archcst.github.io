title: 华为 USG6000 系列防火墙重置管理员密码  
date: 2019-02-18  
updated:  
comments: true  
tags:  

-   learn

layout: post  

---

前任 IT 留下来的华为 USG6000 防火墙莫名其妙的重置了 DHCP 配置，开发原来使用的 IP 不能用了。然而前任 IT 忘记了登陆密码，不得不折腾一下密码重置。  

# 清除密码

绿联的 Console 线用的 FT232R 芯片，可以直接通过 Windows 安装驱动。在设备管理器的 `端口` 中确定 COM 序号后使用 Putty 链接到 USG6000 的 Console 口，然后重启设备。  

出现 `Press Ctrl+B to Enter Main Menu...3` 时按 `Ctrl+B` ，输入 BootROM 密码进入主菜单。 USG6000 的 BootROM 默认密码为 `O&m15213` 。  

<!--more-->

```shell
Press Ctrl+B to Enter Main Menu...3                                            
Password: ********                                                              

                                                                                
For the sake of security, please modify the original password.

                                                                                
====================< Extend Main Menu >====================                    
| <1> Boot System                                          |                    
| <2> Set Startup Application Software and Configuration   |                    
| <3> File Management Menu...                              |                    
| <4> Load and Upgrade Menu...                             |                    
| <5> Modify Bootrom Password                              |                    
| <6> Reset Factory Configuration                          |                    
| <0> Reboot                                               |                    
| ---------------------------------------------------------|                    
| Press Ctrl+T to Enter Manufacture Test Menu...           |                    
| Press Ctrl+Z to Enter Diagnose Menu...                   |                    
============================================================ 
Enter your choice(0-6): 5 // 修改 BootROM 密码

Change Password.

Old Password: ********

New Password: *********

Confirm Password: *********

Save new password ... Done.

====================< Extend Main Menu >====================                    
| <1> Boot System                                          |                    
| <2> Set Startup Application Software and Configuration   |                    
| <3> File Management Menu...                              |                    
| <4> Load and Upgrade Menu...                             |                    
| <5> Modify Bootrom Password                              |                    
| <6> Reset Factory Configuration                          |                    
| <0> Reboot                                               |                    
| ---------------------------------------------------------|                    
| Press Ctrl+T to Enter Manufacture Test Menu...           |                    
| Press Ctrl+Z to Enter Diagnose Menu...                   |                    
============================================================
Enter your choice(0-6): 2   //进入设置启动文件和配置文件的子菜单。  

Current boot application software: <hda1:/sup.bin>
Current boot configuration: <hda1:/vrpcfg.zip>
<1> Modify setting                                                              
<0> Quit                                                                        
Enter your choice (0-1): 1   //进入修改配置子菜单。 

File(s) in hda1:                                                                
                                                                                
1:hda1:/sup.bin                                       139720443 bytes 
2:hda1:/vrpcfg.zip                                    17142 bytes        
Total size: 1201569792 bytes.                                                   
Free  size: 1061832207 bytes. 

File(s) in hda2:                                                                
                                                                                
1:hda2:/keylog/log_1389968546.txt                     442185 bytes              
Total size: 640745472 bytes.                                                    
Free  size: 640253952 bytes.                                                    
                                                                                
                                                                                
Input the name of application software(eg: hda1:/sup.bin):   // 留空即可

Input the name of configuration or '.' to clear setting(eg: hda1:/vrpcfg.zip):.   // 清空配置

Modifed configuration successful.                                               
Next boot configuration: NULL                                                   
                                                                                
====================< Extend Main Menu >====================                    
| <1> Boot System                                          |                    
| <2> Set Startup Application Software and Configuration   |                    
| <3> File Management Menu...                              |                    
| <4> Load and Upgrade Menu...                             |                    
| <5> Modify Bootrom Password                              |                    
| <6> Reset Factory Configuration                          |                    
| <0> Reboot                                               |                    
| ---------------------------------------------------------|                    
| Press Ctrl+T to Enter Manufacture Test Menu...           |                    
| Press Ctrl+Z to Enter Diagnose Menu...                   |                    
============================================================                    
Enter your choice(0-6): 1   // 启动
```

启动（实测2分钟左右）后可以拔掉 Console 线，找一根网线插入 MGMT 接口，配置笔记本 IPv4 192.168.0.2 ， 网关为 192.168.0.1 ，掩码 255.255.255.0 ，然后浏览器打开 `192.168.0.1:8443` 。  

输入默认的账号： `admin` ，密码： `Admin@123` 后即可成功登陆 Web 系统。  

# 修改密码
