## [CentOS 7.4 安装python3及虚拟环境](/h ttps://www.centos.bz/2018/05/centos-7-4-安装python3及虚拟环境/)

由于写了个爬虫脚本，需要放到服务器中运行。之前一直在[Ubuntu](https://www.centos.bz/category/other-system/ubuntu/)系统中安装多[Python](https://www.centos.bz/category/python/)环境，而[CentOS](https://www.centos.bz/tag/centos/)

系统的安装步骤略微有些出入，故详细记录下这几天趟过的坑。

## 说明 {#说明}

1.本文的系统命令一般会在语句前加上\#号，以区分系统命令及其他内容。输入命令时，无需输入\#号。

```
# yum install vim
```

2.本文系统输出的信息，会在前面加上&gt;&gt;号。

```
# which python
>> /usr/bin/python    # 系统输出的信息
```

3.本文的系统命令都是在root账号下执行的，假如非root账号执行，提示没有权限，可在命令前加sudo。

    # yum install vim    #root账号下执行命令
    # sudo yum install vim    #非root账号下执行管理员权限命令，需在命令前加`sudo`

4.安装环境

系统版本：CentOS 7.4（自带Python2.7）  
安装版本：Python3.6  
安装插件：virtualenv、virtualenvwrapper

## 一、安装Python3 {#一、安装Python3}

由于[CentOS7](https://www.centos.bz/tag/centos7/)原本就安装了Python2，而且这个Python2不能被删除，因为有很多系统命令，比如yum都要用到。所以我们要额外安装Python3，而且系统一般允许多个版本的python同时存在。

我们先来查看python安装位置，一般是位于/usr/bin/python目录下。

```
# which python
>> /usr/bin/python
```

下面介绍安装Python3的方法：

### 1. 安装依赖包（切记安装） {#1. 安装依赖包（切记安装）}

```
# yum -y groupinstall "Development tools"
# yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

### 2.下载Python3安装包 {#2.下载Python3安装包}

大家可根据自己需求下载不同版本的Python3，我下载的是Python3.7.0

```
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
tar -xvJf  Python-3.7.0.tar.xz
```

### 3. 新建python3存放目录 {#3. 新建python3存放目录}

```
# mkdir /usr/local/python3
```

### 4. 安装Python3 {#4. 安装Python3}

解压压缩包，进入解压目录，指定安装目录，安装Python3。

```
mkdir /usr/local/python3 #创建编译安装目录
cd Python-3.7.0
./configure --prefix=/usr/local/python3
make && make install
```

安装Python3时，会自动安装pip。假如没有，需要自己手动安装。

```
# yum -y install python-pip
```

### 5. 创建软链接 {#5. 创建软链接}

```
# ln -s /usr/local/python3/bin/python3 /usr/bin/python3
# ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

### 6. 安装完成，输入python3测试 {#6. 安装完成，输入python3测试}

```
python
```

## 二、创建虚拟环境 {#二、创建虚拟环境}

virtualenv是一个可以在同一计算机中隔离多个python版本的工具。有时，两个不同的项目可能需要不同版本的python，如 python2.7 / python3.6 ，但是如果都装到一起，经常会导致问题。virtualenv能够用于创建独立的Python虚拟环境，多个Python相互独立，互不影响。  
virtualenvwrapper这个软件包可以让我们管理虚拟环境变得更加简单。不用再跑到某个目录下通过virtualenv来创建虚拟环境，并且激活的时候也要跑到具体的目录下去激活。

下面介绍安装python虚拟环境的方法：

使用pip安装包前，先更新pip。

```
# pip3 install --upgrade pip
```

**1. 安装virtualenv、virtualenvwrapper**

```
# pip3 install virtualenv
# pip3 install virtualenvwrapper
```

**2. 进入.bashrc文件中，定义virtualenvwrapper路径**

使用vim编辑.bashrc文件

```
# vim ~/.bashrc
```

在文末填入以下代码并保存

```
VIRTUALENVWRAPPER_PYTHON=/usr/local/python3/bin/python3    # 指定virtualenvwrapper执行的python版本
export WORKON_HOME=$HOME/.virtualenvs    # 指定虚拟环境存放目录，.virtualenvs目录名可自拟
source /usr/local/bin/virtualenvwrapper.sh    # virtualenvwrapper.sh所在目录
```

**3. 运行.bashrc文件**

```
# source ~/.bashrc
```

**4. 创建虚拟环境**

```
# mkvirtualenv py3-env
```

也可指定虚拟环境的python版本

```
# mkvirtualenv --python=/usr/bin/python3 py3-env
```

**5. 进入虚拟环境中，然后进入到项目所在目录，安装好相应的包（如无需要，可跳过此步）**

```
#  pip install -r requirements.txt
```

虚拟环境搭建完成！

**常见的virtualenvwrapper命令**

* 创建虚拟环境

```
# mkvirtualenv my_env
```

* 切换到某个虚拟环境

```
# workon my_env
```

* 退出当前虚拟环境

```
# deactivate
```

* 删除某个虚拟环境

```
# rmvirtualenv my_env
```

* 列出所有虚拟环境

```
# lsvirtualenv
```

* 进入到虚拟环境所在的目录

```
# cdvirtualenv
```

三、异常情况

* 假如source ~/.bashrc时，提示以下错误

```
# source ~/.bashrc
>>-bash: /usr/local/bin/virtualenvwrapper.sh: No such file or directory
```

**【原因】**  
.bashrc文件中的virtualenvwrapper.sh所在目录错误。

**【解决方案】**  
①查找virtualenvwrapper.sh所在目录

```
# find / -name "virtualenvwrapper.sh"
>>/usr/local/python3/bin/virtualenvwrapper.sh
```

②把.bashrc文件的virtualenvwrapper.sh目录更改为实际所在目录

```
source /usr/local/python3/bin/virtualenvwrapper.sh    # virtualenvwrapper.sh实际所在目录
```

* 假如创建虚拟环境时，提示以下错误

```
# mkvirtualenv my_env
>> ERROR: virtualenvwrapper could not find virtualenv in your path
```

**【解决方案】**

①查找virtualenv所在目录

```
# find / -name "virtualenv"
>> /usr/local/python3/bin/virtualenv
```

②创建软链接

```
#  ln -s /usr/local/python3/bin/virtualenv /usr/local/bin/virtualenv
```

## 二、创建虚拟环境 {#二、创建虚拟环境}

virtualenv是一个可以在同一计算机中隔离多个python版本的工具。有时，两个不同的项目可能需要不同版本的python，如 python2.7 / python3.6 ，但是如果都装到一起，经常会导致问题。virtualenv能够用于创建独立的Python虚拟环境，多个Python相互独立，互不影响。  
virtualenvwrapper这个软件包可以让我们管理虚拟环境变得更加简单。不用再跑到某个目录下通过virtualenv来创建虚拟环境，并且激活的时候也要跑到具体的目录下去激活。

下面介绍安装python虚拟环境的方法：

使用pip安装包前，先更新pip。

```
# pip3 install --upgrade pip
```

**1. 安装virtualenv、virtualenvwrapper**

```
# pip3 install virtualenv
# pip3 install virtualenvwrapper
```

**2. 进入.bashrc文件中，定义virtualenvwrapper路径**

使用vim编辑.bashrc文件

```
# vim ~/.bashrc
```

在文末填入以下代码并保存

```
VIRTUALENVWRAPPER_PYTHON=/usr/local/python3/bin/python3    # 指定virtualenvwrapper执行的python版本
export WORKON_HOME=$HOME/.virtualenvs    # 指定虚拟环境存放目录，.virtualenvs目录名可自拟
source /usr/local/bin/virtualenvwrapper.sh    # virtualenvwrapper.sh所在目录
```

**3. 运行.bashrc文件**

```
# source ~/.bashrc
```

**4. 创建虚拟环境**

```
# mkvirtualenv py3-env
```

也可指定虚拟环境的python版本

```
# mkvirtualenv --python=/usr/bin/python3 py3-env
```

**5. 进入虚拟环境中，然后进入到项目所在目录，安装好相应的包（如无需要，可跳过此步）**

```
#  pip install -r requirements.txt
```

虚拟环境搭建完成！

**常见的virtualenvwrapper命令**

* 创建虚拟环境

```
# mkvirtualenv my_env
```

* 切换到某个虚拟环境

```
# workon my_env
```

* 退出当前虚拟环境

```
# deactivate
```

* 删除某个虚拟环境

```
# rmvirtualenv my_env
```

* 列出所有虚拟环境

```
# lsvirtualenv
```

* 进入到虚拟环境所在的目录

```
# cdvirtualenv
```

三、异常情况

* 假如source ~/.bashrc时，提示以下错误

```
# source ~/.bashrc
>>-bash: /usr/local/bin/virtualenvwrapper.sh: No such file or directory
```

**【原因】**  
.bashrc文件中的virtualenvwrapper.sh所在目录错误。

**【解决方案】**  
①查找virtualenvwrapper.sh所在目录

```
# find / -name "virtualenvwrapper.sh"
>>/usr/local/python3/bin/virtualenvwrapper.sh
```

②把.bashrc文件的virtualenvwrapper.sh目录更改为实际所在目录

```
source /usr/local/python3/bin/virtualenvwrapper.sh    # virtualenvwrapper.sh实际所在目录
```

* 假如创建虚拟环境时，提示以下错误

```
# mkvirtualenv my_env
>> ERROR: virtualenvwrapper could not find virtualenv in your path
```

**【解决方案】**

①查找virtualenv所在目录

```
# find / -name "virtualenv"
>> /usr/local/python3/bin/virtualenv
```

②创建软链接

```
#  ln -s /usr/local/python3/bin/virtualenv /usr/local/bin/virtualenv
```



