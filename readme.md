---
title: k8s三小时实验之一k8s常用命令
date: 2022-03-04 02:15:50
permalink: /pages/069e55/
categories:
  - k8s
  - k8s有价值课程
tags:
  - 
---

* git路径 : https://github.com/hfbhfb/test-k8s
  * git clone git@github.com:hfbhfb/test-k8s.git
  * 本地路径 : cd /Users/hfb/projects/linux/test-k8s
  * k8s master服务器路径 : cd /home/hfb/projects/linux/test-k8s

* 基础性命令:
  * 所有命名空间的pod
    * kubectl get pods --all-namespaces -o wide
  * 列举所有pod
    * kubectl get pods -o wide

# 部署应用
kubectl apply -f app.yaml
# 查看 deployment
kubectl get deployment
# 查看 pod
kubectl get pod -o wide
# 查看 pod 详情
kubectl describe pod pod-name
# 查看 log
kubectl logs pod-name
# 查看 log 持续
kubectl logs pod-name -f
# 进入 Pod 容器终端， -c container-name 可以指定进入哪个容器。
kubectl exec -it pod-name -- bash
# 伸缩扩展副本
kubectl scale deployment test-k8s --replicas=5
# 把集群内端口映射到节点
kubectl port-forward pod-name 8090:8080
# 查看历史
kubectl rollout history deployment test-k8s
# 回到上个版本
kubectl rollout undo deployment test-k8s
# 回到指定版本
kubectl rollout undo deployment test-k8s --to-revision=2
# 删除部署
kubectl delete deployment test-k8s
# 删除pod
kubectl delete pod testapp
# 获取deployment的配置文件
kubectl get deployment test-k8s -o yaml > 1.yaml
# 删除service
kubectl delete service test-k8s

# 查看全部
kubectl get all
# 重新部署
kubectl rollout restart deployment test-k8s
# 命令修改镜像，--record 表示把这个命令记录到操作历史中
kubectl set image deployment test-k8s test-k8s=ccr.ccs.tencentyun.com/k8s-tutorial/test-k8s:v2-with-error --record
# 暂停运行，暂停后，对 deployment 的修改不会立刻生效，恢复后才应用设置
kubectl rollout pause deployment test-k8s
# 恢复
kubectl rollout resume deployment test-k8s
# 输出到文件
kubectl get deployment test-k8s -o yaml >> app2.yaml
# 删除全部资源
kubectl delete all --all


* 示例1 按不同方式布署pod : cd /home/hfb/projects/linux/test-k8s
  * 命令行方式
    * kubectl run testapp --image=ccr.ccs.tencentyun.com/k8s-tutorial/test-k8s:v1
  * pod方式 
    * kubectl apply -f yaml/deployment/pod.yaml
  * service方式
    * kubectl apply -f yaml/deployment/app.yaml

* 调试示例1 : 
  * 监控打印 :
    * kubectl logs deployment/test-k8s -f
    * kubectl logs pod/test-k8s-xxx-xxx -f
  * 端口映射(master) :
    * kubectl port-forward deployment/test-k8s 8091:8080 --address='0.0.0.0'
    * kubectl port-forward pod/test-k8s-xxxxx-xxx 8091:8080 --address='0.0.0.0'
 
* 验证代码版本回滚
  * kubectl rollout history deployment test-k8s
  * kubectl apply -f yaml/deployment/app_for_recover.yaml
  * 回滚上一版本
    * kubectl rollout undo deployment test-k8s



* [k8s操作实践3小时文档路径 bilibili : 3小时 k8s](https://k8s.easydoc.net/docs/dRiQjyTY/28366845/6GiNOzyZ/puf7fjYr)




