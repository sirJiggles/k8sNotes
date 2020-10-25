# Deployments

How do we deploy apps to a cluster?

Deployments are a type of object that allow you to organize and maintain your pods. They provide us with features that allow us automation. They allow you to just say "I want it to look like this" and k8s will just maintain how you want it to look.

Scaling:
For example with scaling. You can say I want `3 replicas` k8s will just auto create 3 pods, it will also make sure they evenly spread out and a ton of other cool stuff.

Rolling updates
is where they are also useful, you can for example when there is a new version of your app it will gradually replace the pods. This would mean 0 down time

Self healing:
If a pod is removed, the deployment will auto create the one that was removed by mistake / died as this is the "desired state"

## Lets do some things

Here is a simple deployment, named nginx, we specify 2 replicas and in the template and spec we talk about the containers in this deployment.

```
cat <<EOF | kubectl create -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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

Now we can get a list of the deployments just like with the pods by doing this:

`kubectl get deployments`

And we can describe it:
`kubectl describe deployment nginx-deployment`

Here again if there is anything wrong you can have a look at things like events to see what is going wrong.

If you like you can delete one of the pods with something like

`kubectl delete pod POD_NAME` and run `kubectl get pods` you should see by the age that a new one was just made to heal the fact that you removed one like this:

```
nginx-deployment-9b644dcd5-b2z9k   1/1     Running   0          6s
nginx-deployment-9b644dcd5-zj2kb   1/1     Running   0          5m17s
```
