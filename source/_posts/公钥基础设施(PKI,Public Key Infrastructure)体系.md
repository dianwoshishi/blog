---
title: [技术]公钥基础设施(PKI,Public Key Infrastructure)闲谈
date: 2022-02-26 14:03:13
---

# 公钥基础设施(PKI,Public Key Infrastructure)闲谈

## 背景

在现实空间中，人类的活动范围和接触人的范围有限，人和人最初的信任是建立在小团体或部落内部。随着全球化进展，人类的活动已经遍布全球，通信极度发达，能够低延迟的在地球的两端进行通话，甚至是太空与地面的通话（参考：[卫星互联网这样助力太空通话](https://www.toutiao.com/i6977910971411071518/)）。但这对人和人之间的身份识别带来了前所未有的挑战。

以前人和人之间的身份识别非常的简单，就是面对面的交流：通过外貌、体型、声音等。但是随着贸易的往来，人和人之间见面识别身份的代价逐渐开始变大，例如，美国和英国的人见面就要大费周章。同时，人和人之间有交往并不代表这两个人认识，可能他们真的就是陌生人而已，双方可能都极少掌握对方的信息。 所以在这种非接触式(不能见面)的情况下，如何有效的识别身份，建立信任关系是一个重要的问题。

人们最初设计互联网时，很少考虑到安全。这样的结果是，核心通信协议本质上是不安全的，只能依靠所有参与方的诚信行为。互联网时代更是对解决这个问题提出了更加迫切的需求。网络通信的双方如何认证对方的身份，对于网络安全至关重要。

身份认证技术是保护信息安全的第一道屏障，其核心技术是信息安全保障体系，也是最基本的环节。它的基本思路：经过验证用户所具有的属性，来判断用户身份是否真实。

什么意思呢？举个例子，在智取威虎山中

![天王盖地虎暗号大全](https://img.phb123.com/uploads/allimg/161226/1-1612261S942A5.jpg)

土匪通过黑话“认证”了杨子荣的“土匪”身份：

> 土匪：蘑菇，你哪路？什么价？（什么人？到哪里去？）
> 杨子荣：哈！想啥来啥，想吃奶来了妈妈，想娘家的人，孩子他舅舅来了。（找同行）
> 杨子荣：拜见三爷！
> 土匪：天王盖地虎！（你好大的胆！敢来气你的祖宗？）
> 杨子荣：宝塔镇河妖！（要是那样，叫我从山上摔死，掉河里淹死。）
> 土匪：野鸡闷头钻，哪能上天王山！（你不是正牌的。）
> 杨子荣：地上有的是米，喂呀，有根底！（老子是正牌的，老牌的。）
> 土匪：拜见过阿妈啦？（你从小拜谁为师？）
> 杨子荣：他房上没瓦，非否非，否非否！（不到正堂不能说。）
> 土匪：嘛哈嘛哈？（以前独干吗？）
> 杨子荣：正晌午说话，谁还没有家？（许大马棒山上。）
> 土匪：好叭哒！（内行，是把老手）
> 杨子荣：天下大耷拉！（不吹牛，闯过大队头。）
> 座山雕：脸红什么？
> 杨子荣：精神焕发！
> 座山雕：怎么又黄了？
> 杨子荣：防冷，涂的蜡！
> 座山雕：许旅长两件心爱的宝贝你知道是什么？
> 杨子荣：两件珍宝，好马快刀。
> 座山雕：马是什么马？
> 杨子荣：卷毛青棕马。
> 座山雕：刀是什么刀，
> 杨子荣：日本指挥刀。
> 座山雕：看来你是许旅长的人啦！

这就是一种身份认证的技术，通过双方事先掌握的秘密知识，实现了身份认证。类似于我们日常生活中的“QQ密码”(认证你是账户主人的身份)。

## 基于口令的身份认证技术的问题

看上去似乎很美好，有什么问题呢？上述方法的认证是基于口令的认证，这种认证天生“缺陷”，尤其是在当前点对点通信的环境下。

1. 口令代表了你的身份，但是口令并不安全

在《智取威虎山》中，杨子荣就是利用这个问题，成功的打入土匪内部。在现实生活中一样，如果我拥有了你的密码，那么我就拥有了你QQ上的这个身份。并且，这件事很容易做，毕竟123456这样的口令始终是最流行的口令。虽然现在有了双因子认证（口令，短信验证码），但只是加大了难度，由口令代表你的身份变为口令，手机代表你的身份。

2. 口令不能分发，势必不能大规模使用

口令是一个人的秘密，不能告诉任何人，这势必会影响大规模使用，这种Client-Server的模式还好，但是当前大量P2P模式的认证怎么办？每个人保存所有人密码的hash来认证？这种方法代价太大

3. 杨子荣式的“挑战-应答”方式在身份认证中代价太大

这种问题可能会没有一个尽头，并且不能保证即使通过挑战，就一定百分百是真实身份。

实质上，口令的方式代表的是密码体制中的对称密码体制。

![img](https://pic1.zhimg.com/80/v2-865fe6654b7173f2dd6745756f3f4635_720w.jpg?source=1940ef5c)

上述的问题也是对称密码体制存在的问题：密钥分发和管理问题。可能你发现了，对称密码就使用一把钥匙，那么问题就来了，在当今网络时代，如果大家都共享一把钥匙，那么会出现什么情况呢？ 当然就是加密如同没有加密一样。所以，对称密码的痛点就在于秘钥分发的困难性。二战时期，非对称密码体制还未出来，德军和盟军都使用密码本这个东西来分发秘钥，可想而知，如果密码本被缴获，那么代价就是消息不安全，更换新的密码本(人力、物力、财力)。

这种对称加密技术的问题给身份认证也带来了挑战。那么解决方案是什么呢？

## 现有方案是什么

现有的解决方案是使用公钥基础设施。

### 非对称加密技术

公钥基础设施的基础是非对称加密技术

![img](https://pic4.zhimg.com/80/v2-47368b1ae94f65afa4bc99709bbc92e5_720w.jpg?source=1940ef5c)

非对称加密技术，简单来说就是使用者有一对密钥：公钥和私钥。顾名思义，公钥就是公开的密钥，可以发给任何人，私钥就是秘密密钥，只能自己保留。

根据对公钥私钥的使用不同，非对称加密体制可以干两件事：加密和签名。

### 加密

当使用公钥对消息进行操作，用私钥进行逆操作，这种方式称为加密。

### 签名

用私钥进行加密，用公钥进行逆操作，这种方式叫做签名。当然一般加密的部分是消息的哈希摘要。这样用户只需要验证能否用发送者的公钥解密，然后再对比哈希摘要就可以实现对私钥的认证，继而完成**身份认证**。在身份认证中，更多的使用的是签名。

## 公钥基础设施

有了公开密钥算法之后，我们就可以通过他人的公开密钥与其安全通信，但是还有一些悬而未决的问题：如何与那些从未谋面的人进行沟通？如何存储和吊销密钥？最重要的是，如何让现实世界中数以百万计的服务器、几十亿人和设备之间安全通信？这个问题非常的复杂，而公钥基础设施（public key infrastructure, PKI）就是为解决此问题而建立的。

![查看源图像](https://media.githubusercontent.com/media/jckling/Assets/master/GB/certificate/hierarchy.png)

### 数字证书

同样，公钥如何分发也是一个问题。直接把公钥的发给别人可以吗？理论上是可以的，但实际上有诸多问题，例如我怎么知道你发给我的一串数字（公钥）是谁的？怎么发，如何保证没有被篡改？

在现实生活中，我们的做法是，给你颁发一张证书，证明你是来自蓝翔的某某某，你具有什么资质，那些产品是你做的。在数字空间中呢？

同样，数字空间中有数字证书。数字证书包含证书持有人的真实身份信息、公开密钥信息等信息一段数据。目前数字证书的格式普遍遵循 X.509V3 国际标准，证书包括证书的版本号、序列号、算法、命名规则通常采用 X.500 格式、有效日期、名称、公钥信息、数字证书颁发者标识符。 X.509V3 数字证书包括扩展标识符、关键程度指示器和扩展字段值。数字证书扩展包括密钥信息、政策信息等。

### 认证中心

但是是不是谁都可以颁发证书呢？理论上也是可以的，但是实际上，你发的也许根本没人信。这就和现实生活中做假证的一样，不具有可信性。

颁发证书需要强大的机构做背书。这种机构就是认证中心 CA。

认证中心 CA 为特定的应用网络和用户提供身份认证服务，负责验证公钥的合法性，证明用户是证书的合法持有者，同时，签发、分配、储存、更新和删除证书。这里我们不妨称这个CA为大哥，因为它具有最大的权威。

### 证书链

但是一个大哥来干这个事好费劲啊！全球这么多需求，这么多用户。怎么办呢？找些小弟吧，让他们来我这登记一下，然后我授权给他们，给与他们颁发证书的资质。这样就形成了层次结构。

![查看源图像](https://th.bing.com/th/id/Re08ba40613302676a64668f8b2d7d191?rik=sTOEgyliVxRfEA&riu=http%3a%2f%2fpublib.boulder.ibm.com%2ftividd%2ftd%2fTRM%2fSC23-4822-00%2fzh_CN%2fHTML%2ftrustchn.gif&ehk=SRsGXyv4kgmvFgnFtzviporInEXkCtdNAHAnWIW7w9U%3d&risl=&pid=ImgRaw)

但是有个问题出现了，小弟可靠吗？我原来是信任大哥的，但是现在你派个小弟来，我就有点犹豫了。

但是此时，如果小弟拿出一张纸条，纸条上说：我是XXX,我是大哥的小弟。并且最后附上大哥难以伪造的签名，可信吗？ 这就好多了对吧，感觉一下就信任了这个小弟了。

万一小弟又派出了小小弟呢？那就小弟再写一张纸条，类似的内容，签上小弟的名字，给小小弟，小小弟就拿着开心的去办事了。这样就形成了

$你-->    小小弟 -->     小弟 -->    大哥$

这样的信任链条，而这里面信任的基础就是签了名的纸条（数字证书）。如何验证呢？就是拿着纸条一个一个去问上一级，这是你的签名吗？直到问到大哥。



## 总结

本来想写具体的PKI的技术，但是考虑到具体技术之前需要对整体有个认识Big Picture。所以最后题目改为“闲谈”。固然有很多不完善的地方，恳请大家批评指正。

