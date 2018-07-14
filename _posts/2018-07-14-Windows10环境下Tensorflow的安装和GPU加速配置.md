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

***[对比深度学习十大框架：TensorFlow 并非最好？](https://www.oschina.net/news/80593/deep-learning-frameworks-a-review-before-finishing-2016)***

## 怎么安装Tensorflow ##

Tensorflow更新的好快，在年初我开始学的时候是1.1版本，现在已经更新到1.90了。

最近的新版本很多大佬都说好，因为对新人很友好，除了很多高级API（就是用更少的代码实现功能），1.9版本还加入了对keras的直接支持。

![tensorflowLogo](http://ow1kvhtif.bkt.clouddn.com/tensorflowLogo.jpg)

安装Tensorflow最简单的方法有2种：
1. Anaconda和Conda 
2. pip

推荐大家用第一种，windows和linux都是。

Anaconda是一个python的包管理器，里面有大多数数据处理的包还自带Jupyter Notebook等。 安装很简单，就是从官网下正确的版本，之后一路点next。

***[Anaconda官网](https://www.anaconda.com/download/)***

安装好之后，windows用户可以直接点Anaconda Navigator进入这个界面，点Enviroments，之后需要安装哪个就点哪个就好了，最后点apply就安装了。

![AnacondaTensor](http://ow1kvhtif.bkt.clouddn.com/anacondaTensor.PNG)

或者，直接cmd启动命令行终端，或者打开windows10特有的Windows PowerShell，输入：

```bash
conda install tensorflow tensorflow-gpu
```

简单吧。其中，tensorflow是主程序，-gpu的那个可以支持gpu加速。如果你只需要CPU计算就可以了，那么可以不安装这个或者不care我后面写的其他的，直接开始跑程序就好了。

不过有一点很重要，就是安装前需要先更新一下源，就是你下载软件的地方。额，再详细点吧，就是比如你点了apply或者输入了conda install XXX后，都会从一个网站下载软件到你的目录下，再解压缩执行安装。

目前国内比较好的就是清华的源，再命令行输入以下3行：

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

注意，这两个源都得加哦，要不很多软件下不到或者更新不到最新版本。

今天是7月14号，目前这个源tensorflow最新的为1.8，cudnntoolkit(后面讲)最新版本为9.0。所以如果需要下载最新版本tensorflow的可以用Pip:

```bash
pip install tensorflow tensorflow-gpu
```

如果你已经用conda安装了tensorflow，这时候就报错，装不上了，需要另一个命令，更新：

```bash
pip install --upgrade tensorflow tensorflow-gpu
```

这样，就安装好了。

## 怎么配置GPU加速 ##

前面讲到需要安装tensorflow-gpu，并不是安装了这个就直接可以GPU加速了。GPU加速是依赖Nvidia的CUDA GPU加速软件包的

## 出现的各种问题以及修复策略 ##


```python
import tensorflow as tf
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
```



```bash
pip install --upgrade tensorflow-gpu
```

![successGPU](http://ow1kvhtif.bkt.clouddn.com/successGPUTensorflow.PNG)
![AnacondaCuda](http://ow1kvhtif.bkt.clouddn.com/AnacondaCuda.PNG)

![msgInstall](http://ow1kvhtif.bkt.clouddn.com/msgpackInstall.PNG)