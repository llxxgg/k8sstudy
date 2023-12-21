# install

查看istio版本与k8s版本支持情况：
> https://istio.io/latest/zh/docs/releases/supported-releases/

安装步骤：
```shell

curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.18.5 sh -  

export PATH=$PWD/bin:$PATH

istioctl install --set profile=demo -y

kubectl label namespace default istio-injection=enabled

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

```

参考文档：
> https://istio.io/latest/docs/setup/getting-started/
