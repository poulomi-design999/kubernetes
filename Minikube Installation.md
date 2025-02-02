Introduction
Minikube is a lightweight Kubernetes implementation that creates a VM on your local machine and deploys a simple cluster containing only one node. 
Minikube is available for Linux, macOS, and Windows systems. Here, you will install Kubernetes with Minikube on Ubuntu 20.04.

On a variety of platforms, Minikube is a free utility that facilitates the setup of single-node Kubernetes clusters. 
Numerous Kubernetes technologies, including NodePorts, DNS, the Container Network Interface, Ingress, ConfigMaps, Secrets, etc., are supported.

Prerequisites
Ubuntu 20.04 desktop installed on your system
A root password set up on your system

Step 1 – Updating the system:
1) Make sure that the system is running on the latest versions.

$ apt-get update -y
$ apt-get upgrade -y

2) Restart the computer to have the modifications take effect.

3) Then, download a few packages that would be useful.
$ apt-get install curl wget apt-transport-https -y

Step 2 – Install VirtualBox Hypervisor:
Since both KVM and VirtualBox Hypervisor are supported by Minikube,
$ apt-get install virtualbox virtualbox-ext-pack

Step 3 – Download and Install the latest Minikube:
$ wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

Make sure that you have coppied the downloaded file to /usr/local/bin.
$ cp minikube-linux-amd64 /usr/local/bin/minikube

Give execution permission to Minikube.
$ chmod 755 /usr/local/bin/minikube

Check the version of Minikube
$ minikube version


Step 4 – Install Kubectl:

Download the GPG key, then add it.
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

Add Kubernetes apt repository with the following command
$ echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list

Update the repository and install Kubectl
$ apt-get update -y
$ apt-get install kubectl -y

Check the version of Kubectl installed
kubectl version -o json

Step 5 – Start Minikube:
This command will download VirtualBox image will be downloaded, and the Kubernetes cluster will be set up, make sure your
run this command with Normal users


$ minikube start

* minikube v1.29.0 on Ubuntu 20.04
* Automatically selected the virtualbox driver. Other choices: ssh, none
* Downloading VM boot image ...
    > minikube-v1.29.0-amd64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.29.0-amd64.iso:  276.35 MiB / 276.35 MiB  100.00% 115.29 MiB
* Starting control plane node minikube in cluster minikube
* Downloading Kubernetes v1.26.1 preload ...
    > preloaded-images-k8s-v18-v1...:  397.05 MiB / 397.05 MiB  100.00% 78.29 M
* Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ..


Check the status of the cluster
$ kubectl cluster-info

$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.59.100:8443
CoreDNS is running at https://192.168.59.100:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

$ kubectl cluster-info dump
$ kubectl config view

$ kubectl get nodes
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   2m36s   v1.26.1


$ kubectl get pods --all-namespaces
NAMESPACE     NAME                               READY   STATUS    RESTARTS        AGE
kube-system   coredns-787d4945fb-jx97g           1/1     Running   0               2m56s
kube-system   etcd-minikube                      1/1     Running   0               3m7s
kube-system   kube-apiserver-minikube            1/1     Running   0               3m11s
kube-system   kube-controller-manager-minikube   1/1     Running   0               3m7s
kube-system   kube-proxy-9d9kk                   1/1     Running   0               2m56s
kube-system   kube-scheduler-minikube            1/1     Running   0               3m11s
kube-system   storage-provisioner                1/1     Running   1 (2m22s ago)   3m4s
