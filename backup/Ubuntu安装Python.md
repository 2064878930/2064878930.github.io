# Ubuntu安装Python

出现 ` 权限问题` 进入超级用户

```
sudo -i
```

------

安装依赖

```
apt-get install gcc
apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev  libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev   xz-utils tk-dev
```

然后下载python版本

```
wget -c https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tgz
```

将安装包移动到指定目录下

```
mv Python-3.6.2.tgz /usr/local
```

解压安装包并重命名

```
cd /usr/local
tar -xvzf Python-3.6.2.tgz
mv Python-3.6.2 Python
```

进入Python并安装

```
cd Python 
# 添加配置
./configure --prefix=/usr/python
# 安装Python 
make&&make install
apt-get install install python3-pip
```

新建软链接

```
ln -s /usr/python/bin/python3 /usr/bin/python
ln -s /usr/python/bin/pip  /usr/bin/pip
```

指定pip安装镜像为国内源（阿里源）

```
# 进入用户目录
cd 
mkdir .pip
vi pip.conf
# 将以下内容写入到pip.conf文件中
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

