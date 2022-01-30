> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.v2rayssr.com](https://www.v2rayssr.com/v2raybaota.html)

> 前言 还是有不少的朋友们问我，说是装了 V2RAY+Nginx+Ws+Tls 就不能自己的博客了。

前言
--

还是有不少的朋友们问我，说是装了 V2RAY+Nginx+Ws+Tls 就不能自己的博客了。非也非也！V2RAY 其实和你的博客（WordPress）是不冲突的。只要的 VPS 性能可以达到，这些要求实现起来也是不难，反倒是降低了 V2RAY 的使用门槛，让配置只会更加的简单方便！

废话还是不多说，开始我们的表演。请配合 youtube 教程观看。

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122804252475.png)

教程地址：[点击观看](https://www.v2rayssr.com/go?url=https://youtu.be/tk22q_jtYPg)

对于 VPS 的要求
----------

既然你有那么多的需求，那么我相信你的 VPS 性能肯定是差不了。内存少于 512M 的就别再折腾了。按照波仔上期的教程，搭建一个 V2RAY+Ws+Tls 就行了。别幻想其他！！！推荐 1G 内存（若你还打算安装 SQL 的话。）

准备好你的域名
-------

我这边还是使用免费的域名，有很多朋友们说 CDN 很鸡肋，也是，所以这期就直接使用你们域名提供商提供的 DNS 直接解析域名。（其实上期申请 CDN 仅仅只是为了做一个 SSL 证书而已，其实有很多地方可以申请 SSL 证书，波仔 youtube 里面有专门 SSL 证书申请的视频。[https://youtu.be/FsFuE9SVP8w](https://www.v2rayssr.com/go?url=https://youtu.be/FsFuE9SVP8w)）

申请地址：[https://freenom.com](https://www.v2rayssr.com/go?url=https://freenom.com) （若你是申请不了免费的，那么请移步下面）

收费申请地址：[https://www.namesilo.com](https://www.v2rayssr.com/go?url=https://www.namesilo.com/?rid=6254266mw)    (随便申请一个年付 0.99 / 美元的域名，支付宝支付)

做好域名的解析，增加两个解析，一个泛解析，一个 www 的解析。有的域名提供商泛解析需要在主机名那里填入 @，但是也有一些域名提供商主机名留空。

下面就开始 VPS 上面的操作

更改 VPS 系统时间（可选开启时间同步）
---------------------

时间同步对于 SSR 也许无所谓，但是对于 V2RAY 很重要。是非常重要。下面的时间同步可以选择跳过，时间改好就可以了。

```
rm -rf /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

```

### NTP 同步时间 协议（可选择跳过）

众所周知，NTP 协议是网络时间同步协议，有了它，我们可以很轻松的同步本地时间与互联网时间。VPS 上也可以使用 NTP 来同步网络。首先安装必要的软件包：

#### Ubuntu/Debian 系统

```
apt-get update
apt-get install ntp ntpdate -y

```

#### CentOS/RHEL 系统

```
yum install ntp ntpdate -y

```

### 接下来我们需要先停止 NTP 服务器，再更新时间。

```
service ntpd stop                 #停止ntp服务
ntpdate us.pool.ntp.org           #同步ntp时间
service ntpd start                #启动ntp服务

```

执行完成后，VPS 上就是相对精确的时间设置了。很多依赖于系统时间的应用程序也就能正常工作了。

Debian 安装宝塔面板
-------------

BT 面板官方安装脚本：（Debian 系统）

```
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && bash install.sh

```

Centos 安装

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh

```

如下图就安装成功了。

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122208074251.png)

根据上面提示的地址和密码登录你的宝塔面板  
安装 Nginx/Sql / 或是其他你需要的运行环境软件

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122208083113.png)

因为有时候 debian 不能急速安装，一般是编译安装，所以速度慢的奇葩！！若是真心是建站需求的话，推荐使用 CENTOS7 以上的系统，那样安装运行环境很急速的！一般 10 分钟内全部搞定

开启 DEBIAN9 BBR
--------------

```
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
sysctl net.ipv4.tcp_available_congestion_control
lsmod | grep bbr

```

安装 v2ray 服务器：官方脚本
-----------------

```
bash <(curl -L -s https://install.direct/go.sh)

```

如果提示 curl: command not found ，那是因为你的 VPS 没装 Curl  
ubuntu/debian 系统安装 Curl 方法

```
apt-get update -y && apt-get install curl -y 

```

centos 系统安装 Curl 方法

```
yum update -y && yum install curl -y

```

`vi /etc/v2ray/config.json` V2RAY 服务器的配置文件如下：（下面代码可以直接覆盖源文件代码）

```
{
  "inbounds": [{
    "port": 65432,           //此处为安装时生成的端口，可修改随意，但是保证和下面提到的端口号相同
    "listen":"127.0.0.1",
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "xxxxxxxxx", //此处为安装时生成的id
          "level": 1,
          "alterId": 64      //此处为安装时生成的alterId
        }
      ]
    },
    "streamSettings": {
      "network": "ws",
      "wsSettings": {
        "path": "/SoftDown"   //此处为路径，需要和下面NGINX上面的路径配置一样
      }
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  },{
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}

```

设置为开机自动启动

```
systemctl enable v2ray

```

启动 v2ray 服务

```
systemctl start v2ray

```

自动签发 SSL 证书，并强制开启 HTTPS  
![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122208092464.png)

配置站点的 nginx

```
location /SoftDown {
proxy_redirect off;
proxy_pass http://127.0.0.1:65432;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $http_host;
}

```