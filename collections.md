1. 列出环境内所有的pv 并以 name字段排序（使用kubectl自带排序功能）
```
kubectl get pv -A --sort-by='{.metadata.name}'
```
2. 列出指定pod的日志中状态为Error的行，并记录在指定的文件上
```
kubectl logs <pod_name> | grep Error >> output
```
3. 列出k8s可用的节点，不包含不可调度的 和 NoReachable的节点，并把数字写入到文件里
4. 创建一个pod名称为nginx，并将其调度到节点为 disk=stat上
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  nodeSelector:
    disk: stat
```
5. 提供一个pod的yaml，要求添加Init Container，Init Container的作用是创建一个空文件，pod的Containers判断文件是否存在，不存在则退出
6. 创建一个pod名称为test，镜像为nginx，Volume名称cache-volume为挂在在/data目录下，且Volume是non-Persistent的