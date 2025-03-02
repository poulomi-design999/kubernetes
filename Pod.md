PODS:
=========


A pod is the basic building block of Kubernetes, and it can be described as the basic unit of deployment. 
Just like we define a process as a program in execution, we can define a pod as a running process in the Kubernetes world. 
Pods are the smallest unit of replication in Kubernetes. A pod can have any number of containers running in it. 
A pod is basically a wrapper around containers running on a node. 

Using pods instead of individual containers has a few benefits. For example, containers in a pod have shared volumes, 
Linux namespaces, and cgroups. 

Each pod has a unique IP address and the port space is shared by all the containers in that pod. 
This means that different containers inside a pod can communicate with each other using their corresponding ports on 
localhost.


Ideally, we should use multiple containers in a pod only when we want them to be managed and located together in the 
Kubernetes cluster. For example, we may have a container running our application and another container that fetches 
logs from the application container and forwards them to some central storage. In this case, we would want both of our 
containers to stay together, to share the same IP so that they can communicate over localhost, and to share the same 
storage so that the second container can read the logs our application container is generating.



Pod Configuration
====================

General Structure of POD.yaml

apiVersion: v1
kind: Pod
metadata:
  name: pod-name
spec:
  containers:
  - name: container1-name
    image: container1-image
  - name: container2-name
    image: container2-image


apiVersion: Version of the Kubernetes API we are going to use.
kind: The kind of Kubernetes object we are trying to create, which is a Pod in this case.
metadata: Metadata or information that uniquely identifies the object we're creating.
spec: Specification of our pod, such as container name, image name, volumes, and resource requests.

Note:
apiVersion, kind, and metadata apply to all types of Kubernetes objects and are required fields. spec is also a required field; however, its layout is different for different types of objects.


Creating a Pod with a Single Container:
-------------

vi single-container-pod.yaml 

apiVersion: v1
kind: Pod
metadata:
  name: first-pod
spec:
  containers:
  - name: my-first-container
    image: nginx


kubectl create -f single-container-pod.yaml
kubectl get pods
kubectl describe pod first-pod



Creating a Pod in a Different Namespace by Specifying the Namespace in the CLI:
------------------
kubectl get namespaces
kubectl --namespace kube-public create -f single-container-pod.yaml
kubectl --namespace kube-public get pods


Creating a Pod in a Different Namespace by Specifying the Namespace in the Pod Configuration YAML file:
----------
kubectl get namespaces

vi single-container-pod-with-namespace.yaml

apiVersion: v1
kind: Pod
metadata:
  name: first-pod-with-namespace
  namespace: kube-public
spec:
  containers:
  - name: my-first-container
    image: nginx


kubectl create -f single-container-pod-with-namespace.yaml
kubectl --namespace kube-public get pods


Changing the Namespace for All Subsequent kubectl Commands:
======--------
kubectl get namespaces
kubectl config set-context $(kubectl config current-context) --namespace kube-public


Creating a Pod Running a Container That Exposes a Port:
-------------
pod-with-exposed-port.yaml


apiVersion: v1
kind: Pod
metadata:
  name: port-exposed-pod
spec:
  containers:
    - name: container-with-exposed-port
      image: nginx
      ports:
        - containerPort: 80

kubectl create -f pod-with-exposed-port.yaml
kubectl port-forward pod/port-exposed-pod 80
curl 127.0.0.1
kubectl logs port-exposed-pod


Creating a Pod with Multiple Containers Running inside It:
=============
vi multiple-container-pod.yaml 

apiVersion: v1

kind: Pod

metadata:

  name: multi-container-pod

spec:

  containers:

    - name: first-container

      image: nginx

    - name: second-container

      image: ubuntu

      command:

        - /bin/bash

        - -ec

        - while :; do echo '.'; sleep 5; done

kubectl create -f multiple-container-pod.yaml

kubectl describe pod multi-container-pod

kubectl logs <pod-name> <container-name>
kubectl logs multi-container-pod second-container -f



Life Cycle of a Pod

kubectl get pod

Pending: This means that the pod has been submitted to the cluster, but the controller hasn't created all its containers yet.
Running: This state means that the pod has been assigned to one of the cluster nodes and at least one of the containers is either running or is in the process of starting up.
Succeeded: This state means that the pod has run, and all of its containers have been terminated with success.
Failed:This state means the pod has run and at least one of the containers has terminated with a non-zero exit code,
Unknown: 



How to Communicate with Kubernetes (API Server)

Here we are going to discuss we will build a foundational understanding of the Kubernetes API server and the various 
ways of interacting with it.


We will learn how kubectl and other HTTP clients communicate with the Kubernetes API server





The Kubernetes API Server:

- In Kubernetes, all communications and operations between the control plane components and external clients, such as 
kubectl, are translated into RESTful API calls that are handled by the API server.

- 


































Deployment
==================

apiVersion: apps/v1

kind: Deployment

metadata:

  name: kubeserve

  labels:

    app: kubeserve

spec:

  replicas : 3

  selector:

    matchLabels:

      app: kubeserve

  template:

    metadata:

      labels:

        app: kubeserve

    spec:

      containers:

      - name: nginx

        image: nginx

        ports:

        - containerPort: 80


kubectl apply -f sample-deployment.yaml
kubectl get deployments
kubectl describe deploy kubeserve
