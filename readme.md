# AShare 
###### 一款阿里云多账户直链解析程序
###### 支持绑定多个账号，分享单加密目录，分享单目录，批量获取文件夹内容直链，获取单文件直链，在线预览等

## 准备工作

1. 装了宝塔的linux服务器一台
2. 在网页上登录好你的阿里云盘账号

### 第一步-获取阿里云盘的refresh_token

```
#按下f12
#复制下面的代码，直接到控制台执行

JSON.parse(localStorage.getItem('token')).refresh_token

#出来的结果就是refresh_token
```




### 第二步-去你的宝塔新建一个站点

域名设置一个你自己的域名，php版本选择纯静态




### 第三步-到站点 /usr/local/bin 目录，上传直链程序


```
#直链程序下载地址：

#win版本下载地址：

https://one.blob.core.chinacloudapi.cn/video/AShare.exe


#linux版本下载地址：

https://media.cooluc.com/source/AShare/AShare_linux_amd64

```

上传好后设置直链程序的权限


### 第四步-到你服务器的 /usr/lib/systemd/system 目录，新增启动文件

文件名为：AShare.service

文件内容如下：

```
[Unit]
Description=AShare service
Wants=network.target
After=network.target network.service

[Service]
Type=simple
WorkingDirectory=/opt/AShare
ExecStart=/opt/AShare/AShare_linux_amd64
KillMode=proces

[Install]
WantedBy=multi-user.target
```


### 第五步-启动服务
systemctl start AShare   # 启动程序
systemctl enable AShare  # 开机启动
systemctl status AShare  # 查看运行状态以及显示默认账户密码

如图，可以看到我程序运行的地址和账号密码

再次进入第二步添加的站点设置页，添加反向代理
代理就是就是上图看到的服务运行地址，在我这里就是
http://127.0.0.1:5201


### 第六步-打开系统，根据上面安装的账号密码登录系统，开始新增阿里云盘账号

点击新增账号，填入你第一步获取的refresh_token，点击确定即可



第二种安装方式
下载程序二进制：

mkdir -p /opt/AShare/
wget -O /opt/AShare/AShare_linux_amd64 https://media.cooluc.com/source/AShare/AShare_linux_amd64
chmod 0755 /opt/AShare/AShare_linux_amd64
创建 systemd 启动脚本（不要迷惑，就是直接复制粘贴到终端）：

cat >/lib/systemd/system/AShare.service <<EOF
[Unit]
Description=AShare service
Wants=network.target
After=network.target network.service

[Service]
Type=simple
WorkingDirectory=/opt/AShare
ExecStart=/opt/AShare/AShare_linux_amd64
KillMode=process

[Install]
WantedBy=multi-user.target
EOF
启动进程 & 开机自启

systemctl start AShare   # 启动程序
systemctl enable AShare  # 开机启动
systemctl status AShare  # 查看运行状态以及显示默认账户密码


