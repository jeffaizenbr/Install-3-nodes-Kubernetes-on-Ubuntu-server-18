# Install-3-nodes-Kubernetes-on-Ubuntu-server-18


##On Master Node:

```bash
sudo hostnamectl set-hostname k8s-master
```
##On Worker Node 01:

```bash
sudo hostnamectl set-hostname k8s-node-01
```
##On Worker Node 02:

```bash
sudo hostnamectl set-hostname k8s-node-02
```
##On all Nodes

```bash
cat /etc/hosts
192.168.2.2 k8s-master 
192.168.2.3 k8s-node-01 
192.168.2.4 k8s-node-02
```

##Setup Kubernetes on Ubuntu 18.04 – Prerequisites (Run on all nodes)
```bash
sudo apt-get update ; sudo apt-get upgrade ; sudo apt-get install linux-image-extra-virtual ; sudo reboot
```
##Add user to manage Kubernetes cluster:


```bash
sudo useradd -s /bin/bash -m k8s-admin ; sudo passwd k8s-admin ;  sudo usermod -aG sudo k8s-admin ; echo "k8s-admin ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/k8s-admin
```
##Setup Kubernetes on Ubuntu 18.04 – Install Docker Engine

```bash
sudo apt-get remove docker docker-engine docker.i
```
##Install dependencies:

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```
##Import Docker repository GPG key:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```bash
sudo add-apt-repository \
 "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
 $(lsb_release -cs) \
 stable"
```
##Install docker:

```bash
sudo apt-get update ; sudo apt-get install docker-ce ; sudo usermod -aG docker k8s-admin
```

##Setup Kubernetes on Ubuntu 18.04 – Install and Configure Kubernetes Master

```bash
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
##Then import GPG key:

```bash
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
##Update apt package index:


```bash
sudo apt update

```
##Install Kubernetes Master Components

```bash
sudo apt install kubectl kubelet kubeadm kubernetes-cni
```
If swap is on, turn it off.

```bash
sudo swapoff -a
```

vim /etc/docker/daemon.json
```bash
{
    "exec-opts": ["native.cgroupdriver=systemd"]
}
```


```bash
export KUBECONFIG=/etc/kubernetes/admin.conf
```
##Initialize Kubernetes Cluster:

```bash
kubeadm init --apiserver-advertise-address 192.168.122.100
```

initialize worker slave
```bash
kubeadm join 192.168.122.100:6443 --token nec1aj.mhh8vdmzqzkmgkrf        --discovery-token-ca-cert-hash sha256:7f3d5d34ed20da0b4ee4ca6146ce1c2ca1bccdc2844d546b2ac9593858edea89 --v=5

```
If needed create a new token

```bash
kubeadm token generate

```

generate key

```bash
kubeadm token create <key last command> --print-join-command --ttl=0

```
