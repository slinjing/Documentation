# 修改docker默认存储路径

### 1. 查看默认路径

```bash
docker info | grep Dir
```

<img src="F:\docs\img\1702864999440-a59c0db4-b232-41b3-9dff-05bab36b7516.png" alt="img" style="zoom:200%;" />

### 2. 停止docker服务

```bash
systemctl stop docker
```

### 3. 编辑/etc/docker/daemon.json文件

```bash
vim /etc/docker/daemon.json

{
"data-root": "/home/docker"  # 新存储路径
}
```

<img src="F:\docs\img\1702865703462-29097bbb-109a-44df-a8d2-433a8622d060.png" alt="img" style="zoom:200%;" />

### 4. 目录迁移

```bash
mkdir -p /home/docker
rsync -avz /var/lib/docker /home/docker
```

若提示-bash: rsync: command not found，需先安装rsync

<img src="F:\docs\img\1702865755300-af9ea98b-7bb9-421d-b790-68cb7ae031e8.png" alt="img" style="zoom:200%;" />

```bash
yum -y install rsync
```

或者使用mv命令也可以

```bash
mv /var/lib/docker /home/docker
```

### 5. 重启docker

```bash
systemctl restart docker
```

### 6. 检查验证

```bash
docker info | grep Dir
docker ps -a
docker images
```

<img src="F:\docs\img\1702866086566-308fe808-7f59-4372-92c6-c0e8da0a2b90.png" alt="img" style="zoom:200%;" />

### 7. 删除/var/lib/docker/目录中的文件

检查没问题之后删除原目录下的文件

```bash
rm -rf /var/lib/docker/*
```
