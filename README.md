swjtu donkeycar项目实践

# lvche016

## 项目简介
本项目最终目的是搭建一套无人驾驶小车
项目设计方法和技术有：
    智能计算系统的设计与构建方法
    深度学习模型的训练和优化方法
    轮式机器人的机械结构和电路系统的硬件制作能力
    通过项目制作和搭建，了解基本的制造工艺、工程设计的过程和方法
## 功能特性
本项目将会实现小车的数据采集、训练和自动驾驶

## 环境依赖
### 电脑端
#### 安装miniconnda Python3.7
    https://blog.csdn.net/u012325865/article/details/80454813
#### 安装git 64bit
    https://blog.csdn.net/qq_42594368/article/details/81902367
### 树莓派端
#### 利用Win32DiskImager进行系统安装
    https://blog.csdn.net/guoer9973/article/details/78981205

## 子模块和库的描述
NumPy(Numerical Python) 是 Python 语言的一个扩展程序库,支持大量的维度数组与矩阵运算,此外也针对数组运算提供大量的数学函数库。
pillow一个非常好用的图像处理库
docopt 本质上是在 Python 中引入了一种针对命令行参数的形式语言，在代码的最开头使用 """ """文档注释的形式写出符合要求的文档，就会自动生成对应的parse
Tornado 是 FriendFeed 使用的可扩展的非阻塞式 web 服务器及其相关工具的开源版本，提供一个轻量化的web框架
requests是一个Python第三方库，用于快速处理URL资源
h5py文件是存放两类对象的容器，数据集(dataset)和组(group)，dataset类似数组类的数据集合，和numpy的数组差不多。group是像文件夹一样的容器，它好比python中的字典，有键(key)和值(value)。
PrettyTable 是python中的一个第三方库，可用来生成美观的ASCII格式的表格
MQTT（MQ Telemetry Transport），消息队列遥测传输协议，轻量级的发布/订阅协议，适用于一些条件比较苛刻的环境，进行低带宽、不可靠或间歇性的通信。
simple_pid是Python中的一个简单并且容易使用的PID控制器。
progress是文本进度条，可以在文本显示界面中方便的看到处理过程的进度
typing_extensions暂未查到作用
pyfiglet 是一个专门用来生成艺术字的模块，只支持英文。
psutil是一个跨平台库(http://pythonhosted.org/psutil/)能够轻松实现获取系统运行的进程和系统利用率（包括CPU、内存、磁盘、网络等）信息。它主要用来做系统监控，性能分析，进程管理。它实现了同等命令行工具提供的功能，如ps、top、lsof、netstat、ifconfig、who、df、kill、free、nice、ionice、iostat、iotop、uptime、pidof、tty、taskset、pmap等。

## 部署步骤
### 更改为您想用作项目负责人的目录。
    mkdir projects
    cd projects
### 从 Github 获取最新的驴。
    git clone https://github.com/autorope/donkeycar
    cd donkeycar
    git checkout master
### 如果这不是您的第一次安装，请更新 Conda 并删除旧驴
    conda update -n base -c defaults conda
    conda env remove -n donkey
### 创建 Python anaconda 环境
    conda env create -f install\envs\windows.yml
    conda activate donkey
    pip install --user tensorflow==2.2.0
    pip install -e .[pc]
### 创建本地工作目录：
    donkey createcar --path ~/mycar
### 查看新创建的目录中的myconfig.py， ~/mycar
    cd ~/mycar
    nano myconfig.py
### 打开您的汽车文件夹并启动您的汽车。
    cd ~/mycar
    python manage.py drive

## 相关重点指令
### 收集数据
    确保收集好的数据。

    在赛道上练习几次。
    当您确信自己可以无误地行驶 10 圈时，重新启动 python mange.py 进程以创建新的 tub 会话。Start Recording如果使用网络控制器，请按。操纵杆将自动记录任何非零油门。
    如果您撞车或跑出赛道，请立即按 Stop Car 停止录音。如果您使用操纵杆，请轻按三角形按钮以删除最后 5 秒的记录。
    在您收集了 10-20 圈的良好数据（5-20​​k 图像）之后，您可以Ctrl-c在您的汽车的 ssh 会话中停止您的汽车。
    您收集的数据位于最近的 tub 文件夹中的 data 文件夹中。
    将数据从您的汽车传输到您的计算机
    由于树莓派不是很强大，我们需要将数据传输到PC电脑上进行训练。Jetson nano 更强大，但训练速度仍然很慢。如果需要，请跳过此传输步骤并在 Nano 上进行训练。

    在主机 PC 上的新终端会话中，使用 rsync 从 Raspberry Pi 复制汽车文件夹。

    rsync -rv --progress --partial pi@<your_pi_ip_address>:~/mycar/data/  ~/mycar/data/
### 训练模型
    在同一个终端中，您现在可以通过将路径作为参数传递到该浴缸来在最新的浴缸上运行训练脚本。您可以选择传递路径掩码，例如./data/*或./data/tub_?_17-08-28以收集多个浴                    缸。例如：
    donkey train --tub <tub folder names comma separated> --model ./models/mypilot.h5
    您可以--type在训练期间使用参数创建不同的模型类型。您还可以选择更改 myconfig.py 中的默认模型类型DEFAULT_MODEL_TYPE。指定新模型类型时，请确保在运行模型时提供该类    型，或在绘图或分析等其他工具中使用该模型。有关不同模型类型的更多信息，请在此处查看Keras Parts。模型将被放置到文件夹中models/。您也可以省略--model标志，模型名称将使用模式自动创建pilot_YY-MM-DD_N.h5。

    如果您使用版本 >= 4.3.0 运行，模型将自动以 tflite 格式创建以进行快速推理。训练./models/mypilot.tflite也会生成一个文件。通过CREATE_TF_LITE = False在您的    myconfig.py文件中进行设置，可以抑制 Tflite 创建。此外，如果您设置CREATE_TENSOR_RT = True，则会生成 tensorrt 模型，这是False默认设置。该设置生成的./models/mypilot.trt文件应该适用于所有平台。在 RPi 上，tflite 模型将是最快的。

    注意：在 4.2 版中有一个回归，您只需在模型参数中提供模型名称，例如--model mypilot.h5. 这在 4.2.1 版中得到解决。请更新到那个版本。

### 将模型复制回汽车
    在上一步中，我们设法获得了一个对数据进行训练的模型。现在是将模型移回 Rasberry Pi 的时候了，因此我们可以使用它来测试它是否会自动驱动。

### 再次使用 rsync 将您训练有素的飞行员模型移回您的汽车。
    rsync -rv --progress --partial ~/mycar/models/ pi@<your_ip_address>:~/mycar/models/
    确保将汽车放在轨道上，以便准备好行驶。

### 现在您可以再次启动您的汽车并将其传递给您的模型以进行驾驶。
    python manage.py drive --model ~/mycar/models/mypilot.h5
    但是，如果您从 tflite 模式开始，您会看到更好的性能。
    python manage.py drive --model ~/mycar/models/mypilot.tflite --type tflite_linear
    车子应该可以自己开车了，恭喜！

## 历史问题
### 安装过程中会出现下面这个问题：
    Found existing installation: numpy 1.12.1
    Not uninstalling numpy at /usr/lib/python3/dist-packages, outside environment /home/pi/env
    Can’t uninstall ‘numpy’. No files were found to uninstall.
    在此处要耐心等待完成，完成后用下述命令查看numpy版本：
    pip list
    使用下述命令卸载新安装的numpy版本
    pip uninstall numpy
    使用以下命令验证tensorflow的安装。
    python -c "import tensorflow"
    出现下述信息。
    下述的警告是正常的：
    /home/pi/env/lib/python3.5/importlib/_bootstrap.py:222: RuntimeWarning: compiletime version 3.4 of module ‘tensorflow.python.framework.fast_tensor_util’ does not match runtime version 3.5
    return f(*args, **kwds)
    /home/pi/env/lib/python3.5/importlib/_bootstrap.py:222: RuntimeWarning: builtins.type size changed, may indicate binary incompatibility. Expected 432, got 412
return f(*args, **kwds)




## 开发者列表

   左立晗：软件安装及环境搭建、小车线路连接、donkey car的数据采集、训练及自动驾驶实现

   王萍萍：软件安装及环境搭建、小车硬件安装、donkey car的数据采集、训练及自动驾驶实现

   王月新：软件安装及环境搭建、小车硬件安装、donkey car的数据采集、训练及自动驾驶实现

   王娇扬：软件安装及环境搭建、小车线路连接、donkey car的数据采集、训练及自动驾驶实现

   李默涵：软件安装及环境搭建、小车硬件安装、donkey car的数据采集、训练及自动驾驶实现

## 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md。
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)