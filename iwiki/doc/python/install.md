# 安装

## Linux安装Python3.x

### 安装依赖

1. 首先安装 `gcc` 编译器, 通过 `gcc --version ` 查看是否已经安装，如果没安装的先安装 `yum -y install gcc`
2. 安装其他依赖包(注意：不要缺少，否则有可能安装python出错，python3.7.0以下的版本可不装 libffi-devel)

```python
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel
```

### 下载源码包

1. [源码包地址](https://www.python.org/ftp/python/) 根据需求下载需要的版本

2. 下载Python3.7.0安装包

```python
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
```

### 解压

```python
tar -zxvf Python-3.7.0.tgz
```

### 安装目录

```python
mkdir /usr/local/python3.7
```

### 编译, 编译安装

```python
cd Python-3.7.0
./configure --prefix=/usr/local/python3.7
make && make install
```

### 建立软连接

```python
ln -s /usr/local/python3.7/bin/python3.7 /usr/bin/python3.7
ln -s /usr/local/python3.7/bin/pip3.7 /usr/bin/pip3.7
```
