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

### Various ways to configure applications.
1. Create a pod (name: ubuntu-sleeper) with the ubuntu:18.04 image to run a container to sleep for 5000 seconds.
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper
spec:
  containers:
  - image: ubuntu:18.04
    command:
    - "sleep"
    - "5000"
```

2. Create a pod with the given specifications.
	1. Pod name: print-greeting, image: ubuntu.
	2. 3 env variables GREETING, HONORIFIC, and NAME are set to Warm greetings to, The Most Honorable, and Kubernetes.
	3. run in the bash shell the command: echo $(GREETING) $(HONORIFIC) $(NAME), then sleep 5000.
```
apiVersion: v1
kind: Pod
metadata:
  name: print-greeting
spec:
  containers:
  - name: print-greeting
    image: 11.8.84.11:5000/ubuntu:18.04
    env:
    - name: GREETING
      value: Warm greetings to
    - name: HONORIFIC
      value: The Most Honorable
    - name: NAME
      value: Kubernetes
    command:
    - "/bin/bash"
    args: ["-c","echo $(GREETING) $(HONORIFIC) $(NAME) && sleep 5000"]
```

3. Set env variables with ConfigMap.
   1. create a configmap name family with father=lkd, mother=tn, son=bob.
   2. create a pod name shell-demo, image: nginx, env name (iam) from the key father of configmap family.
   3. use envFrom to define all of the ConfigMap’s data as Pod environment variables.
```
kubectl create configmap family --from-literal=father=lkd --from-literal=mother=tn --from-literal=son=bob
```

```
apiVersion: v1
kind: Pod
metadata:
  name: shell-demo
spec:
  containers:
  - name: nginx
    image: nginx
    env:
    - name: iam
      valueFrom:
        configMapKeyRef:
          key: father
          name: family
```

```
...
    envFrom:
    - configMapRef:
        name: family
...
```
