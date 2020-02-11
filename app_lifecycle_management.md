### How to perform rolling update and rollbacks on deployments/daemonsets.
1. Create the DaemonSet with nginx:1.10.1 image.
2. Update the DaemonSet above with newer version of the nginx:1.12.1-alpine server.
3. Change the updateStrategy to OnDelete and rollback to revision 1.

### Various ways to configure applications.
1. Create a pod (name: ubuntu-sleeper) with the ubuntu:18.04 image to run a container to sleep for 5000 seconds.
2. Create a pod with the given specifications.
	1. Pod name: print-greeting, image: ubuntu.
	2. 3 env variables GREETING, HONORIFIC, and NAME are set to Warm greetings to, The Most Honorable, and Kubernetes.
	3. run in the bash shell the command: echo $(GREETING) $(HONORIFIC) $(NAME), then sleep 5000.
3. Set env variables with ConfigMap.
    1. create a configmap name family with father=lkd, mother=tn, son=bob.
	2. create a pod name shell-demo, image: nginx, env name (iam) from the key father of configmap family.
	3. use envFrom to define all of the ConfigMap’s data as Pod environment variables.