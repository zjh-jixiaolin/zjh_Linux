# Linux 端口与进程管理

[TOC]

文档：[link](http://c.biancheng.net/view/705.html)

## 0. 端口

端口，是设备与外界通讯的出入口。端口可分为两种：**物理端口**和**虚拟端口**。

- 物理端口：又称为接口，是可见的端口，如电脑 `USB`接口、`RJ45`网口、`HDMI`端口等。

  ![image-20230517010110847](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305170124115.png)

- 虚拟端口：指计算机内部的端口，是不可见的，是操作系统用来与外部进行交互使用的。

  ![image-20230517010419579](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305170124716.png)

### 虚拟端口

**物理端口**比较好理解，而 **虚拟端口** 的定义很模糊，如虚拟端口有什么作用，为什么需要它？如下举例：

- 当两台计算机进行通讯，计算机`A` —> 计算机`B`。通过 `IP`地址即可找到对应主机

  ![image-20230517010835384](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305170124931.png)

- 两台计算机上都运行着各种的程序，当计算机 `A` 的微信程序 —>计算机`B` 的微信程序进行通讯，如果只通过`IP` 地址会显得不够精确，因为`IP`地址只能代表网络中的一台计算机，它锁定不了计算机中某个具体程序。这种问题可以通过 **虚拟端口** 去解决，如下图，计算机 `A` 通过端口`50001`去发出访问请求到计算机 `B` 的端口`5678`，由此可发现，通过访问端口就等同于访问计算机上的微信程序。

  ![image-20230517011146889](https://raw.githubusercontent.com/zjh-jixiaolin/map_strong/main/202305170124823.png)































