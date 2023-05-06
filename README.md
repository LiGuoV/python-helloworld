# python-helloworld
[创建DockerHub 令牌](https://www.docker.com/blog/docker-hub-new-personal-access-tokens/)  
[创建GitHub 密钥](https://www.docker.com/blog/docker-hub-new-personal-access-tokens/) 设置用于action的存储库变量,环境变量  
[Build and Push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images) marketplace中的上游操作  
创建`/.github/workflows/foo.yaml`


> 练习 创建github action 可以构建推送app的dockerImage  
满足以下要求  
- Image name: python-helloworld
- Tag: latest
- Platforms: platforms: linux/amd64,linux/arm64

 
 > 练习2  
 ```shell
vagrant up  
vagrant ssh
```
安装 [k3s](https://k3s.io/)  
```
kubectl get node
kubeclt gry po -A
```
安装[ArgoCD](https://argo-cd.readthedocs.io/en/stable/)

```
kubectl get pods -n argocd
```
接下来访问AragoCD应用
```
# 获取命名空间下所有服务 
kubectl get svc -n argocd
```
找到端口从vritualBox暴露给主机
复制清单更改为节点端口 用nodeport公开argoCD服务器
```shell
kubectl get svc -n argocd argocd-server -o yaml > argocd-nodeport.yaml
```
修改配置参考[kubernetes配置](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport)  
最终文件[argocd-nodeport.yaml](/argocd/argocd-server-nodeport.yaml)

应用配置： `kubectl apply -f argocd-nodeport.yaml `  

检查 `kubectl get svc -n argocd argocd-server`

vitrualBox有一个静态IP(vagratefile中有配置)加端口号[登录](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)  
[需要applicationCRD for argo](/argocd-python.yaml)
这个文件指定了目标集群和清单来源
`kubectl apply -f argocd-python.yaml`  
`kubectl get app -n argocd `
