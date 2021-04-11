---
layout: post
title: VSCode Remote - SSH 配置
---

## 背景

之前如果需要修改服务器上的文件的话，一般都是通过 `ssh`命令 或者登录对应服务器平台进行远程登录操作，Windows 平台虽然也有 **WinSCP** 这样的软件支持，但还是存在每次登录比较繁琐，而且对应的文件也不支持语法高亮。因此 VScode 提供了一个 **Remote SSH** 拓展，支持直接在 VScode 连接远程服务器对文件进行修改，执行命令。

# 服务器配置 SSH 密钥认证(华为云为例)

1. 打开命令终端
2. 执行如下命令生成公私钥对（生成命令过程全选默认）  
   `ssh-keygen -t rsa -b 2048`
3. 找到默认在`C:\Users\username/.ssh`目录下生成的**id_rsa.pub**公钥文件
4. 将该文件上传到服务器`/root/.ssh`(root 用户)目录下，修改文件名为**authorized_keys**，如果存在**authorized_keys**文件，则把公钥文件内容复制到**authorized_keys**文件即可
5. 设置**authorized_keys**文件的权限为 600  
   **chmod 600 /home/操作系统用户名/.ssh/authorized_keys**
6. 查看服务器 ssh 配置文件 **/etc/ssh/sshd_config** 修改以下两个配置  
   **PubkeyAuthentication yes**  
   **RSAAuthentication yes**

# VScode 配置

1. 首先安装 **Remote SSH** 拓展
   ![安装Remote-SSH拓展](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-extendsion.png)
2. 添加 **Remote SSH** 远程服务器配置文件  
   ![打开Remote-SSH拓展](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-icon.png)
3. 配置文件说明  
   ![打开Remote-SSH配置文件](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-config.png)  
   Host: 主机名称，可自定义  
   Hostname: 连接的远程服务器 IP 地址  
   User: 连接的远程服务器用户名  
   Port: 连接的远程服务器端口号（默认 22）
4. 此外还需要修改**Remote-SSH**拓展配置，避免出现连接超时的问题
   1. 将`Config file`的值修改为**3**的配置文件路径
      ![修改Remote-SSH配置文件](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-config-file.png)
   2. 指定远程服务器的平台，`Item`值为**3**配置的主机名称
      ![修改Remote-SSH配置平台](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-config-platform.png)
5. 配置完成后 VScode 左侧 SSH TARGETS 即会生成配置的远程服务器，点击服务器右侧图标，即可打开新窗口连接到远程服务器  
   ![打开Remote-SSH进行连接](/blog/assets/images/VScode-Remote-SSH/Remote-SSH-connect.png)
6. 连接成功的远程服务器示例，左下角表示连接的远程服务器，**Open Folder**可打开指定文件夹，**TERMINAL**可直接使用 linux 命令
   ![Remote-SSH远程服务器窗口示例](/blog/assets/images/VScode-Remote-SSH/Remote-SSH.png)

# 参考文献

1. <a href="https://code.visualstudio.com/docs/remote/ssh-tutorial#_connect-using-ssh" target="_blank">Connect using SSH</a>
2. <a href="https://support.huaweicloud.com/faq-htplugin-kunpengdevps/kunpenghtplugin_10_0003.html" target="_blank">如何配置 SSH 密钥认证</a>
3. <a href="https://zhuanlan.zhihu.com/p/68577071" target="_blank">VS Code Remote SSH 配置</a>
4. <a href="https://zhuanlan.zhihu.com/p/64849549" target="_blank">VSCode Remote 体验 | 远程 Linux 环境开发真香</a>
