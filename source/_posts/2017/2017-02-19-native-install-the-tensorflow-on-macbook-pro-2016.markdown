---
layout: post
title: "在Macbook Pro 2016中原生安装tensorflow 1.0"
date: 2017-02-19T15:15:09+08:00
comments: true
external-url: 
categories: [Python, TensorFlow]
---

前几天的TensorFlow开发者峰会宣布的TensorFlow 版本达到1.0，再加上过去一年人工智能，机器学习非常非常热，虽然作为一名Ruby程序员，但形势所迫，又不得不捡起了Python，准备稍稍了解一下。

[TensorFlow的安装官网](https://www.tensorflow.org/install/install_mac)写的很清晰，我选择的是virtualenv的安装，安装的python版本是brew的版本。

```bash
brew install python
sudo easy_install pip
sudo pip install --upgrade virtualenv
virtualenv --system-site-packages ~/tensorflow
source ~/tensorflow/bin/activate 
pip install --upgrade tensorflow 
```

安装完成后每次运行需要先运行`source ~/tensorflow/bin/activate`，嫌麻烦可以把这句加到.bash_profile中。

```bash
echo "source ~/tensorflow/bin/activate" >> ~/.bash_profile
```

安装完成后可以跑一下tensorflow版的“Hello Word”验证一下安装是否正确，需要先进入python的交互环境：

```bash
python
```

然后运行：

```python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

{% img /images/2017/cpu_instructions_warning.jpg TensorFlow will warning CPU instruction not native %}

问题来了，一堆CPU指令没有优化，Macbook Pro 2016已经没有核弹卡了，只能跑在CPU版上了，如果CPU再不优化，感觉已经很难愉快的玩下去了，所以只能选择从[源码安装](https://www.tensorflow.org/install/install_sources)了。

源码安装就很烦了，下载源吗就非常费网络，还必须FQ，编译也需要至少25分钟时间，不过亲爱的读者，我已经帮你们做好了，现在可以使用我的安装包直接升级到原生CPU优化版：

```bash
pip install --upgrade http://baye-deploy.oss-cn-shanghai.aliyuncs.com/tensorflow-1.1.0-cp27-cp27m-macosx_10_12_x86_64.whl
```
