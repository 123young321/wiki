
# 镜像的制作

## 基于CentOS:7.4.1708创建GitBook所需的环境

### 背景

随着公司的项目功能的迭代对所需的项目进行

### 下载镜像

1. 注册 `Docker ID` 下载官方镜像源
2. 在[Docker Hub](https://hub.docker.com/_/centos?tab=tags&page=1&ordering=last_updated)搜索自己所需要的镜像
3. 宿主机登录 `docker login` 按照提示输入用户名和密码登录
4. 下载镜像 `CentOS:7.4.1708` 镜像 `docker pull centos:centos7.4.1708`

查看本地镜像：
```
$ docker images
REPOSITORY   TAG        IMAGE ID       CREATED        SIZE
centos       latest     300e315adb2f   5 months ago   209MB
centos       7          8652b9f0cb4c   6 months ago   204MB
centos       7.4.1708   9f266d35e02c   2 years ago    197MB
```

说明：如果国内的镜像源可以拉取指定的 `Tag` 的镜像可以直接拉取镜像源，不需要上面的操作，但是在使用 `docker search centos:7.4.1708` 会搜索不到该镜像，所以推荐注册 `Docker ID` 进行拉取


### 下载安装包

#### 安装Nodejs

下载[Nodejs](https://nodejs.org/dist/latest-v12.x/)版本 `12.22.1`

```python
wget -O /data/soft/node-v12.22.1-linux-x64.tar.gz https://nodejs.org/dist/latest-v12.x/node-v12.22.1-linux-x64.tar.gz
```

#### 安装Nodejs
##### 创建安装目录
```python
mkdir /application
```
##### 解压安装目录
```
tar -zxvf /data/soft/node-v12.22.1-linux-x64.tar.gz -C /application/
```
##### 创建软连接
```
ln -s /application/node-v12.22.1-linux-x64/bin/* /usr/local/bin/
```

##### 查看版本
```
node -v
```

##### 添加环境变量

编辑 `/etc/profile` 添加如下内容
```
export NODE_HOME=/application/node-v12.22.1-linux-x64/
export PATH=$PATH:$NODE_HOME/bin
export NODE_PATH=$NODE_HOME/lib/node_modules
```
重载环境变量
```
source /etc/profile
```

#### 安装 GitBook

##### 使用 NPM 安装GitBook
```
npm install -g gitbook-cli
```
注意：基于NPM安装GitBook的安装路径
```
$ npm install -g gitbook-cli
/application/node-v12.22.1-linux-x64/bin/gitbook -> /application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/bin/gitbook.js
+ gitbook-cli@2.3.2
added 578 packages from 672 contributors in 15.358s
```
安装GitBook客户端

排查过程：命令找不到
```
$ gitbook -V
-bash: gitbook: command not found
```
安装GitBook报错命令找不到的主要原因：gitbook-cli 并没有在环境变量中
解决方案：
+ gitbook-cli的安装路径进行软连接
+ 或者将Nodejs添加到环境变量中

排查过程：安装罪行的`raceful-fs`安装包

```
$ gitbook -V
CLI version: 2.3.2
Installing GitBook 3.2.3
/application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:287
      if (cb) cb.apply(this, arguments)
                 ^

TypeError: cb.apply is not a function
    at /application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:287:18
    at FSReqCallback.oncomplete (fs.js:169:5)

```
参考地址：[gitbook-cli安装错误TypeError：cb.apply不是graceful-fs内部的函数](https://stackoverflow.com/questions/64211386/gitbook-cli-install-error-typeerror-cb-apply-is-not-a-function-inside-graceful)

```
cd /application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/
npm install graceful-fs@latest --save
```
##### 初始化书籍

```
$ mkdir /root/book && cd /root/book
$ gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```

##### 预览书籍

```
$ gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 1 pages
info: found 0 asset files
_stream_readable.js:637
  if (state.pipesCount === 1) {
            ^

TypeError: Cannot read property 'pipesCount' of undefined
    at ReadStream.Readable.pipe (_stream_readable.js:637:13)
    at /root/.gitbook/versions/3.2.3/node_modules/cpr/lib/index.js:163:22
    at callback (/application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/graceful-fs/polyfills.js:299:20)
    at FSReqCallback.oncomplete (fs.js:168:21)
```
参考解决方案：

+ [TypeError: Cannot read property 'pipesCount' of undefined #113](https://github.com/GitbookIO/gitbook-cli/issues/113)

```
修改graceful-fs的版本
cd /application/node-v12.22.1-linux-x64/lib/node_modules/gitbook-cli/node_modules/npm/node_modules/ && \
npm install graceful-fs@4.2.0 --save
```

##### 打包书籍
```
gitbook build
```

### 打包环境部署容器中

将安装的nodejs以及家目录的 .npm 和 .gitbook 放入到目录中
```
mkdir book
cp -r .gitbook/ book/
cp -r .npm/ book/
cp -r /application book/
```
打包
```
tar zcvf book.tar.gz book
```

### 创建容器

创建容器并登陆

```python
$ docker run -id --name=cdb-doc 9f266d35e02c
c356ac10cd34e6c21a47e3fa9065948fc0cd588688c1c9cafaf4466641c3274a

$ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
c356ac10cd34   9f266d35e02c   "/bin/bash"   4 seconds ago   Up 3 seconds             cdb-doc

$ docker exec -it cdb-doc /bin/bash
[root@c356ac10cd34 /]#
```

将 `book.tar.gz` 拷贝到容器中

```python
docker cp book.tar.gz cdb-doc:/root
```

登录容器中并解压

```python
tar xf book.tar.gz && cd book
mv application /
mv .npm $HOME
mv .gitbook $HOME
```

添加环境变量

```python
export NODE_HOME=/application/node-v12.22.1-linux-x64/
export PATH=$PATH:$NODE_HOME/bin
export NODE_PATH=$NODE_HOME/lib/node_modules
```

### 制作镜像

基本语法介绍
```python

docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

  -a :提交的镜像作者；
  -c :使用Dockerfile指令来创建镜像；
  -m :提交时的说明文字；
  -p :在commit时，将容器暂停。
```


```python
& docker commit -m "cdb-doc环境" -a "lipanpan" cdb-doc centos:7.4.1708

[root@localhost ~]# docker commit -m "cdb-doc环境" -a "lipanpan03" cdb-doc 9f266d35e02c
sha256:8932ccd60ec010b384888aaea0f6784341dedf00764c0261b7b3dc616553a8f4
```


```python
[root@docker-node1 ~]# docker commit -m "GitBook Deploy" -a "lipanpan<1299793997@qq.com>" ea5425258c62 9f266d35e02c
sha256:edb59af03a4623401cb5b496942b596681b45dda07d10972bd6dc7856b16c0bb

```


### 打包镜像

将镜像打Tag的基本指令：

```python
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

```python
& docker tag 9f266d35e02c lipanpan58/centos7.4.1708:v1
```

注意事项：名称前加上自己的 `docker hub` 的 `Docker ID` ，即：`lipanpan58` 下面的v1的tag标签可以不打，默认是latest



将镜像推送到 `docker hub` 中：
```
& docker push lipanpan58/centos7.4.1708:v1
```



# 参考地址
https://www.cnblogs.com/kevingrace/p/9599988.html

https://blog.csdn.net/weixin_41790086/article/details/102932185

https://blog.csdn.net/haiyangyiba/article/details/88805764


https://www.cnblogs.com/yangxiayi1987/p/11818130.html
