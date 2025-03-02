Kubernetes Installation
=======================

Ref: 
https://kubernetes.io/
https://labs.play-with-k8s.com/
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
https://projectcalico.docs.tigera.io/getting-started/kubernetes/self-managed-onprem/onpremises


AMZON LINUX -----> CentOS based

1: Master Node 172.31.47.223   43.205.110.93
2: Worker node 172.31.47.46    52.66.119.59



This is the installation step using KUBEADM
==============================================


Step# 1: Install docker on BOTH  master and worker node

sudo yum update
sudo yum install docker
sudo systemctl enable docker.service
sudo systemctl start docker.service
sudo systemctl start docker.service
systemctl status docker.service

=======
Step#2: Run on BOTH MASTER and Worker

cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

# Set SELinux in permissive mode (effectively disabling it)

sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet

========

Step#3 : This you have to run on MASTER node ONLY

sudo kubeadm init  --apiserver-advertise-address=172.31.47.223 --pod-network-cidr=192.168.0.0/16 

------------ output ------------
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.47.223:6443 --token c0k62t.xbw215hw9zwq5tt4 --discovery-token-ca-cert-hash sha256:1a980fea2bb2fc6f3894234ca3effbbb8842461111f57afdc6895f0232a17b93

------------ output Ends------------

Step#4: Install Calico on MASTER ONLY

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/tigera-operator.yaml
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/custom-resources.yaml -O
kubectl create -f custom-resources.yaml


-------------



