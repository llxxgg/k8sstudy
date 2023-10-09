# k8s安装

关闭防火墙

```shell
systemctl disable firewalld
systemctl stop firewalld
```

安装etcd和kubernetes软件(会自动安装docker)
``yum install -y etcd kubernetes``

安装k8s过程中出现如下错误：
```
Error: docker conflicts
Error: docker-ce-cli conflicts with 2:docker-1.13.1-209.git7d71120.el7.centos.x86_64
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```
解决办法： 系统中已经安装有docker-ce，卸载后可正常安装k8s
```
yum -y remove docker-ce
yum -y remove docker-ce-cli
```

**需要修改两处配置**

> Docker 配置文件/etc/sysconfig/docker , OPTIONS='--seliunx-enabled=false --insecure-registry gcr.io'
> kubernetes apiservce 配置文件/etc/kubernetes/apiserver,把--admission_control参数中的ServiceAccount删除。

按下面的顺序启动所有服务
```shell
systemctl start etcd
systemctl start docker
systemctl start kube-apiserver
systemctl start kube-controller-manager
systemctl start kube-scheduler
systemctl start kubelet
systemctl start kube-proxy
```

安装完成：```ps -ef|grep kube```查看一下

服务停止
```shell
systemctl stop kube-proxy
systemctl stop kubelet
systemctl stop kube-scheduler
systemctl stop kube-controller-manager
systemctl stop kube-apiserver
systemctl stop docker
systemctl stop etcd
```

可以开始使用了！

测试k8s：myweb-rc.yaml
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myweb
spec:
  replicas: 5
  selector:
    app: myweb
  template:
    metadata:
      labels:
        app: myweb
    spec:
      containers:
      - image: kubeguide/tomcat-app:v1
        name: myweb
        resources:
          limits:
            cpu: "2"
            memory: 4Gi
        ports:
        - containerPort: 8080
        env:
        - name: MYSQL_SERVICE_HOST
          value: 'mysql'
        - name: MYSQL_SERVICE_PORT
          value: '3306'

```
