# Clustering and nodes

k8s implements a 'clustered' architecture, this means we have multiple servers that can run our workloads or containers.

The servers that run the containers are called nodes. There are two types of servers.

## control servers

Manage and control the cluster and host the k8s API. can have more than one of these for high availability. They are normally separate from the worker nodes

## worker nodes

Responsible for running the app workload. They are the containers that

## Lets have a look at the nodes

`kubectl get nodes` on the master will show the nodes
`kubectl describe node NODE_NAME` this will give us additional information about this node. Allocated resources and events are worth noting, as you keep an eye on when you might need to give more resources and events help for when things are wrong and you might need to debug.
