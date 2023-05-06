# 从单体应用到微服务
# 容器化,到镜像DockerHub
# 再到使用 Kubernetes 进行容器编排
# 从集群到PaaS
![](/image/faas.png)
 PaaS 解决方案可将工程工作从基础设施管理转移到产品开发。此外，它在应用程序的整个发布过程中提供了强大的开发人员体验，其中主要功能是通过 UI 或控制台使用的。但是，此产品有一个主要缺点：供应商和应用程序目录锁定。所以有了开源Paas CloudFundry
 其主要组件可以在集群中作为 pod 运行  
低交互的应用占用资源，不符合成本效益 进而有了FaaS

 

[Cloud Foundry 概述](https://docs.cloudfoundry.org/concepts/overview.html)  
[Stratos 简介](https://stratos.app/)  
[Cloud Foundry：使用应用程序清单进行部署](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)  
[将 Cloud Foundry Hello World 应用程序迁移到 Kubernetes——这有多难？](https://www.suse.com/c/moving-a-cloud-foundry-hello-world-app-to-kubernetes-src/)

# CICD管道
一旦团队开发了产品，自动化发布过程就变得至关重要   
应用程序的构建、测试、打包和部署到多个集群应该是高度自动化的  
构建结构化且定义明确的 CI/CD 流程是成功发布产品的关键  
使用GitHub Actions 以实施持续集成阶段和 ArgoCD 以执行持续交付阶段  
此外，使用 模板配置管理器 Helm,使得 YAML 清单的参数化以及它们在多个 Kubernetes 集群中的轻松部署。
> ## CI 使用 github actions  

[创建DockerHub 令牌](https://www.docker.com/blog/docker-hub-new-personal-access-tokens/)  
[创建GitHub 密钥](https://www.docker.com/blog/docker-hub-new-personal-access-tokens/) 设置用于action的存储库变量,环境变量  
[Build and Push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images) marketplace中的上游操作  
创建`/.github/workflows/foo.yaml`


> 练习 创建github action 可以构建推送app的dockerImage  
满足以下要求  
- Image name: python-helloworld
- Tag: latest
- Platforms: platforms: linux/amd64,linux/arm64


 > ## CD 使用 ArgoCD 部署应用  
ArgoCD 是一个 Kubernetes 原生工具，能够自动化应用程序 YAML 清单的生命周期管理。使用 GitOps 模型，配置更改可以通过最少的手动干预进行协调，从而实现高效的部署机制。


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
访问AragoCD应用  
```bash
# 获取命名空间下所有服务 找到端口 进而从vritualBox暴露给主机
kubectl get svc -n argocd
# 复制清单更改为节点端口 用nodeport公开argoCD服务器
kubectl get svc -n argocd argocd-server -o yaml > argocd-nodeport.yaml  
```  
修改配置`argocd-nodeport.yaml` 参考[kubernetes文档](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport)   
最终文件 [argocd-nodeport.yaml](/argocd/argocd-server-nodeport.yaml)  

应用配置： `kubectl apply -f argocd-nodeport.yaml `  
检查 `kubectl get svc -n argocd argocd-server`

[登录](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)  
> vitrualBox有一个静态IP(vagratefile中有配置)加端口号
[需要applicationCRD for argo](/argocd/argocd-python.yaml)  
这个文件指定了目标集群和清单来源  
`kubectl apply -f argocd-python.yaml`  
`kubectl get app -n argocd `

![](/image/%E6%88%AA%E5%9B%BE%202023-05-06%2012-31-40.png)

![](/image/%E6%88%AA%E5%9B%BE%202023-05-06%2013-03-40.png)


# helm 模板配置管理器  
> CI/CD 管道对于自动化和标准化应用程序发布过程至关重要。新的更改通过多个环境传播，包括生产集群，并确保消费者可以使用最新的功能。在理想情况下，所有集群的配置都相似，这样工程团队就可以检查生产部署的真实模拟。这意味着每个集群、沙箱、暂存和生产都需要一组几乎相似的清单。为了减少监督每个集群的类似套件配置的管理开销，模板化是必要的。
![](/image/image3.png)解决这个问题  
`kubectl apply -f argocd-helm-python-prod.yaml`
`kubectl apply -f argocd-helm-python-prod.yaml`