> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/353815261)

**百万富翁问题**
----------

```
两个富翁，分别为张三和李四
他们自己都清楚自己有几千万财产即他们心里清楚 1～10中的一个数（代表自己千万级的财富）
他们想知道到底谁的数更大一些。


```

`不经意传输的解决方案`

![](https://pic1.zhimg.com/v2-706f9f5017c265933c39c5514d9b83d4_r.jpg)

`不经意传输（OT）协议`

```
在密码学中 发送方可以向接收方传输一系列信息中的某一部分
接收方可以正确收到信息，但不知道信息属于整体的哪个部分


```

**安全可信计算**
----------

### **外包计算**

`含义`

![](https://pic3.zhimg.com/v2-e8430372ef3ebfe900a4982f81e983ce_r.jpg)

`常用技术`

```
同态加密


```

![](https://pic3.zhimg.com/v2-73771369aeb9f5525aec9dd27209127e_r.jpg)

`典型的应用场景`

```
数据持有者想对其持有的大量数据进行计算
奈何其拥有的计算资源不足
想借助云服务器的算力完成该计算


```

![](https://pic4.zhimg.com/v2-d4b9484f177b5e8ffa080f679bb47427_r.jpg)

### **多方计算**

```
多方计算的目标就是对一组计算的参与者
每个参与者拥有自己的数据
并且不信任其它参与者和任何第三方
在这种前提下，如何对各自私密的数据计算出一个目标结果的过程


```

`应用场景`

```
姚式百万富翁
安全拍卖
安全电子选举
安全机器学习
丹麦甜菜拍卖
波士顿工资平等研究


```

`安全模型`

```
根据参与方的可信程度可以建立几种安全模型


```

![](https://pic2.zhimg.com/v2-f455c09681899b3a9297d244feeb6ce9_r.jpg)

*   理想模型

![](https://pic1.zhimg.com/v2-86fc2b14307a2c131dff4ee88791f3a4_r.jpg)

**现实生活中不存在**

*   半诚实模型

```
参与方会诚实的运行协议
但是他会根据其它方的输入或者计算的中间结果来推导额外的信息


```

*   恶意模型

```
可能不会诚实的运行协议，甚至会搞破坏


```

**相比于恶意模型，参与方如果真的想获取到同时对自己有用的信息，多数情况下符合半诚实模型**

**基本概念和方法**
-----------

### **Secret Sharing（密钥分享）**

![](https://pic2.zhimg.com/v2-1aab0877853aa4c32959a18a2ca9bea9_r.jpg)

```
1、数据拆分->数据分发->数据计算->得到计算结果->汇总计算结果

2、密钥分享保证了计算过程中各个参与方看到的都是一些随机数
但最后仍然算出了想要的结果


```

### **Random Oracle (随机预言机)**

`表结构`

![](https://pic3.zhimg.com/v2-a51e8d26104b063af717aed0e329f9b6_r.jpg)

`RO的行为`

![](https://pic1.zhimg.com/v2-595cdbeb79ef973adf35f4594e9c2748_r.jpg)

`重要的安全性特征`

**如果 x 未曾记录在表里，则 RO 相当于一个完全随机的函数**

```
1、RO自己都不知道x会被映射到哪个值
2、任何询问RO的人，即使拿到了除了x之外所有的RO的输出，也无法确定x会被映射到哪个值


```

`x是什么值的概率`

```
一个未曾记录在表里的新元素x映射到目标集合里特定元素E的概率
即把任意长度的01字符串映射到一个256位的01字符串上的Hash
即映射到k位的01字符串上


```

### **Yao’s Garbled Circuits Protocol（姚氏混淆电路）**

![](https://pic4.zhimg.com/v2-31e9c3a3b0260fde463ee723e47f85df_r.jpg)

```
混淆电路就是通过加密和扰乱这些电路的值来掩盖信息的
加密和扰乱是以门为单位的
每个门都有一张真值表


```

`与门的真值表`

![](https://pic4.zhimg.com/v2-a682a3ec7663bfffda64dc27e1dd4237_r.jpg)

`或门的真值表`

![](https://pic1.zhimg.com/v2-a283616518a1c9b821296e2b950ed5b8_r.jpg)

`Alice给Bob发送数据`

![](https://pic3.zhimg.com/v2-79039fb76719f93d067a8af367e819ba_r.jpg)

`整体思路`

```
Step 1: Alice 生成混淆电路
Step 2: Alice 和 Bob 进行通信
Step 3: Bob evaluate 生成的混淆电路
Step 4: 分享结果


```

### **Step 1: Alice 生成混淆电路**

`第一步`

```
Alice 对电路中的每一线路（Wire）进行标注


```

![](https://pic1.zhimg.com/v2-4c6d5fa85f1997a82cc626fd94a38008_r.jpg)

```
模块输入输出 Wa0,Wb0,Wc0,Wc1
模块中间结果 Wd,We,Wf
对于每条线路Wi
Alice生成长度为k的字符串


```

![](https://pic3.zhimg.com/v2-98d65366c0a9f6ee68492fc7e3691896_b.png)

```
这2个字符串分别对应逻辑上的0和1
这些生成的标注会在Step2 有选择的发送给Bob
但Bob并不知道这两个字符串对应的逻辑值


```

`第二步`

```
Alice对电路中的每一个逻辑门的Truth Table用


```

![](https://pic3.zhimg.com/v2-98d65366c0a9f6ee68492fc7e3691896_b.png)

```
进行替换 由


```

![](https://pic1.zhimg.com/v2-891a1aefa157a8f9e1a582429b8fa300_b.png)

```
替换成0


```

![](https://pic2.zhimg.com/v2-784b0bb5838a764585954363293c4eb1_b.png)

```
替换成1

比如电路图中左上方的 XOR 门的输入是 a0、c0 输出是 d
对应的 Truth Table 可以做如下转换


```

![](https://pic4.zhimg.com/v2-ad3ada3109633895c6311c12bce8e48f_r.jpg)

`第三步`

```
Alice 对每一个替换后的 Truth Table 的输出进行两次对称密匙加密（即加密和解密的密匙相同）
加密的密匙是 Truth Table 对应行的两个输入
比如 Truth Table 的第一行是 


```

![](https://pic1.zhimg.com/v2-3d6418dd04ac306f83744d24ab02b2b0_b.jpg)

```
则用


```

![](https://pic3.zhimg.com/v2-dc54f7a2388c6787cfd6dfd8f146b50e_b.png)

```
加密


```

![](https://pic2.zhimg.com/v2-65a6734910f7a2b4252e46e3adfb3765_b.png)

```
生成


```

![](https://pic4.zhimg.com/v2-9864ab8d5bf3e76e7835e6c16a000903_b.jpg)

`第四步`

```
Alice 对第三步加密过后的 Truth Table 的行打乱得到 Garbled Table
所以 Garbled Table 的内容和行号就无关了
混淆电路的混淆二字便来源于这次打乱


```

### **Step 2: Alice 和 Bob 进行通信**

`第一步`

```
Alice 将她的输入对应的字符串发送给 Bob
比如 a0=1 那 Alice 会发送


```

![](https://pic3.zhimg.com/v2-181cb4b658f27b7bfd976ec419a91ba6_b.png)

```
给 Bob

由于 Bob 不知道



```

![](https://pic4.zhimg.com/v2-978729bc2775233535a8eea6cddec2b7_b.png)

```
对应的逻辑值
也就无从知晓 Alice 的秘密了


```

`第二步`

```
Bob 通过不经意传输（OT）协议从 Alice 获得他的输入对应的字符串
不经意传输保证了 Bob 在 


```

![](https://pic4.zhimg.com/v2-e6e118f188c3450e72d319cf2bd0311b_b.png)

```
中获得一个
且 Alice 不知道 Bob 获得了哪一个
所以 Alice 也就无从知晓 Bob 的


```

![](https://pic4.zhimg.com/v2-2552a3e12b847bfd9f755b6546f670e3_b.png)

`第三步`

```
Alice 将所有逻辑门的 Garbled Table 都发给 Bob
在这个例子中，一共有四个 Garbled Table


```

### **Step 3: Bob evaluate 生成的混淆电路**

```
Alice 和 Bob 通信完成之后
Bob 便开始沿着电路进行解密

因为 Bob 拥有所有输入的标签和所有 Garbled Table
他可以逐一对每个逻辑门的输出进行解密

在这个例子中，假设 Bob 拥有的输入标签为


```

![](https://pic2.zhimg.com/v2-2e7383fbdfd9996ecb6945af1b36c13d_b.jpg)

`他可以`

![](https://pic4.zhimg.com/v2-268a46a678e55ba6b06f247a52d896a7_r.jpg)

```
由于 Garbled Table 每一行的密匙都不同
所以 Bob 只能解密其中一行
Bob 并不知道解密出来的 


```

![](https://pic4.zhimg.com/v2-162b18d41014dbf2c3169b050ea3914f_b.jpg)

```
对应的逻辑值 也就无从获得更多信息了
而 Alice 全程不参与 Bob 的解密过程
所以也如法获得更多信息


```

### **Step 4: 分享结果**

```
最后 Alice 和 Bob 共享结果
Alice 分享


```

![](https://pic3.zhimg.com/v2-949628b8b208afc837c8ab1cd4a4c9de_b.png)

```
或者 Bob 分享 


```

![](https://pic3.zhimg.com/v2-a55dd2e2d6ecb01adcb96066a734d0fe_b.png)

```
双方就能获得电路输出的逻辑值了


```

### **Zero-Knowledge Proof (零知识证明)**

```
零知识证明指的是证明者能够在不向验证者提供任何有用的信息的情况下
使验证者相信某个论断是正确的


```

![](https://pic4.zhimg.com/v2-d6a9e0996433eb53103f9d31e1fa780b_r.jpg)

```
洞穴里有一个秘密，知道咒语的人能打开 C 和 D 之间的密门
但对任何人来说，两条通路都是死胡同
假设 P 知道这个洞穴的秘密
她想对 V 证明这一点，但她不想泄露咒语
下面是她如何使 V 相信的过程


1、V站在A点
2、P一直走进洞穴，到达C或者D点
3、在P 消失在洞穴中之后 V走到B点
4、V向P 喊叫，要她: 从左通道出来，或者从右通道出来
5、P答应 若有必要则用咒语打开密门
6、P和V重复步骤(1)-(5)多次

若多次重复中 若每次P都从V要求的通道中出来
则能说明P确实知道咒语
并且V不知道咒语的具体内容

```