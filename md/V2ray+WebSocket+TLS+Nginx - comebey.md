> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.rootfw.com](https://www.rootfw.com/posts/54c2b25c.html)

> Finalshell SSH 连接工具下载 Mobaxterm SSH 连接工具下载 centos7 系统环境搭建，温馨提示：请使用 centos7 系统搭建，如果学习能力强可以 Google 搜索 Debian 或其他系......

[Finalshell SSH 连接工具下载](https://www.hostbuf.com/t/988.html)  
[Mobaxterm SSH 连接工具下载](https://mobaxterm.mobatek.net/download-home-edition.html)

**centos7 系统环境搭建，温馨提示：请使用 centos7 系统搭建，如果学习能力强可以 Google 搜索 Debian 或其他系统搭建方法，只是部分代码不一样而已，V2Ray+WebSocket+TLS+Nginx 前几期视频分别提到过 TLS 和 WebSocket 的配置方法，而本文搭配 Web 服务并同时实现 TLS 和 WebSocket 浏览器域名访问自动跳转 https 有简单前端网页。关于 Web 的软件官方给出 Nginx，Caddy 和 Apache 三个例子，三选一即可**

### [](#安装前准备 "安装前准备")安装前准备

#### [](#更新服务器 "更新服务器")**更新服务器**

```
yum update -y
```

#### [](#更改root密码 "更改root密码")更改 root 密码

[centos debian 服务器 root 密码更改](https://www.rootfw.com/posts/3828c730.html)

#### [](#安装bbr "安装bbr")安装 bbr

```
centos7系统
yum -y install wget
wget "https://raw.githubusercontent.com/ComeBey/rootfw-bbr/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh  

debian系统  请确定服务器已经安装了wget 如没有按照请先执行这条代码 apt-get install wget
wget --no-check-certificate -O tcp.sh https://github.com/cx9208/Linux-NetSpeed/raw/master/tcp.sh && chmod +x tcp.sh && ./tcp.sh
```

### [](#安装v2ray代码 "安装v2ray代码")安装 v2ray 代码

```
bash <(curl -L -s https://install.direct/go.sh)
```

#### [](#安裝和更新V2Ray "安裝和更新V2Ray")安裝和更新 V2Ray

```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

#### [](#安装geoip和geosite "安装geoip和geosite")安装 geoip 和 geosite

install update geoip.dat and geosite.date

```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

#### [](#温馨提示 "温馨提示")温馨提示

卸载 v2ray

```
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```

安装成功后执行 **vi /etc/v2ray/config.json** 长按键盘 D 键删所有代码，在输入键盘 Ins 键进入可编辑状态，复制以下代码后 黏贴到配置中按键盘 Esc 键，在同时按键盘 **shift+：输入 wq 保存退出**

```
//复制以下代码
{
  "log" : {
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log",
    "loglevel": "warning"
  },
  "inbound": {
    "port": 9000, //重点(此端口与nginx配置保持一致)
    "listen": "127.0.0.1", //重点(此端口与nginx配置保持一致)
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "eb950add-608e-409d-937f-e797324387093z", //你的UUID，可更改需与客户端保持一致 
          "level": 1,
          "alterId": 64 //此ID也需与客户端保持一致（不要设置超过100以上可自定义）
        }
      ]
    },
   "streamSettings":{
      "network": "ws", //可设置tcp等
      "wsSettings": {
           "path": "/v2ray" //与nginx配置相关（可自定义）
      }
   }
  },
  "outbound": {
    "protocol": "freedom",
    "settings": {}
  },
  "outboundDetour": [
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "routing": {
    "strategy": "rules",
    "settings": {
      "rules": [
        {
          "type": "field",
          "ip": [
            "0.0.0.0/8",
            "10.0.0.0/8",
            "100.64.0.0/10",
            "127.0.0.0/8",
            "169.254.0.0/16",
            "172.16.0.0/12",
            "192.0.0.0/24",
            "192.0.2.0/24",
            "192.168.0.0/16",
            "198.18.0.0/15",
            "198.51.100.0/24",
            "203.0.113.0/24",
            "::1/128",
            "fc00::/7",
            "fe80::/10"
          ],
          "outboundTag": "blocked"
        }
      ]
    }
  }
}
```

```
你可以输入  service v2ray restart  在输入  service v2ray status -l 
service v2ray start|stop|status|reload|restart|force-reload
```

打开 80（HTTP）和 443（HTTPS）端口 (如果看过我前几期视频，确定防火墙规则设置好了，谷歌云可以忽略。其他品牌服务器请确认已经开启了 80 和 443 端口如果没有可尝试下面代码操作或者服务器网页端  
开启`http80端口`和`https 443端口`)

也可以通过下面的命令来打开这两个端口: 请自行开启防火墙开机启动

```
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

### [](#安装NGINX "安装NGINX")安装 NGINX

```
sudo yum install epel-release


sudo yum install nginx

sudo systemctl enable nginx.service         

sudo systemctl start nginx.service
```

```
调试代码如下：
sudo systemctl start nginx.service                //开启 Nginx
sudo systemctl stop nginx.service                //停止 Nginx
sudo systemctl status -l nginx.service          //查看 Nginx运行状态
sudo systemctl restart nginx.service            //重新启动 Nginx
sudo systemctl disable nginx.service           //取消开机启动 Nginx
sudo systemctl reload nginx.service           //重载Nginx (如更改Nginx配置需要重新载入数据)
sudo systemctl enable nginx.service            //开机启动
     
     
调试代码如下： Nginx调试也可以不需要代码后面添加 **.service** 请先**sudo systemctl start nginx.service**
和**sudo systemctl enable nginx.service** 然后在通过下面代码也可以调试.

sudo systemctl start nginx              //开启 Nginx
sudo systemctl stop nginx              //停止 Nginx
sudo systemctl status -l nginx        //查看 Nginx运行状态
sudo systemctl restart nginx             //重新启动 Nginx
sudo systemctl disable nginx           //取消开机启动 Nginx
sudo systemctl reload nginx          //重载Nginx (如更改Nginx配置需要重新载入数据)
sudo systemctl enable nginx          //开机启动
```

通过以上方式安装的 Nginx，所有相关的配置文件都在 /etc/nginx/ 目录中  
**Nginx 的主配置文件是 /etc/nginx/nginx.conf**  
**Nginx 日志文件（access.log 和 error.log ）位于 /var/log/nginx/ 目录中。**

如果在设置完成之后不能成功使用，可能是由于 SElinux 机制 (如果你是 CentOS 7 的用户请特别留意 SElinux 这一机制) 阻止了 Nginx 转发向内网的数据  
如果是这样的话，在 V2Ray 的日志里不会有访问信息，在 Nginx 的日志里会出现大量的 “Permission Denied” 字段  
要解决这一问题需要在终端下键入以下命令：

```
setsebool -P httpd_can_network_connect 1    //debian忽略
```

验证 Nginx 是否成功启动，可以在浏览器中打开 [http://YOUR_IP](http://your_ip/) 注意：打开则显示 centos 网页 但是提示不安全网站没开启 ssl 加密 https

### [](#生成证书 "生成证书")生成证书

**如果你已经有其他证书可忽略，把证书和密钥放到服务器指定目录下，只需在 Nginx 中指定 证书和密钥路径。申请证书方法太多可通过安装 acme.sh 工具生成证书或其他方法生成证书，可 Google 搜索。**

**1. 安装 acem.sh 证书生成工具，以下提供 3 种方法安装，选其中任意一种方法安装证书工具 (温馨提示：自动升级 acme.sh 在 root 下输入 acme.sh upgrade)**

```
curl  https://get.acme.sh | sh   // 如提示安装失败 请先安装curl 输入 yum -y install curl
wget -O -  https://get.acme.sh | sh     //如提示安装失败请（先安装wget）输入 yum -y install wget 已经安装了忽略
git clone https://github.com/acmesh-official/acme.sh.git   // 如提示安装失败 先安装git 已经安装了的忽略 输入 yum install git
cd ./acme.sh
./acme.sh --install
```

通过以上代码安装 acme.sh 提示红色抱错 你可以按实际相关情况而定安装依赖 比如安装 socat 或者 netcat  

```
centos7  yum install openssl
centos7  yum install socat  
centos7  yum isntall netcat

debian  apt-get install openssl cron socat curl
debian  apt-get -y install netcat
debian  apt-get -y install socat 

安装成功后执行 source ~/.bashrc  以确保脚本所设置的命令别名生效
```

**2. 生成证书 路径为 / root/.acme.sh 文件下 安装好后可自行查看**  
**温馨提示：通过 acme.sh 生成证书有多种方法：**  
例如—自动 DNS API 集成 如：cloudflare DNS API 令牌 和 使用全局 API 密钥 acme.sh 支持大多数 dns 生成证书  
例如—使用 DNS 手动模式，等多种其他安装方法，如果你是个好学的人可 Google

生成证书如下：本期视频只用指定端口 生成证书 **推荐使用 443 端口生成证书** （一般用单域名足以，毕竟是翻墙用无需搞那么多花里胡哨的多域，比如：主域 baidu.com 那么不建议使用 [www.baidu.com, 因为是翻墙的前端 web 请自定义比如 tw.baidu.com, 前缀 tw 可以自定义请不要写太长，主域 baidu.com 和二级 www.baidu.com 可以备用你懂的）](http://www.baidu.xn--com%2Cwebtw-m68qggk76c2vqwpgl9dvxmpo6c6uxaoy3c7ku6q0awjk3k7d.baidu.xn--com%2Ctw%2Cbaidu-xd7vlwlh07hz7tymd44l1mxpzg1lqzw9sk3pa8k2c7no3g9d.xn--comwww-dg8ir32b1l6g.baidu.xn--com%29-d65fnzq1wdsk0xt2k8bped/)

**1. 通过侦听 80 端口申请证书，如果 80 端口被占用, 请使用 443 端口, 请确保这些端口都打开了**

```
sudo ~/.acme.sh/acme.sh --issue -d 域名 --standalone -k ec-256
```

**2. 如果您 80 在反向代理或负载均衡器后面使用非标准端口，则可以–httpport 用来指定端口**

```
sudo ~/.acme.sh/acme.sh --issue -d 域名 --standalone --httpport 端口
```

**3. 侦听 443 端口以颁发证书，请确保 443 端口开启**

```
sudo ~/.acme.sh/acme.sh --issue -d 域名 --alpn -k ec-256
```

**4. 如果您 443 在反向代理或负载均衡器后面使用非标准端口，则可以–tlsport 用来指定端口**

```
sudo ~/.acme.sh/acme.sh --issue -d 域名 --alpn --tlsport 端口
```

**-k 表示密钥长度，后面的值可以是 ec-256 、ec-384、2048、3072、4096、8192，带有 ec 表示生成的是 ECC 证书，没有则是 RSA 证书。在安全性上 256 位的 ECC 证书等同于 3072 位的 RSA 证书**

**把证书和密钥安装移到指定路径 /etc/v2ray （路径可自定义）**

```
ecc 安装代码 sudo ~/.acme.sh/acme.sh --installcert -d 域名 --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key --ecc

rsa 安装代码 sudo ~/.acme.sh/acme.sh --installcert -d 域名 --fullchainpath /etc/v2ray/v2ray.crt --keypath /etc/v2ray/v2ray.key
```

### [](#配置nginx-conf "配置nginx.conf")配置 nginx.conf

**vi /etc/nginx/nginx.conf #进入编辑配置文件：长按键盘上 D 键删除所有配置信息，再按键盘 Ins 键进入编辑模式复制如下代码黏贴到配置中编辑，填写对应自己的配置信息后。最后同时按键盘上 shift+: 键，在输入 wq 保存退出**

```
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;


events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

   
    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include  /etc/nginx/mime.types;
    default_type application/octet-stream;
    include /etc/nginx/conf.d/*.conf;

server {
    listen 80;
    server_name 域名;
    rewrite ^/(.*) https://域名$1 permanent; 
}

    server 
	{ 
    
    listen 443 ssl http2 default_server;
    listen [::]:443 ssl http2 default_server;
    ssl_certificate /路径/*.pem; 
    ssl_certificate_key /路径/*.key; 
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;  
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    root /usr/share/nginx/html; 
    server_name 域名; 
 
    location /ray { 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:端口; 
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $http_host;
}

}


}
```

**套件如下：**

```
ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;   #RSA套件
```

```
EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;
```

**修改 Nginx 配置后，请务必重新加载配置 输入 sudo systemctl reload nginx.service #必须这样操作**

查询是否开启 ssl 打开网站 [https://myssl.com/ssl.html 输入自己域名，端口输入 443](https://myssl.com/ssl.html%E8%BE%93%E5%85%A5%E8%87%AA%E5%B7%B1%E5%9F%9F%E5%90%8D%EF%BC%8C%E7%AB%AF%E5%8F%A3%E8%BE%93%E5%85%A5443) 或者 打开 [https://www.ssllabs.com/ssltest/index.html](https://www.ssllabs.com/ssltest/index.html) 查询 ，也可以 google 搜索关键字 ssl 查询，很多网站可以查询，评分是否到达 A 级别

**温馨提示：通过 acme.sh 工具生成证书请使用 443 端口 代码如下：sudo ~/.acme.sh/acme.sh –issue -d 域名 –alpn -k ec-256 （侦听 443 端口以颁发证书，请确保 443 端口开启 按文章流程操作 80 端口生成证书时会提示端口占用，当然如果你想简单点可以通过已有购买的证书或者通过宝塔申请的或者其他证书，把密钥和证书直接复制拷贝到服务器指定路径下，在配置 nginx 指向证书和路径。部分免费证书会缺少 CA 证书那么在网站 ssl 评分就会大打折扣，解决办法是重新把 CA 证书添加下就可以了**

**通过在浏览器输入自己域名查询是否发现网站加了一把锁🔒开启了 https， 且网站域名 ssl 评分到达 A 如果是，恭喜你搭建成功. 如果你已经学会了那么接下来的课程会越来越有意思.**

### [](#电报群讨论 "电报群讨论")电报群讨论