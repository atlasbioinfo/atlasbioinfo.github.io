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

前面讲到需要安装tensorflow-gpu，并不是安装了这个就直接可以GPU加速了。GPU加速是依赖Nvidia的CUDA GPU加速软件包的，所以我们需要安装CUDA GPU加速包和专门为深度学习设计的cuDNN包。

差点忘了说了，为社么要用GPU加速呢？我CPU不是也可以好好运行吗？其实深度学习计算量相当大，但是每个计算量都不算太繁重。

这么说吧，CPU是一个大力士，比如最新的i7 8770就可以被认为是12个大力士（6核12线程）；而GPU是小学生，但是很多，比如GTX1080Ti，就像是3584个小学生（3584个流处理器单元）。如果你让这两组人搬100个巨石，那大力士肯定厉害，小学生都被累趴了也搬不动。但是如果让这两组人去送10000封信，那很明显是小学生厉害啊，人多。而深度学习就很类似去送10000封信，大力士每次送12封，小学生每次送3000多封，肯定GPU有优势嘛！

回到安装上，有一点需要注意的就是，版本问题。tensorflow并不是通识所有版本的CUDA的，目前我验证过的是1.8和1.9版本的tensorflow可以使用9.0版本的CUDA和cnDNN。低于1.8的tensorflow就不行=-=。

安装就是在官网下载制定版本的CUDA和cnDNN就行了。

**[CUDA官网下载](https://developer.nvidia.com/cuda-downloads)**

![CUDAdownload](http://ow1kvhtif.bkt.clouddn.com/cudaDownLoad.PNG)

注意：**在安装CUDA的时候，它会检验physX和Nvidia Geforce Experence，这两个软件如果你之前安装过更高版本的，就没办法安装CUDA，需要把高版本的这两个软件卸载才行**

**[cuDNN官网下载](https://developer.nvidia.com/cudnn)**

![cuDNN下载](http://ow1kvhtif.bkt.clouddn.com/cuDNNdownload.PNG)

注意，cuDNN下载的时候Nvidia要求注册他们的账号登陆，并填一个调查问卷，填好就可以下载了。

cuDNN不需要安装，解压缩之后有一个include和一个lib64 2个文件夹。把这两个里面的东西直接复制粘贴到你安装CUDA目录里对应文件夹里就行了。这个是Windows的安装，linux似乎不是这样（以后自己试过跟大家再详细说）

其实，似乎还有一个简单的方案，就是直接在Anaconda里面安装cudatoolkit似乎就行=-=，不过每次都是安装完前两个才想到这种方法，所以不知道是不是单独用anaconda安装就可以实现。有兴趣的朋友可以试试。

![AnacondaCuda](http://ow1kvhtif.bkt.clouddn.com/AnacondaCuda.PNG)

如果安装好了之后，打开你运行python的IDE（我用的是PyCharm），运行下面的命令：

```python
import tensorflow as tf
sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
```

如果出现下图，现实GPU信息，就说明成功了：

![success](http://ow1kvhtif.bkt.clouddn.com/successGPUTensorflow.PNG)

## 出现的各种问题以及修复策略 ##

### FutureWarning: Conversion of the second argument... ###

完整的报错为：

```bash
FutureWarning: Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.from ._conv import register_converters as _register_converters
```
这是因为h5py包没装好，装好就行了。

```bash
pip install h5py==2.8.0rc1
```

### h5py包装不上 ###

那需要安装msgpack包，同样的方法，见下图：

![msgInstall](http://ow1kvhtif.bkt.clouddn.com/msgpackInstall.PNG)

### 小技巧：用迅雷加速下载 ###

在你用pip install的时候下载速度可能会比较慢，这时候你可以把那个带安装包whl的下载链接复制下来，用迅雷等软件下好，之后本地安装。本地安装也很简单, 命令行切换到到那个目录，之后:

```bash
pip install ./tensorflow_1_9_0.whl
```

就好了。
