# Kubernetes 部署 Wordpress

### 怎么使用？

```
  git clone https://github.com/usoftglobal/k8s-wordpress.git

  cd k8s-wordpress
```

### 创建一个本地数据卷

```
  kubectl apply -f local-volumes.yaml

  # 查看 PV，正常情况下状态为 Available
  kubectl get pv

  # 部署 Wordpress
  kubectl apply -k ./deployments
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
