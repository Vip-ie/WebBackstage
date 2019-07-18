## MacOSX 安装python虚拟环境

### python3安装

官方下载python3安装包

https://www.python.org/downloads/

```
pip3 install virtualenvwrapper
```

## 环境配置

vi ~/.bash\_profile

打开终端使用mdfind -name “python3”查找python3详细安装路径与virtualenvwrapper.sh

```
/Library/Frameworks/Python.framework/Versions/3.7/bin/python3
/Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
```

插入以下命令

```
export WORKON_HOME='~/.virtualenvs'
VIRTUALENVWRAPPER_PYTHON='/Library/Frameworks/Python.framework/Versions/3.7/bin/python3'
source /Library/Frameworks/Python.framework/Versions/3.7/bin/virtualenvwrapper.sh
```

保存后执行

```
source ~/.bash_profile
```

virtualenvwrapper创建虚拟环境

```
mkvirtualenv tornado1
```

切换虚拟环境

```
workon tornado1
```

退出虚拟环境

```
deactivate
```

## Mysql启动

打开终端执行`PATH="$PATH":/usr/local/mysql/bin`

终端登录到MySQL的命令 `mysql -u root -p leva8888;`

