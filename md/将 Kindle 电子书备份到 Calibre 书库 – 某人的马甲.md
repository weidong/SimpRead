> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [maajiaa.wordpress.com](https://maajiaa.wordpress.com/2018/07/19/kindle-to-calibre/)

> 我只想保证我买过的书不会被消失，不鼓励传播盗版书！ 搜了一圈，最好的备份 kindle 电子书的方案还是 Ca…

我只想保证[我买过的书不会被消失](https://maajiaa.wordpress.com/2018/07/19/purchased-kindle-ebook-disappeared/)，**不鼓励传播盗版书！**

搜了一圈，最好的备份 kindle 电子书的方案还是 Calibre + DeDRM 插件 + KFX input 插件。网上教程很多，以下是我自己的总结笔记。

### 一、导入电子书的来源限制

Kindle 有不同的设备不同平台的客户端软件，有的可以成功解密并导入，有的暂不支持。研究了一整晚，目前基本确认的支持情况如下：

##### 1. 从 kindle 设备导入

*   **支持，也是目前看来最便捷的途径。**
*   不支持 Windows 应用商店版本的 Kindle for PC，请从亚马逊官网或其他网站直接下载安装。

##### 3. Kindle for Android/iOS

*   不支持。网上有些帖子说支持安卓版，说的是很久以前的老版本。新的安卓 app 已经不能通过 backup 方式拿到解密 key 了。

### 二、工具准备

1.  **Calibre：**一款开源跨平台的电子书管理软件，也可转换电子书格式，有免安装 portable 版本  
    [https://calibre-ebook.com/download](https://calibre-ebook.com/download)
2.  **DeDRM 插件：**去除各种电子书的 DRM 版权保护，目前最新版是 v6.6.1  
    [https://github.com/apprenticeharper/DeDRM_tools/releases](https://github.com/apprenticeharper/DeDRM_tools/releases)
3.  **KFX Input 插件：**支持 kindle 新版 KFX 格式的插件，在 calibre 的官方插件库里就能找到，也可以从这里下载 zip 包：  
    [https://www.mobileread.com/forums/showthread.php?t=291290](https://www.mobileread.com/forums/showthread.php?t=291290)

### 三、安装和设置 DeDRM 插件

运行 Calibre 后，找到「**偏好选项**」->「**高级选项**」->「**插件**」[![](https://maajiaa.files.wordpress.com/2018/07/calibre-plugin-settings-1.png?w=828&h=432)](https://maajiaa.files.wordpress.com/2018/07/calibre-plugin-settings-1.png)

然后选择「**获取新插件**」，可以从官方插件库直接安装 KFX Input，搜索「KFX Input」并安装即可。选择「**从文件加载插件**」，则可以安装下载回来的 [DeDRM 插件](https://github.com/apprenticeharper/DeDRM_tools/releases)。注意此处仅仅需要解压缩后在 **DeDRM_calibre_plugin 目录**下的 **DeDRM_plugin.zip** 文件，不是下载回来的整个 zip 包。[![](https://maajiaa.files.wordpress.com/2018/07/calibre-plugin-settings-2.png?w=828&h=432)](https://maajiaa.files.wordpress.com/2018/07/calibre-plugin-settings-2.png)

安装好插件后，重启 Calibre，回到插件设置界面，在「文件类型插件」分类下可以找到 DeDRM，如果找不到可以直接搜索「DeDRM」。选中插件，右键单击或点击底下的「**自定义插件**」：[![](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-1.png?w=828&h=432)](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-1.png)

接下来需要设置电子书的解密 key，我设置了 eInk Kindle 和 Kindle for PC 两个。[![](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-2.png?w=112&h=150)](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-2.png)

eInk Kindle 需要点右上角绿色的「＋」号添加 **Kindle 设备的序列号**。登录亚马逊网站，在「管理我的内容和设备」这里选中对应的设备就能查看。Kindle 设备「设置」->「设备选项」->「设备信息」这里也能看到。去掉空格输入 16 位字母或数字，注意大小写。

Kindle for PC 需要事先安装好 Kindle for PC 软件并用自己的亚马逊账号登录。DeDRM 插件会自动获取一个解密 key ，通常是 default_key ，也可以重命名。如果在安装并登录 Kindle for PC 之前已经运行过 Calibre，首次运行时生成的 default_key 是无效的，需要在完成 Kindle for PC 登录之后添加解密 key。点击插件右上角的绿色「＋」号，随便取一个 key 名称，插件会自动重新获取 Kindle for PC 的解密 key。[![](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-3.png?w=618)](https://maajiaa.files.wordpress.com/2018/07/dedrm-settings-3.png)

### 四、导入 Kindle 电子书

在 Calibre 主界面选择「**添加书籍**」，对于 eInk Kindle 的电子书，很简单，直接添加之前「通过电脑下载 USB 传输」得到的单一电子书文件即可。或是用 USB 数据线把 kindle 设备连接到电脑，添加 kindle 设备「documents」文件夹下的 .kfx 文件。

对于 Kindle for PC 的电子书，导入时需保证电子书在 Kindle for PC 书库路径底下，通常是在「**我的文档 \ My Kindle Content**」，每本书一个子目录，导入子目录中的 **.azw** 文件。**不能将电子书复制粘贴到其他路径再导入。**

导入之后 DeDRM 和 KFX Input 插件会尝试自动解秘，如果解密成功，Calibre 书库里会看到一个 AZW3 格式或 KFX 格式的书籍。KFX 格式若不能直接双击打开，可右键单击，选择「阅读」->「用 calibre 电子书阅读器打开」，或点击 calibre 上方菜单「**转换书籍**」转成其他格式，比如 epub 。

如果导入后看到的是 KFX-ZIP 格式或 AZW 格式，则说明解密失败。

大功告成！

[![](https://maajiaa.files.wordpress.com/2018/07/calibre-import-successful.png?w=355)](https://maajiaa.files.wordpress.com/2018/07/calibre-import-successful.png)

祝大家买过的电子商品永远不会被消失！