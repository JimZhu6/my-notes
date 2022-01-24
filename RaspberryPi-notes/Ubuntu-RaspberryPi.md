# Ubuntu安装指南

## 安装

根据Ubuntu[官网介绍](https://ubuntu.com/tutorials/how-to-install-ubuntu-desktop-on-raspberry-pi-4#1-overview)，准备一张SD卡，通过下载Raspberry Pi[专用镜像安装器](https://downloads.raspberrypi.org/imager/imager_latest.exe)刷入指定系统。

## 登录

把烧录好镜像的SD卡插入树莓派，通过网线连接路由器，获取到ip后登录树莓派。默认用户名和密码都是ubuntu，第一次登陆后，需要修改默认密码，修改后重新登录。

## 使用frp做映射

[前往下载](https://github.com/fatedier/frp/releases)最新版本的客户端和服务端二进制文件。frps需部署到有公网的服务器端，frpc部署到内网端。按以下教程配置

### 通过 SSH 访问内网机器

这个示例通过简单配置 TCP 类型的代理让用户访问到内网的服务器。

1. 在具有公网 IP 的机器上部署 frps，修改 frps.ini 文件，这里使用了最简化的配置，设置了 frp 服务器用户接收客户端连接的端口：

   ```ini
   [common]
   bind_port = 7000
   ```

2. 在需要被访问的内网机器上（SSH 服务通常监听在 22 端口）部署 frpc，修改 frpc.ini 文件，假设 frps 所在服务器的公网 IP 为 x.x.x.x：

   ```ini
   [common]
   server_addr = x.x.x.x
   server_port = 7000
   
   [ssh]
   type = tcp
   local_ip = 127.0.0.1
   local_port = 22
   remote_port = 6000
   ```

   `local_ip` 和 `local_port` 配置为本地需要暴露到公网的服务地址和端口。`remote_port` 表示在 frp 服务端监听的端口，访问此端口的流量将会被转发到本地服务对应的端口。

3. 分别启动 frps 和 frpc。

4. 通过 SSH 访问内网机器，假设用户名为 test：

   `ssh -oPort=6000 test@x.x.x.x`

   frp 会将请求 `x.x.x.x:6000` 的流量转发到内网机器的 22 端口。

### 安全地暴露内网服务

这个示例将会创建一个只有自己能访问到的 SSH 服务代理。

对于某些服务来说如果直接暴露于公网上将会存在安全隐患。

使用 `stcp(secret tcp)` 类型的代理可以避免让任何人都能访问到要穿透的服务，但是访问者也需要运行另外一个 frpc 客户端。

1. frps.ini 内容如下：

   ```ini
   [common]
   bind_port = 7000
   ```

2. 在需要暴露到内网的机器上部署 frpc，且配置如下：

   ```ini
   [common]
   server_addr = x.x.x.x
   server_port = 7000
   
   [secret_ssh]
   type = stcp
   # 只有 sk 一致的用户才能访问到此服务
   sk = abcdefg
   local_ip = 127.0.0.1
   local_port = 22
   ```

3. 在想要访问内网服务的机器上也部署 frpc，且配置如下：

   ```ini
   [common]
   server_addr = x.x.x.x
   server_port = 7000
   
   [secret_ssh_visitor]
   type = stcp
   # stcp 的访问者
   role = visitor
   # 要访问的 stcp 代理的名字
   server_name = secret_ssh
   sk = abcdefg
   # 绑定本地端口用于访问 SSH 服务
   bind_addr = 127.0.0.1
   bind_port = 6000
   ```

4. 通过 SSH 访问内网机器，假设用户名为 test：

   `ssh -oPort=6000 test@127.0.0.1`
   
   
   
### 开机自启并在后台运行

在 /usr/lib/systemd/system 下面新建一个文件frps.service，文件内容如下：

```bash
[Unit]
Description=frps daemon

[Service]
Type=simple
#此处把/root/frp_linux_arm64替换成 你的frps的实际安装目录
ExecStart=/home/ubuntu/frp/frpc -c /home/ubuntu/frp/frpc.ini

[Install]
WantedBy=multi-user.target
```

- 启动frp `sudo systemctl start frpc`
- 打开自启动 `sudo systemctl enable frpc`
- 重启应用 `sudo systemctl restart frpc`
- 停止应用 `sudo systemctl stop frpc`
- 查看日志 `sudo systemctl status frpc`



## 安装Docker

输入下面命令进行安装

```bash
sudo apt install docker.io
```

查看版本

```bash
docker version
```

启动docker server，需要切换为root用户启动

```bash
service docker start
```

设置开机自启

```bash
systemctl start docker
systemctl enable docker
systemctl start docker.service
systemctl enable docker.service
```

修改源，修改后需要重启服务

```bash
# vi /etc/docker/daemon.json
{
    "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

测试docker

```bash
docker pull hello-world
docker run hello-world
```


