# 用Helm安装Monocular

## 来源

https://github.com/helm/monocular

https://github.com/helm/monocular/blob/master/chart/monocular/README.md

https://jimmysong.io/kubernetes-handbook/practice/create-private-charts-repo.html



## Helm仓库

制作好的charts包可以上传至helm仓库

1. 公共仓库AppHub：不仅为国内用户实时同步了官方 Helm Hub 里的所有应用，还自动替换了这些 Charts 里所有不可访问的镜像 URL（比如 gcr.io, quay.io 等），终于使得国内开发者通过 helm install “一键安装”应用成为了可能。
2. 自己自建私有仓库，例如：kubeapps，Monocular，minio 等，可以利用helm命令一键安装。

### 公共Helm仓库：AppHub

> 发现无法自由上传Chart，就算是上传也需要经过审核才能使用

阿里云 开放云原生应用中心 - Cloud Native App Hub： https://developer.aliyun.com/hub    https://github.com/cloudnativeapp

来源：https://github.com/cloudnativeapp/workshop/tree/master/kubecon2019china/charts/guestbook#installing-helm-v3

Add the repository to your local environment:

```shell
$ helm repo add apphub https://apphub.aliyuncs.com
```

To install the chart with release name of `guestbook`:

```shell
$ helm install guestbook apphub/guestbook
```

### 公共Helm仓库：GitHub Page

用GitHub pages托管charts     https://pages.github.com/

### 私有Helm仓库：Minio

用Minio 托管 charts  
https://juejin.im/post/5d5b8dba6fb9a06b1b19c21b
https://github.com/zknyy/helm-minio-install

## 前提

1. 安装 Kubernetes（忽略）
2. 安装Helm   （忽略）
3. 安装Ingress  

## 安装Ingress  

根据 https://kubernetes.github.io/ingress-nginx/deploy/ 中的方法安装Ingress，执行

```shell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

得到

```shell
"ingress-nginx" has been added to your repositories
```


执行

```shell
helm install ingress-nginx ingress-nginx/ingress-nginx
```

得到

```yaml
NAME: ingress-nginx
LAST DEPLOYED: Thu Jun 18 20:00:55 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The ingress-nginx controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 'kubectl --namespace default get services -o wide -w ingress-nginx-controller'

An example Ingress that makes use of the controller:

  apiVersion: networking.k8s.io/v1beta1
  kind: Ingress
  metadata:
    annotations:
      kubernetes.io/ingress.class: nginx
    name: example
    namespace: foo
  spec:
    rules:
      - host: www.example.com
        http:
          paths:
            - backend:
                serviceName: exampleService
                servicePort: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
        - hosts:
            - www.example.com
          secretName: example-tls

If TLS is enabled for the Ingress, a Secret containing the certificate and key must also be provided:

  apiVersion: v1
  kind: Secret
  metadata:
    name: example-tls
    namespace: foo
  data:
    tls.crt: <base64 encoded cert>
    tls.key: <base64 encoded key>
  type: kubernetes.io/tls
```

根据上面的提示执行

```shell
kubectl --namespace default get services -o wide -w ingress-nginx-controller
```

得到

```
NAME                       TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE   SELECTOR
ingress-nginx-controller   LoadBalancer   10.111.253.100   localhost     80:30519/TCP,443:31520/TCP   10m   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
```



## 安装Monocular

> 按照 https://github.com/helm/monocular/blob/master/chart/monocular/README.md 中的方法安装

```shell
$ helm repo add monocular https://helm.github.io/monocular
```

> 注意不要用下面这种URL

```shell
$ helm repo add monocular https://kubernetes-helm.github.io/monocular
```

得到

```
"monocular" has been added to your repositories
```

执行安装

```shell
$ helm install monocular monocular/monocular
```

得到

```shell
NAME: monocular
LAST DEPLOYED: Thu Jun 18 20:15:46 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The Monocular chart sets up an Ingress to serve the API and UI on the same
domain. You can get the address to access Monocular from this Ingress endpoint:

  $ kubectl --namespace default get ingress monocular-monocular

Point your Ingress hosts to the address from the output of the above command:
  - null
Visit https://github.com/helm/monocular for more information.
```

根据提示执行

```shell
kubectl --namespace default get ingress monocular-monocular
```

得到

```
NAME                  HOSTS   ADDRESS     PORTS   AGE
monocular-monocular   *       localhost   80      64s
```

浏览器访问 http://localhost/   得到

![img](https://raw.githubusercontent.com/zknyy/helm-monocular-install/master/monocular.png)



## 上传[TODO]