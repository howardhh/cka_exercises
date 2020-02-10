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

