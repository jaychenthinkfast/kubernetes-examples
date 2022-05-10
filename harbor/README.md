- [Harbor](#harbor)
  - [前置准备](#前置准备)
  - [安装harbor](#安装harbor)
    - [1.添加仓库](#1添加仓库)
    - [2.拉取包到本地并解压](#2拉取包到本地并解压)
    - [3.进入包目录](#3进入包目录)
    - [4.修改value.yaml](#4修改valueyaml)
    - [5.替换域名证书(可选)](#5替换域名证书可选)
    - [6.安装](#6安装)
    - [7.验证](#7验证)
  - [卸载harbor](#卸载harbor)
  - [参考](#参考)

# Harbor 

## 前置准备
因为使用helm v3 安装需要先安装helm

这里采用二进制安装方式

1. 下载 [需要的版本](https://github.com/helm/helm/releases)
2. 解压(tar -zxvf helm-v3.0.0-linux-amd64.tar.gz)
3. 在解压目中找到helm程序，移动到需要的目录中(mv linux-amd64/helm /usr/local/bin/helm)

## 安装harbor
### 1.添加仓库
```
helm repo add harbor https://helm.goharbor.io
```
### 2.拉取包到本地并解压
```
helm fetch harbor/harbor --untar
```
### 3.进入包目录
```
cd harbor
```
### 4.修改value.yaml
建议使用ingress暴露域名

需修改host为你使用的实际域名，修改className为你实际使用的IngressController
```yaml
  expose:
    ingress:
      hosts:
        core: harbor.chenjie.info
        notary: notary.harbor.chenjie.info
      className: ""
  externalURL: https://harbor.chenjie.info
```
### 5.替换域名证书(可选)
如果有申请权威证书可在cert目录下的证书文件或者后期修改相应的secret

### 6.安装
```
kubectl create ns harbor
helm install -n harbor harbor-helm .
```
安装后输出：
```
NAME: harbor-helm
LAST DEPLOYED: Tue May 10 10:17:41 2022
NAMESPACE: harbor
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Please wait for several minutes for Harbor deployment to complete.
Then you should be able to visit the Harbor portal at https://harbor.chenjie.info
For more details, please visit https://github.com/goharbor/harbor
```

pod 状态 Running为正常
```
kubectl get pods -n harbor -w
```
### 7.验证
浏览器访问https://harbor.chenjie.info (之前配置的域名，如果不是真实域名需本地host解析静态ip）

账号：admin

密码：Harbor12345

然后可以在本地login 输入账号密码登录
```
docker login harbor.chenjie.info
```

## 卸载harbor
```
helm uninstall -n harbor harbor-helm 
```

## 参考
1. https://helm.sh/zh/docs/intro/install/
2. https://goharbor.io/docs/edge/install-config/harbor-ha-helm/
3. https://goharbor.io/docs/edge/install-config/configure-yml-file/






