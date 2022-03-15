# K8s Cluster Setup

## Architecture
- 1 Master node 
- 2 Worker nodes

### Master Node Specs
- 4 CPUs

- 4 GB RAM

- 20 GB Storage

### Worker Nodes Specs
- 8 CPUs

- 8 GB RAM

- 20 GB storage

### Master Node IPv4 config

Subnet `172.16.10.0/24`

Address `172.16.10.211`

Gateway `172.16.10.1`

Name servers `8.8.8.8`

### Worker Node 1 IPv4 config

Subnet `172.16.10.0/24`

Address `172.16.10.212`

Gateway `172.16.10.1`

Name servers `8.8.8.8`

### Worker Node 2 IPv4 config

Subnet `172.16.10.0/24`

Address `172.16.10.213`

Gateway `172.16.10.1`

Name servers `8.8.8.8`

## Cluster Preparation

Perform steps on each node (master + workers)

### 1. Install Docker 

[Docker Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04) 

`sudo nano /etc/docker/daemon.json`

add to file:

```json
{
    "exec-opts": ["native.cgroupdriver=systemd"]
}
```

`sudo systemctl daemon-reload`

`sudo systemctl restart docker`

### 2. Disable swap

`sudo swapoff -a`

`sudo nano /etc/fstab`

comment out `/swap.img none swap sw 0 0`

`#/swap.img none swap sw 0 0`

### 3. Install Kubeadm Kubectl Kubelet

`sudo apt-get update`

`sudo apt-get install -y apt-transport-https ca-certificates curl`


`sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg`

`echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list`

`sudo apt-get update`

`sudo apt-get install -y kubelet kubeadm kubectl`


## Initialize Cluster (Master Node)

`sudo kubeadm init`

`mkdir -p $HOME/.kube`

`sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`

`sudo chown $(id -u):$(id -g) $HOME/.kube/config`

Install weavenet

`kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"`

## Add Worker Nodes to Cluster

Run on master node:

`kubeadm token create --print-join-command`

Copy & Paste output on worker node