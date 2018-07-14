---
layout: post
title: Windows10环境下Tensorflow的安装和GPU加速配置
categories:
 - 生物信息
---

也算折腾了好几天，终于在Windows上把Tensorflow和GPU加速配置好了，跟大家分享一下经验。目前实验室服务器上木有显卡，所以以后实验室土豪了装了显卡我再去摸索Linux怎么配置这个，估计套路差不多。
>* 怎么安装Tensorflow
>* 怎么配置GPU加速
>* 出现的各种问题以及修复策略

***

Tensorflow就不用简介了，谷歌的深度学习框架，也是目前最火的深度学习框架。其他的框架还有Caffe，Theano，MXNet等等。我是个小白，其他没用过，就不说了=-=，详细的大家可以看
**[对比深度学习十大框架](https://www.oschina.net/news/80593/deep-learning-frameworks-a-review-before-finishing-2016)**