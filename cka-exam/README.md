**Q1** - Create a pod and allow it to be able to set `system_time`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo-pod
spec:
  containers:
  - name: sec-ctx-pod
    image: gcr.io/google-samples/node-hello:1.0
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
```

Run following commands to get the pod status and details:

```Shell
kubectl get pod
```

```Shell
kubectl describe security-context-demo-pod
```

Reference link: [Set capabilities for a Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-capabilities-for-a-container)


**Q4** - Create a new deployment, scale this deployment to 3 replicas.

```Shell
kubectl create deployment my-deployment --image nginx
```

```Shell
kubectl scale deployment my-deployment --replicas=3
```
Run following commands to get the pod status:

```Shell
k get deployments.apps my-deployment
```

**Q6** - Kubernetes: deploy a web pod using nginx & set label to tier=web

```Shell
k run my-pod --image nginx -l tier=web
```
Run following commands to get the pod details with labels:

```Shell
kubectl describe my-pod
```

**Q7** - Create static pod on worker node node01.

SSH to `node01` and here, we are going to identify the path configured for static pods in this node:

```Shell
ssh node01
```

Check the kubelet config file:

```Shell
ps -aux | grep kubelet
```

Output will show this:

`--config=/var/lib/kubelet/config.yaml`.

Now, we will see the value assigned for `staticPodPath`:

```Shell
vi /var/lib/kubelet/config.yaml
```
Check the yaml file and you will find: `staticPodPath: /etc/kubernetes/manifests`

Create a pod definition file in the manifests folder. To do this, run the command:  

```Shell
kubectl run --restart=Never --image=nginx static-nginx --dry-run=client -o yaml > /etc/kubernetes/manifests/static-nginx.yaml
```

We can check on which node the pod is running by following command:

```Shell
kubectl get pods --all-namespaces -o wide  | grep static-nginx
```

**Q8** - Create a pod with multiple containers

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mc1
  labels:
    app: mc1
spec:
  containers:
  - name: redis
    image: redis
  - name: nginx
    image: nginx
```

Run following commands to get the pod details with multiple containers:

```Shell
kubectl get pod
```

```Shell
kubectl describe pod mc1
```

**Q9** - Create a pod in a namespace with the defined environment.


