# Services

These allow us to dynamically access a group of replica pods. For example the pods we made in 7 for nginx could always be changing.

- if a pod is removed
- if we scale up or down

So as the list changes, we could have something internal or external to the cluster that needs to access those pods the question is "how do I access my nginx webserver, when the pods are changing" hello services.

Services give us load balance usage of the dynamic list of pods.

If you make a request to a service you will get a response from one of the pods that are part of the service.

Is an abstraction layer built on top of a set of replica pods. you don't talk direct to the pods, just through the service.

## How does it look

Lets make a node port service for those two pods

```
cat << EOF | kubectl create -f -
kind: Service
apiVersion: v1
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
  type: NodePort
EOF
```

the selector is super important, in our case above we are going to select pods that have a label of app with a value of nginx. Then we just specify the ports where we are going to send the traffic.

Here we are also making a 'NodePort' type service, there are many types of services. but for now, node port will allow us to expose the serves externally. for us it is ok for now for testing.

We can now list the services with `kubectl get svc` which should give us something like this:

```
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        114m
nginx-service   NodePort    10.111.40.176   <none>        80:30080/TCP   4s
```

Now if we make a request from outside to our server we should get a response, we can just test it like so:

`curl localhost:30080`

As this is on our server we can just test it like so on the server. you should get the nginx response:

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

## Internal services

You can also have internal services, like the k8s one in the previous list of running services. Any pod in the cluster can use this to make requests to other pods. This is really cool if you have micro-services.
