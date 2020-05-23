# Kubernetes 部署 Wordpress

### 怎么使用？

```
  git clone https://github.com/usoftglobal/k8s-wordpress.git

  cd k8s-wordpress

  # 创建一个本地数据卷
  kubectl apply -f local-volumes.yaml

  # 查看 PV，正常情况下状态为 Available
  kubectl get pv

  # 部署 Wordpress，pod 状态为 running 有一个过程稍微等一会
  kubectl apply -k ./deployments

  # 查看 pod 节点是否正常运行，正常情况 wordpress 开头的 pod 状态为 Running
  kubectl get pods

  # 查看 pod 详细信息，如果长时间没有 Running 状态则可能出问题了，需要查看 Events 日志排查
  kubectl describe pod {上一步的podname}

  # 查看 NodePort 类型的端口，结合 Master 的主机 IP 就可以远程访问了
  kubectl get services
```

### 怎么清除（重装）？

```
  # 清除 Wordpress
  kubectl delete -k ./deployments

  # 清除本地数据卷
  kubectl delete -f local-volumes.yaml
```

### 常见问题 Q&A

Q：查看 pod 详情里面提示有 `node.kubernetes.io/not-ready` 相关污点

A：这表示节点还没准备好，通过 `kubectl get nodes` 如果发现 STATUS 不是 Ready 有可能缺少网络插件，可以使用 `kubectl apply -f https://raw.githubusercontent.com/weaveworks/weave/master/prog/weave-kube/weave-daemonset-k8s-1.9.yaml` 命令在 Master 节点安装

Q：查看 pod 详情里面提示有 `node-role.kubernetes.io/master` 相关污点

A：Master 主机默认不允许部署 pod，可以通过 `kubectl taint nodes --all node-role.kubernetes.io/master-` 去除

### 参考文档

https://kubernetes.io/zh/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/
