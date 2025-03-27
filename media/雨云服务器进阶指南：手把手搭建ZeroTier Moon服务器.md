# 雨云服务器进阶指南：手把手搭建ZeroTier Moon服务器

## 前言

在完成技术验证测试后，我发现闲置的雨云服务器还有更多潜力可挖。随着IPv4地址资源日益枯竭，公网IP获取难度加大，虚拟局域网(VPN)成为理想替代方案。本文将重点介绍ZeroTier Moon服务器的搭建方法，为后续的虚拟局域网应用打下基础。

> 提示：部分命令需要根据您的实际环境进行调整补充。

## 一、准备工作

### 1.1 云服务器选购指南

**搭建Moon服务器的首要条件是拥有一台具备公网IP的云服务器**，推荐选择配置适中的方案：

- 基础配置：2核CPU + 2GB内存
- 带宽选择：根据需求选择高带宽或不限流量套餐
- 机房选择：国内用户建议选择内地机房，访问速度更优

👉 [【点击查看】2025年最新雨云优惠码及特价云服务器方案汇总](https://bit.ly/RainYun)

### 1.2 系统环境配置

推荐使用Debian 11系统：
1. 登录雨云控制台
2. 进入"云服务器"管理界面
3. 选择Debian 11系统镜像
4. 完成购买后等待实例启动

## 二、Moon服务器搭建全流程

### 2.1 连接服务器

使用SSH连接服务器（Windows用户可使用内置终端）：
bash
ssh 用户名@服务器IP -p 22

### 2.2 ZeroTier安装

执行一键安装命令：
bash
curl -s https://install.zerotier.com/ | sudo bash

### 2.3 配置文件准备

1. 创建专用目录：
bash
mkdir /var/lib/zerotier-one/moons.d

2. 生成moon配置文件：
bash
cd /var/lib/zerotier-one/
zerotier-idtool initmoon identity.public >> moon.json

3. 编辑配置文件（使用nano或vim）：
bash
nano moon.json

- 记录id字段
- 修改stableEndpoints为"服务器IP/9993"

4. 生成并移动moon文件：
bash
zerotier-idtool genmoon moon.json
cp -a 生成的文件名 moons.d/

5. 重启服务：
bash
systemctl restart zerotier-one.service

## 三、客户端接入指南

### 3.1 Windows系统接入

1. 以管理员身份运行PowerShell
2. 执行接入命令：
bash
zerotier-cli.bat orbit moonID moonID

3. 验证连接：
bash
zerotier-cli listpeers

### 3.2 Linux系统接入

bash
sudo zerotier-cli orbit moonID moonID
zerotier-cli listpeers

> 注意：Docker环境可能需要在容器内执行上述命令

## 四、总结

通过本文的详细步骤，您已成功在雨云服务器上搭建了ZeroTier Moon节点。这个中继服务器将显著提升您的虚拟局域网连接质量。后续我们将深入探讨ZeroTier的实际应用技巧，敬请期待！