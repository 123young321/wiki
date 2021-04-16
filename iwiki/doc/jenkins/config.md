

# Jenkins基本配置

## 配置Jenkins插件下载地址
```shell
cd /var/lib/jenkins/updates &&
sed -i 's/http:\/\/updates.jenkins-ci.org\/download/https:\/\/mirrors.tuna.tsinghua.edu.cn\/jenkins/g' default.json && 
sed -i 's/http:\/\/www.google.com/https:\/\/www.baidu.com/g' default.json && 
systemctal restart jenkins
```

