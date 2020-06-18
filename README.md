# 用helm安装Monocular

## 来源

https://github.com/helm/monocular

https://github.com/helm/monocular/blob/master/chart/monocular/README.md

https://jimmysong.io/kubernetes-handbook/practice/create-private-charts-repo.html



## 其他方法

1. 用GitHub pages托管charts    
   https://pages.github.com/
2. 用Minio 托管 charts  
   https://juejin.im/post/5d5b8dba6fb9a06b1b19c21b
   https://github.com/zknyy/helm-minio-install

## 前提

1. 安装 Kubernetes（忽略）
2. 安装Ingress  （忽略）
3. 安装Helm   （忽略）

## 安装

> 按照 https://github.com/helm/monocular/blob/master/chart/monocular/README.md 中的方法安装

```shell
$ helm repo add monocular https://helm.github.io/monocular
$ helm install monocular monocular/monocular
```

> 注意不要用下面这种URL

```shell
$ helm repo add monocular https://kubernetes-helm.github.io/monocular
```

得到

```
"monocular" has been added to your repositories

```

