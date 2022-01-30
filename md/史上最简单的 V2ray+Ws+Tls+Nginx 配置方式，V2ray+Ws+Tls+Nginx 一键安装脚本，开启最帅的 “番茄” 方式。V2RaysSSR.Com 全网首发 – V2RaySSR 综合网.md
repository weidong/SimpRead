> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.v2rayssr.com](https://www.v2rayssr.com/easyv2ray.html)

> 前言 已经出了几期关于 V2ray+Ws+Tls 配置的教程了，但是大伙儿还是有很多人不明白，其实这个一键安装脚本早就在江湖中流传了，但是由于一键安装有时出现问题，大伙儿更加的不知道如何下手，并且有时候知…......

前言
--

已经出了几期关于 V2ray+Ws+Tls 配置的教程了，但是大伙儿还是有很多人不明白，其实这个一键安装脚本早就在江湖中流传了，但是由于一键安装有时出现问题，大伙儿更加的不知道如何下手，并且有时候知道问题所在也无法解决。 但是很多人强烈要求一键安装，省心省力。好吧，那么[波仔](https://www.v2rayssr.com/go?url=https://bozai.cc)给大家带来了代码。 其实对于你的 VPS IP 被墙，V2RAY 是可以继续使用 VPS 的，套用一个免费的 CDN 就可以了。

你按照下面的方法配置你的 V2RAY，不同的就是你的域名 DNS 需要托管在 [cloudflare](https://www.v2rayssr.com/go?url=https://www.cloudflare.com/)，其他方法是一模一样，打开小云朵，那么你被墙的 VPS 就可以继续它本质的工作了。

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122804115765.png)

本文章视频发布地址：[点击观看](https://www.v2rayssr.com/go?url=https://youtu.be/SbRWnlqSatU)

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122207070286.png)

服务器要求
-----

你需要安装 V2RAY，你还需要安装 NGINX 来代理，你还需要 VPS 速度很快，那么我相信你的 VPS 配置一定差不了，512M 有吧！！！没有？？那我觉得这个方案还是不怎么适合你。当然你要勉强也不是不可以，对吧？折腾呗！

测试的系统
-----

我这边是用谷歌云 - 香港 - Debian 9 来为大家演示的，相信折腾的人，都是这个系统。当然也有挺多人用的 CentOS 系统，个人觉得 CentOS 系统稳定，但是系统占用太高了，大家自行斟酌吧。

准备工作
----

准备好你的域名：

申请地址：[https://freenom.com](https://www.v2rayssr.com/go?url=https://freenom.com) （若你是申请不了免费的，那么请移步下面）

收费申请地址：[https://www.namesilo.com](https://www.v2rayssr.com/go?url=https://www.namesilo.com/?rid=6254266mw)   (随便申请一个年付 0.99 / 美元的域名，支付宝支付)

解析好你的域名（指向 VPS IP），波仔 (bozai.cc) 推荐你使用二级域名（XXX.XXX.COM） 若不懂二级域名为何物，请移步视频区：[点击观看](https://www.v2rayssr.com/go?url=https://youtu.be/SbRWnlqSatU)

代码集合
----

一键安装代码:

```
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log

```

BBR 加速代码: BBR 加速：

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh

```

如果安装不了 BBR 请先运行以下代码：

```
yum -y install wget

```

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122207072243.png)

检查配置
----

看看是否能够正常访问你刚才在命令行中执行的域名，看看是否开启 HTTPS 访问，看看伪装站点是否能够打开。伪装的站点是一个元素周期表，当然你可以自己换掉。

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122207073499.png)

访问你的域名，看见这个界面，代表着搭建全部成功。请开始你的表演！！！！！

![](https://www.v2rayssr.com/wp-content/uploads/2019/12/2019122207074698.png)