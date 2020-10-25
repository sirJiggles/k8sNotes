# Networking in k8s

You should have a basic high level understanding of what is going on. we used flannel, but lets talk about what it does.

Creates a 'virtual network' that spans all the nodes in the cluster. Like a fake box that covers all the machines that are physically separate. So using this network we do not need to know or even care if the pod is on another node. We just need to know the IP

There are tons of plugins as you need to sometimes have different types of networking solutions depending on your infra.

## Lets try it out.

Here is a deployment with two ngix pods:

```
cat << EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.4
        ports:
        - containerPort: 80
EOF
```

Create a 'busybox' pod for testing, with this we can curl:

```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: busybox
    image: radial/busyboxplus:curl
    args:
    - sleep
    - "1000"
EOF
```

Get the IP address of the pods, -o is the output flag so we can see wht node they are no and the IP addr
`kubectl get pods -o wide`

Inside the busybox pod run a command to get the IP of another pod

Get the IP address of one of the nginx pods, then contact that nginx pod from the busybox pod using the nginx pod's IP address:
`kubectl exec busybox -- curl $nginx_pod_ip`

If you did this to one of the nginx pods you should get the output of the html document 🎉
