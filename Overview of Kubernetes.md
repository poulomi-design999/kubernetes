In this chaptersection, we will have our first hands-on introduction to Kubernetes with some fundamental 
Kubernetes components.

Setting up Kubernetes
========================

1: An Overview of Minikube
===========================


MASTER Node: 10.0.0.4  40.81.243.184
WorkerNode: 10.0.0.5   40.81.245.7


Step1: Operating System Changes/Configuration updates:
This is 2 node Kubernetes Cluster and below specifications: 
MasterNode 10.0.0.4 Linux (ubuntu 18.04) 
WorkerNode1 10.0.0.5 Linux (ubuntu 18.04)



sudo apt-get update && sudo apt-get install -y apt-transport-https curl
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

kubeadm init --apiserver-advertise-address=10.0.0.4 --pod-network-cidr=192.168.0.0/16


$ apt update 
$ apt install -y curl gnupg2 
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - 
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
$ apt update 
$ apt install -y containerd.io docker-ce



Configure the Docker daemon, in particular to use systemd for the management of the container’s cgroups.
$ mkdir /etc/docker cat <<EOF | sudo tee /etc/docker/daemon.json { "exec-opts": ["native.cgroupdriver=systemd"], "log-driver": "json-file", "log-opts": { "max-size": "100m" }, "storage-driver": "overlay2" } EOF

Restart Docker and enable on boot:
$ systemctl enable docker 
$ systemctl daemon-reload 
$ systemctl restart docker
$ systemctl status docker
