K8S_Certification Labs/a.core_concepts.md
Core Concepts (13%)
kubernetes.io > Documentation > Reference > kubectl CLI > kubectl Cheat Sheet

kubernetes.io > Documentation > Tasks > Monitoring, Logging, and Debugging > Get a Shell to a Running Container

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > Configure Access to Multiple Clusters

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > Accessing Clusters using API

kubernetes.io > Documentation > Tasks > Access Applications in a Cluster > Use Port Forwarding to Access Applications in a Cluster

Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace
show
Create the pod that was just described using YAML
show
Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output
show
Create a busybox pod (using YAML) that runs the command "env". Run it and see the output
show
Get the YAML for a new namespace called 'myns' without creating it
show
Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it
show
Get pods on all namespaces
show
Create a pod with image nginx called nginx and expose traffic on port 80
show
Change pod's image to nginx:1.7.1. Observe that the container will be restarted as soon as the image gets pulled
show
Get nginx pod's ip created in previous step, use a temp busybox image to wget its '/'
show
Get pod's YAML
show
Get information about the pod, including details about potential issues (e.g. pod hasn't started)
show
Get pod logs
show
If pod crashed and restarted, get logs about the previous instance
show
Execute a simple shell on the nginx pod
show
Create a busybox pod that echoes 'hello world' and then exits
show
Do the same, but have the pod deleted automatically when it's completed
show
Create an nginx pod and set an env value as 'var1=val1'. Check the env value existence within the pod
show
