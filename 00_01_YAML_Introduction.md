Tutorial to help you understand YAML in the context of Kubernetes.
What is YAML?
YAML (YAML Ain't Markup Language) is a human-readable data format used for configuration files.
Kubernetes uses YAML to define resources like Pods, Deployments, and Services.
It is indentation-sensitive (uses spaces, not tabs).
Why YAML for Kubernetes?
Declarative: Describes the desired state of your Kubernetes resources.
Readable and Simple: Easy to understand and manage complex configurations.
Consistent: Kubernetes applies the same YAML across different environments.
Basic YAML Structure
key: value
Key-Value Pairs: Represent configurations or data.
Lists: Use - to represent arrays.
Nested Structures: Indentation defines hierarchy.
Kubernetes Pod Example (Basic YAML):
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
Understanding the Pod YAML File:
apiVersion – Defines the Kubernetes API version to use.
v1 is the version for basic resources (Pods, ConfigMaps).
kind – Describes the type of Kubernetes resource (e.g., Pod, Deployment).
metadata – Provides information about the resource.
name: Unique identifier for the Pod.
labels: Key-value pairs to tag resources.
spec – Describes the desired state of the resource.
containers – List of containers inside the Pod.
name: Name of the container.
image: Docker image to pull (e.g., nginx:latest).
ports: Exposes port 80 for HTTP traffic inside the container.
Another Example – Deployment (More Complex):
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
Key Points to Remember:
Indentation Matters: Use two spaces for each level.
List Items: Use - for lists (like containers and ports).
Consistency: Avoid mixing tabs and spaces.
Order Doesn’t Always Matter: But correct nesting is crucial.
Deploying YAML in Kubernetes:
Save the YAML file:
vim nginx-pod.yaml
Apply the configuration:
kubectl apply -f nginx-pod.yaml
Check Pod status:
kubectl get pods
Validate YAML (Optional):
kubectl apply --dry-run=client -f nginx-pod.yaml
Validates without deploying.
Why YAML is Critical in Kubernetes:
Declarative Approach – You define the desired state, and Kubernetes reconciles the current state to match.
Reusable – YAML files can be stored in Git for versioning and CI/CD pipelines.
Automation – Easily automate resource creation and updates.
