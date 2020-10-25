# Setting up k8s

## Some things we will be using:

### Kubeadm

tool that simplifies setting up the cluster

### Kublet

agent that manages process of running containers on each node

### kubectl
command line tool that we use to interact with the cluster

## Control plane

services that form the master, allows the master to control the cluster

## Making a new cluster

### container run time

does not come with kubernetes, we can use things like docker, rkt containerD but for now we will use docker.

To install docker:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install -y docker-ce=18.06.1~ce~3-0~ubuntu

# lock dow the version here as kubeadm might be behind the latest docker
sudo apt-mark hold docker-ce
```

### Install the three packages

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update

sudo apt-get install -y kubelet=1.15.7-00 kubeadm=1.15.7-00 kubectl=1.15.7-00

sudo apt-mark hold kubelet kubeadm kubectl
```

### On the master

`sudo kubeadm init --pod-network-cidr=10.244.0.0/16`

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

`kubectl version`

Should see the server version, this means the server is running on the master and things are looking good. Now we can get the worker nodes on the cluster

Command will look something like this:
`kubeadm join 172.31.106.253:6443 --token ye1s23.mx3scnzljy5hqgxj --discovery-token-ca-cert-hash sha256:c619ead5a9f66f9c1466f221999228f140d952352006416156e0eb7238ab5925`

Then on the server have a look for the nodes in the cluster like so:

### Networking (using flannel)

on k8’s there are many networking solutions and plugins that can set it up in different ways, for now we will use flannel to set it up like so

on all three nodes run:

`echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf`
`sudo sysctl -p`

Install flannel on the master

Apply some king of config to the master, we are going to pass in the config from a file. and download the file from the internet. This is provided by core os who are maintainers of flannel

`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml`

Now we should be able to get the nodes with

`kubectl get nodes`

They should all have the ready state now!.

Make sure flannel pods are working on the master by running

`kubectl get pods -n kube-system`

The -n and kube-system will show us some of the system pods and not just ‘our’ ones.
