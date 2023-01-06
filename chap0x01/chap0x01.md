# 基于 VirtualBox 的网络攻防基础环境搭建

## 实验目的

- 掌握 VirtualBox 虚拟机的安装与使用；
- 掌握 VirtualBox 的虚拟网络类型和按需配置；
- 掌握 VirtualBox 的虚拟硬盘多重加载；

## 实验环境

以下是本次实验需要使用的网络节点说明和主要软件举例：

- VirtualBox 虚拟机
- 攻击者主机（Attacker）：Kali Rolling 2019.2
- 网关（Gateway, GW）：Debian Buster
- 靶机（Victim）：win7 / Debian / Kali

## 实验要求

- 虚拟硬盘配置成多重加载，效果如下图所示；

![img](https://c4pr1c3.github.io/cuc-ns/chap0x01/attach/chap0x01/media/vb-multi-attach.png)

- 搭建满足如下拓扑图所示的虚拟机网络拓扑；

![img](https://c4pr1c3.github.io/cuc-ns/chap0x01/attach/chap0x01/media/vb-exp-layout.png)

> 根据实验宿主机的性能条件，可以适度精简靶机数量

- 完成以下网络连通性测试；
  - [x] 靶机可以直接访问攻击者主机
  - [x] 攻击者主机无法直接访问靶机
  - [x] 网关可以直接访问攻击者主机和靶机
  - [x] 靶机的所有对外上下行流量必须经过网关
  - [x] 所有节点均可以访问互联网

## 实验过程

- #### 将虚拟硬盘配置成多重加载

  - 在VirtualBox上方的管理中，选择虚拟介质管理

    ![](.\img\Multi-load configuration choice.png)

  - 选中需要的虚拟盘，在属性处将类型修改为多个加载并应用。

    ![](.\img\Multi-load configuration.png)

  - 结果

    ![](.\img\Multi-load configuration result.png)

- #### 搭建虚拟机网络拓扑

  - 在每台虚拟机的设置中对网卡进行配置

    - 网关网卡配置（4块网卡）如图：

      ![](.\img\gateways.png)

    - 攻击者网卡配置（3块卡）如图

      ![](.\img\attacker.png)

    - 受害主机都仅需一张网卡

    - 将受害主机分为两组，两个局域网中

      - Victim-XP-1和Victim-Kali-1在Intnet1
      - Victim-XP-2和Victim-Debian-2在Intnet2

      ![](.\img\XP-victim1.png)

      ![](.\img\kali-victim-1.png)

      ![](.\img\XP-victim2.png)

      ![](.\img\Debian-victim2.png)

- #### 测试网络连通性

  - 查询各主机的IP

    ![](.\img\attacker IP.png)

    ![](.\img\kali-victim-1 IP.png)

    ![](.\img\XP-victim1-IP.png)

    ![](.\img\Debian-victim2 IP.png)

    ![](.\img\XP-victim2-IP.png)

  - 靶机可以直接访问攻击者主机

    - 局域网1内的靶机访问

      ![](.\img\attacker-intnet1.png)

    - 局域网2内的靶机访问

      ![](.\img\attacker-intnet2.png)

  - 攻击者主机无法直接访问靶机

    - 访问局域网1内的靶机

      ![](.\img\intnet1-attacker.png)

    - 访问局域网2内的靶机

      ![](.\img\intnet2-attacker.png)

  - 网关可以直接访问攻击者主机和靶机

    - 访问攻击者主机

      ![](.\img\attacker-gateway.png)

    - 访问局域网1内的靶机

      ![](.\img\intnet1-gateway.png)

    - 访问局域网2内的靶机

      ![](.\img\intnet2-gateway.png)

  - 靶机的所有对外上下行流量必须经过网关

    - 通过观察可以发现，靶机的网关ip 被设置为Gateway enp0 的ip，靶机本身不能上网，靶机只能经由网关对外访问。

      ![](.\img\Gateway IP.png)

    - 关闭网关后不再能访问互联网，且靶机的ip地址是169开头的，不是原来的172，是自动获取的

      ![](.\img\uninvited.png)

  - 所有结点均可以访问互联网

    - 网关

      ![](.\img\Internet-gateway.png)

    - 攻击者主机

      ![](.\img\Internet-attacker.png)

    - 局域网1

      ![](.\img\Internet-Kali1.png)

      ![Internet-XP1](.\img\Internet-XP1.png)

    - 局域网2

      ![](.\img\Internet-Debian2.png)

      ![Internet-XP2](.\img\Internet-XP2.png)

#### 实验中出现的问题

- 网关无法访问局域网1中的Windows XP

  查询资料得可能是XP的防火墙所致，关闭防火墙即可

  ![](.\img\error1.png)

  ![](.\img\error1_1.png)