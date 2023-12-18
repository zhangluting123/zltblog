---
title: Ubuntu
date: 2023-11-21 16:46:07(创建时间)
tags: Linux
categories: 基础知识
description: 描述
comments: true
---

> hhhh


查看系统版本

```
cat /etc/issue
```

****

## 1. 安装Ubuntu：

[https://blog.csdn.net/qq_38189484/article/details/105237757](https://blog.csdn.net/qq_38189484/article/details/105237757)

[https://blog.csdn.net/qq_41349068/article/details/115456013](https://blog.csdn.net/qq_41349068/article/details/115456013)

### 1.1  软件源

[https://www.cnblogs.com/kennywjt/p/13035367.html](https://www.cnblogs.com/kennywjt/p/13035367.html)

Ubuntu 的软件源配置文件是?/etc/apt/sources.list。将系统自带的该文件做个备份，将该文件替换为下面内容，即可使用。

1. 备份：

```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```
2. 修改配置（见sources.list内容）：

```bash
sudo gedit /etc/apt/sources.list
```

3. 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释

```bash
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
```

4. 更新源

```bash
sudo apt-get update
```

### 1.2  添加环境变量

```bash
# 查看环境变量
echo $PATH

# ===临时设置环境变量，当前bash生效===
export PATH=路径:$PATH # 或 export PATH=$PATH:路径
    
# ===永久设置环境变量(1)===
sudo gedit /etc/environment
# 文件后面加":路径"并保存
source /etc/environment # 使其生效，仅在当前bash
# 启动新bash不生效，重启系统后全局生效

# ===永久设置环境变量(2)===
vim /etc/profile
# 添加
export JAVA_HOME=/usr/local/java/jdk1.8  #按自己的路径填
export PATH=$PATH:$JAVA_HOME/bin
# 让配置文件生效
source /etc/profile
# 查看版本
java -version
```

### 1.3  创建用户授予sudo权限

```bash
# 在home路径下创建用户，创建文件夹test
useradd -d /home/test -m test
# 设置密码
passwd test
# 命令行的模式换为bash，默认是sh
usermod -s /bin/bash test
# 把test用户添加到sudo和admin用户组里面
usermod -a -G sudo test
usermod -a -G adm test
# 检查test所在用户组
groups test
# 验证是否成功
sudo su
```

## 2. 安装软件

### 2.1  软件压缩包

```bash
tar zxf pycharm-community-2018.3.tar.gz       ##解压
# xvf   .tar.bz2
# -z或--gzip或--ungzip 通过gzip指令处理备份文件
# -v或--verbose 显示指令执行过程。

cd  pycharm-community-2018.3/bin
./pycharm.sh                                 ##运行pycharm安装脚本
```

### 2.2  python+pip

安装pip

```bash
# 安装默认pip  (9.0.1版本，对应python2.7)
apt install python-pip / python3-pip
# 卸载默认pip
apt remove python-pip

# 升级pip   (21.3.1版本，对应python3)
python -m pip install --upgrade pip 
# 卸载升级的pip 
pip uninstall pip

# 修改python默认版本
$ which python
/usr/bin/python
$ sudo rm /usr/bin/python
$ python
~bash: /usr/bin/python: No such file or directory
$ ln -s /usr/bin/python3.6 /usr/bin/python
$ python -V
Python 3.6.9

# 修改pip默认版本
$ which pip3
$ rm /usr/bin/pip
$ ln -s /usr/local/bin/pip3 /usr/bin/pip
```

pip安装库到对应python版本

```bash
# 环境变量加别名[可选]
echo "alias py27='usr/bin/python2.7'" >> ~/.bash_aliases && source ~/.bash_aliases
echo "alias py36='usr/bin/python3.6'" >> ~/.bash_aliases && source ~/.bash_aliases
# 使用（安装到/usr/local/lib/python版本号/dist-packages下）
py27 -m pip install 库
```

python更换版本保留以前库

```bash
# 保存
pip freeze > library.txt
# ---安装python---
# 重新安装
pip install -r library.txt
```

 pip指定安装位置

```bash
pip install xxx --target='路径'
```

### 2.3 Docker 安装

[https://www.runoob.com/docker/ubuntu-docker-install.html](https://www.runoob.com/docker/ubuntu-docker-install.html)

```bash
sudo apt-get install docker-ce=5:20.10.16~3-0~ubuntu-bionic docker-ce-cli=5:20.10.16~3-0~ubuntu-bionic containerd.io
```

### 2.4 cmake安装

```shell
apt install cmake
```

```bash
# apt install wget
# sudo apt-get install -y build-essential
# sudo apt-get install libssl-dev  # 缺少openssl库 
sudo wget https://cmake.org/files/v3.22/cmake-3.22.0-rc2-linux-x86_64.tar.gz
sudo tar -zxvf cmake-3.22.0-rc2-linux-x86_64.tar.gz
cd cmake-3.22.0-rc2-linux-x86_64/
# 创建软链接
sudo ln -sf /opt/cmake-3.22.0-rc2-linux-x86_64/bin/* /usr/bin/
cmake --version
```

### 2.5 安装库和C++

```bash
apt install build-essential
apt install gdb # 调试
which gcc 
# 安装uuid
apt install uuid-dev
# 安装gtest
git clone https://github.com/google/googletest.git
cd googletest
mkdir build && cd build && cmake .. && make -j4
sudo make install
sudo ldconfig
```

### 2.6 安装Clangd

```shell
apt-get install clangd
sudo apt-get install clangd-12  # clangd-9
```

使用`cmd+shift+p`输入`clangd: download`

下载最新的clangd（Download language server） ，然后重启就可以使用了

### 2.7 java安装

```bash
sudo apt install default-jre
java -version
sudo apt install default-jdk
javac -version
```

### 2.8 Redis 安装

Remote Dictionary Server，高性能key-value数据库

```bash
apt-get install redis
# 启动服务
systemctl start redis
# 查看服务
ps -ef|grep 6379
# 查看服务状态
systemctl status redis
# 停止服务
systemctl stop redis
# python调用redis的API, 需要安装python库
pip install redis
# 查看是否安装成功
import redis

# redis 取出的结果默认是字节，我们可以设定 decode_responses=True 改成字符串。
redis_conn = redis.Redis(host='127.0.0.1', port= 6379 , db= 0)
redis_conn.set('key','Hello redis')
print(redis_conn.get('key')）
```

## tips

### 解锁带锁的文件

```bash
sudo chmod -R 777 目录名/文件名
```

###  vim上下左右删除键失效

```bash
# 卸载原来的vim：
sudo apt-get remove vim-common
# 安装新的vim：
sudo apt-get install vim
```
