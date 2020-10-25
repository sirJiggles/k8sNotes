# Wha

k8s is lots of components that work together to provide the functionality of a cluster, for example:

`kubectl get pods -n kube-system` from the master node will show something like:

```
NAME                                                READY   STATUS    RESTARTS   AGE
coredns-5d4dd4b4db-82skg                            1/1     Running   0          61m
coredns-5d4dd4b4db-krb5w                            1/1     Running   0          61m
etcd.mylabserver.com                      1/1     Running   0          60m
kube-apiserver.mylabserver.com            1/1     Running   0          60m
kube-controller-manager.mylabserver.com   1/1     Running   0          60m
kube-flannel-ds-amd64-8mws8                         1/1     Running   0          54m
kube-flannel-ds-amd64-q2wwz                         1/1     Running   0          54m
kube-flannel-ds-amd64-s489m                         1/1     Running   0          54m
kube-proxy-f6gzr                                    1/1     Running   0          58m
kube-proxy-qn4w8                                    1/1     Running   0          61m
kube-proxy-wzzr2                                    1/1     Running   0          58m
kube-scheduler.mylabserver.com            1/1     Running   0          60m
```

## Some of these components are pods themselves

As many of the components are pods themselves we can have a look at them like this. Lets talk about a few:

### etcd

Provide sync data store for the cluster state:

- for controllers or master server
- what pods are running
- what nodes are running
- all the different data objects in the cluster

### kube-apiserver

serves the k8s API, this is just a web based rest api. when you run commands on the cli, kubectl is talking to the api server to make changes to the cluster

### kube-controller-manager

This is what does the work, this is the back end for the API. so when you hit the API, the API hits this guy to actually do things on the cluster

### kube-scheduler

determines when to run pods and what nodes they need to run on.

### The 4 above together

These together form the k8s control plane, these are the things you need to control the cluster and make up and form the k8s master

### kube-proxy

handle the network communication between the pods, so if pods are on different nodes and need to talk to each-other. for the pod this is just a virtual network. But `kube-proxy` is one of the tools that helps put this together by making firewall rules on each node to allow the traffic to be routed correctly.

## Some important components that are not pods

### kublet

The are additional components that are not part of the control plane, but they are part of the nodes. the most important is `kublet`, this is the agent that runs on each node. acts as a middleman between the k8s API and the container run time (docker for us).

when a container needs to be run on a node the control plane reaches out to the kublet on the node, kublet then tells docker to run the container.

This is not running a pod on the cluster as this is the thing that RUNS the pods. It cannot be a thing that controls itself. So it is actually a service. You can see it like this

`sudo systemctl status kubelet`
