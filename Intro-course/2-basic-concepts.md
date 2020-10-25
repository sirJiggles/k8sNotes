# Containers and Pods

## Pod

- smallest and most basic building block of the model
- normally one container per pod but can have more
- has its own storage resources and its own IP address
- you are normally always doing something that is working with pods in k8s

## Containers

- containers running is called "scheduling", k8s schedules a pod to run on a node, that pod runs the containers on that pod
- they are just containers

## Make a pod

Lets pipe some config to the command to make a pod using `cat EOF` and pipe it to the command. This will be a simple nginx server.

```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```

Run `kubectl get pods` to see that bab boi we just made. Now run `kubectl get pods -n kube-system` you will see all the 'back end' pods. These are 'system pods' by the -n (namespace)

So our nginx pod we just made is in the default namespace which is why we do not need to supply -n to see it.

### More info on a pod

`kubectl describe pod nginx`. What is super handy in here is the event list, in here you can see what is happening in the pod. so if there are errors etc you might be able to find them in there.

### Remove the pod

To remove the pod we just made, lets remove with `kubectl delete pod nginx`

### A note on kubectl X commands

It is work noting `kubectl get, delete and describe` do not just work on pods but on many other resources too.
