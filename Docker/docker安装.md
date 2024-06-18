## Centos：

### 1. 前提条件：

- 系统为centos7以上，Linux内核为3.8以上

```bash
[root@master ~]# cat /etc/redhat-release
CentOS Linux release 7.9.2009 (Core)
[root@master ~]# uname -r
3.10.0-1160.el7.x86_64
```

### 2. CentOS 7（使用 yum 进行安装）：

#### 2.1. 安装必要的一些系统工具

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

#### 2.2. 添加软件源信息  

```bash
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sed -i 's+download.docker.com+mirrors.aliyun.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```

#### 2.3. 更新并安装docker-ce:  

```bash
yum makecache fast
yum -y install docker-ce
```

#### 2.4. 开启docker服务:

```bash
service docker start  --or  systemctl start docker
```



### 3. 安装其他docker版本：

- 官方软件源默认启用了最新的软件，可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，可以通过以下方式开启。同理可以开启各种测试版本等。

```bash
vim /etc/yum.repos.d/docker-ce.repo
# 将[docker-ce-test]下方的enabled=0修改为enabled=1
```

#### 3.1. 安装指定版本的docker-ce:

#### 3.2. 查找docker-ce的版本:  

```bash
yum list docker-ce.x86_64 --showduplicates | sort -r

Loading mirror speeds from cached hostfile
Loaded plugins: branch, fastestmirror, langpacks
docker-ce.x86_64 17.03.1.ce-1.el7.centos docker-ce-stable
docker-ce.x86_64 17.03.1.ce-1.el7.centos @docker-ce-stabl
docker-ce.x86_64 17.03.0.ce-1.el7.centos docker-ce-stable
Available Packages
```

#### 3.3. 安装指定版本的docker-ce: (VERSION例如上面的17.03.0.ce.1-1.el7.centos)



```bash
yum -y install docker-ce-[VERSION]
```



## Ubuntu：

#### 1.1. 安装必要的一些系统工具  

```bash
apt-get update
apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```



#### 1.2. 安装GPG证书  

```bash
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```





#### 1.3. 写入软件源信息  

```bash
add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```





#### 1.4. 更新并安装docker-ce  

```bash
apt-get -y update
apt-get -y install docker-ce
```



### 1. 安装指定版本的docker-ce:

#### 1.1. 查找docker-ce的版本:

```bash
apt-cache madison docker-ce

docker-ce | 17.03.1~ce-0~ubuntu-xenial | https://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
docker-ce | 17.03.0~ce-0~ubuntu-xenial | https://mirrors.aliyun.com/docker-ce/linux/ubuntu xenial/stable amd64 Packages
```



#### 1.2. 安装指定版本的docker-ce: (VERSION例如上面的17.03.1~ce-0~ubuntu-xenial)  

```bash
apt-get -y install docker-ce=[VERSION]
```



#### 1.3. 安装校验：  

```bash
root@iZbp12adskpuoxodbkqzjfZ:$ docker version


Client:
Version: 17.03.0-ce
API version: 1.26
Go version: go1.7.5
Git commit: 3a232c8
Built: Tue Feb 28 07:52:04 2017
OS/Arch: linux/amd64
Server:
Version: 17.03.0-ce
API version: 1.26 (minimum version 1.12)
Go version: go1.7.5
Git commit: 3a232c8
Built: Tue Feb 28 07:52:04 2017
OS/Arch: linux/amd64
Experimental: false
```

## 镜像加速

```bash
vim  /etc/docker/daemon.json
or
tee /etc/docker/daemon.json <<-'EOF'
{  
"registry-mirrors": ["https://b81vthfh.mirror.aliyuncs.com"]
}
EOF

systemctl daemon-reload
systemctl restart docker
```

## 卸载：

```bash
yum -y remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

###   
