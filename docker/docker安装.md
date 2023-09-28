# 安装

1.设置仓库地址
```
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

2.安装 Docker Engine
```
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

3.启动docker
```
sudo systemctl start docker
```