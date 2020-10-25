# Robot shop app

Remove the old service `kubectl delete svc nginx-service`

Clone the repo:

```
cd ~/
git clone https://github.com/linuxacademy/robot-shop.git
```

Have a look in the folder `cd ~/robot-shop/K8s/descriptors/` you will see a bunch of config files. These are descriptors for all the objects needed in our cluster.

Create a namespace and deploy the application objects to the namespace using the deployment descriptors from the Git repository:

```
kubectl create namespace robot-shop
kubectl -n robot-shop create -f ~/robot-shop/K8s/descriptors/
```

Now lets have a look at our pods with `kubectl get pods -n robot-shop -w` notice you need to supply the namespace now. if you just do kubectl get pods, nothing will show up. This is because we previously added a little namespace.

You should now be able to have a look at the site running when the app is up at `http://YOUR_MASTER_PUBLIC_IP:30080`. Have a look through the repo code. There are a ton of services in this application various databases and so on. And now you can just manage it all through the yaml files. noice and easy peasy!
