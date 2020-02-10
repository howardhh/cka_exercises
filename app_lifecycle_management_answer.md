### How to perform rolling update and rollbacks on deployments/daemonsets.
1. Create the DaemonSet with nginx:1.10.1 image.
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-ds
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      name: nginx-ds-pod
      labels:
        app: nginx 
    containers:
    - image: nginx:1.10.1
      imagePullPolicy: IfNotPresent 
```

2. Update the DaemonSet above with newer version of the nginx:1.12.1-alpine server.
```
kubectl set image daemonset nginx-ds *=nginx:1.12.1-alpine
```

3. Change the updateStrategy to OnDelete and rollback to revision 1.
```
...
spec:
  updateStrategy:
    type: OnDelete
...
```

```
kubectl rollout undo daemonset/nginx-ds --to-revision=1.
```