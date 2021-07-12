# lvche016

#### 介绍
swjtu
donkeycar项目实践

#### 软件架构
软件架构说明
在Windows下安装donkeycar
https://conda.io/miniconda.html

https://git-scm.com/download/win

git库:https://gitee.com/bloomlj/donkeycar


#### 安装教程（待补充）

1.  在windows上安装donkeycar

1.1安装miniconda python

1.2 安装git

1.3创建项目目录
              ·从开始菜单启动Anacanda

              ·创建你自己的项目目录


                ①`cd 你自己期望的目录文件下`

                ②`mkdir projects `    --创建一个项目文件

                ③`cd projects`

1.4从gitee上获取最新的donkeycar源码
                                 ·`git clone https://github.com/bloomlj/donkeycar`

   进入本地donkeycar文件下

                        ·`cd donkeycar`
                        ·`git checkout master` 

1.5更换国内源
            
```
             conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
             conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
             conda config --set show_channel_urls yes
```
1.6更新

       `conda update -n base -c defaults conda
        conda env remove -n donkey`

（有问题后再详细解决）

2.  创建python Anaconda环境
    
```
    conda env create -f install\envs\windows.yml  --这里windows.yml文件路径可能需要修改
    conda activate donkey
    pip install -e .[pc]
```

3.  安装Tensorflow-GPU

    `conda install tensorflow-gpu==1.13.1`

    ·创建工作目录： `donkey createcar --path D:/mycar`  --path表示路径

#### 使用说明

1.  xxxx
2.  xxxx
3.  xxxx

#### 参与贡献

1.  Fork 本仓库
2.  新建 Feat_xxx 分支
3.  提交代码
4.  新建 Pull Request


#### 特技

1.  使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2.  Gitee 官方博客 [blog.gitee.com](https://blog.gitee.com)
3.  你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解 Gitee 上的优秀开源项目
4.  [GVP](https://gitee.com/gvp) 全称是 Gitee 最有价值开源项目，是综合评定出的优秀开源项目
5.  Gitee 官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6.  Gitee 封面人物是一档用来展示 Gitee 会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
