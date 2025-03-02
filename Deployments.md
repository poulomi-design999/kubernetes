# Deployments

In real world thing starts from here, we write deployment to launch Pods. A Deployment provides declarative updates for Pods and ReplicaSets.

- A Deployment is a Kubernetes object that acts as a wrapper around a ReplicaSet and makes it easier to use. 
- In order to manage replicated services, it's recommended that you use Deployments that, in turn, manage the ReplicaSet and the Pods created by the ReplicaSet. 
- The major motivation for using a Deployment is that it maintains a history of revisions. 
- Every time a change is made to the ReplicaSet or the underlying Pods, a new revision of the ReplicaSet is recorded by the Deployment. 
- This way, using a Deployment makes it easy to roll back to a previous state or version. Keep in mind that every rollback will also create a new revision for the Deployment

```yaml
Here's an example of a Deployment configuration:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:

    app: nginx

spec:

  replicas: 3

  strategy:

    type: RollingUpdate

    rollingUpdate:

      maxUnavailable: 1

      maxSurge: 1

  selector:

    matchLabels:

      app: nginx

      environment: production

  template:

    metadata:

      labels:

        app: nginx

        environment: production

    spec:

      containers:

      - name: nginx-container

        image: nginx
 
```



The value for the kind field is Deployment. The rest of the configuration remains the same as that for ReplicaSets. Deployments also have the replicas, selector, and Pod template fields used in the same way as ReplicaSets.

**Strategy**

In the strategy field under spec, we can specify which strategy the Deployment should use when it replaces old pods with new ones. This can either be RollingUpdate or Recreate. The default value is RollingUpdate.

**Rollig Update**:

- This is a strategy used to update a Deployment without having any downtime. 
- With the **RollingUpdate** strategy, the controller updates the Pods one by one. 
- Hence, at any given time, there will always be some Pods running. 
- This strategy is particularly helpful when you want to update the Pod template without incurring any downtime for your application. 
- However, be aware that having a rolling update means that there may be two different versions of Pods (old and new) running at the same time.

```
strategy:

  type: RollingUpdate

  rollingUpdate:

    maxUnavailable: 1

    maxSurge: 1
```

- **maxUnavailable** is the maximum number of Pods that can be unavailable during the update.
- **maxSurge** is the maximum number of Pods that can be scheduled/created above the desired number of Pods (as specified in the **replicas** field).

The two parameters—**maxUnavailable** and **maxSurge**—can be tuned for availability and the speed of scaling up or down the Deployment.

**Recreate**

- In this strategy, all the existing pods are killed before creating the new Pods with an updated configuration.
- This means there will be some downtime during the update. 
- This, however, ensures that all the Pods running in the Deployment will be on the same version (old or new). 

```
strategy:

  type: Recreate
```


UseCase: A good use case for using the **Recreate** update strategy is if we need to run some data migration or data processing before the new code can be used. In this case, we will need to use the **Recreate** strategy because we can't afford to have any new code running along with the old one without running the migration or processing first for all the Pods.

**Examples**



```
Creating Deployment with Nginx COntainers

vi nginx-deployment.yaml

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

      environment: production

  template:

    metadata:

      labels:

        app: nginx

        environment: production

    spec:

      containers:

      - name: nginx-container

        image: nginx

kubectl apply -f nginx-deployment.yaml
kubectl get deployment nginx-deployment
kubectl get pods
kubectl describe rs nginx-deployment
```





```

Updating a Deployment
==========================
vi nginx-deployment2.yaml

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

        image: nginx:1.14.2

        ports:

        - containerPort: 80

kubectl apply -f nginx-deployment2.yaml
kubectl get deployments

Let's update the nginx Pods to use the nginx:1.16.1 image instead of the nginx:1.14.2 image.

kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
or use the following command:
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1

Alternatively, you can edit the Deployment and change .spec.template.spec.containers[0].image from nginx:1.14.2 to nginx:1.16.1:

kubectl edit deployment/nginx-deployment

To see the rollout status, run:

kubectl rollout status deployment/nginx-deployment

After the rollout succeeds, you can view the Deployment by running:
kubectl get deployments

kubectl get rs
kubectl get pods
kubectl describe deployments
```



## Rolling Back a Deployment

- In a real-life scenario, you may make a mistake when making a change in the Deployment configuration. 
- You can easily undo a change and roll back to a previous stable revision of the Deployment.
- use the **kubectl rollout** command to check the revision history and rollback

```
kubectl rollout history deployment <deployment_name>
```

```

vi app-deployment.yaml

apiVersion: apps/v1

kind: Deployment

metadata:

  name: app-deployment

  labels:

    environment: production

spec:

  replicas: 3

  selector:

    matchLabels:

      app: nginx

      environment: production

  template:

    metadata:

      labels:

        app: nginx

        environment: production

    spec:

      containers:

      - name: nginx-container

        image: nginx

kubectl apply -f app-deployment.yaml
kubectl rollout history deployment app-deployment

For the first update, let's change the name of the container to nginx instead of nginx-container. 
Update the content of the app-deployment.yaml file with the following:

apiVersion: apps/v1

kind: Deployment

metadata:

  name: app-deployment

  labels:

    environment: production

spec:

  replicas: 3

  selector:

    matchLabels:

      app: nginx

      environment: production

  template:

    metadata:

      labels:

        app: nginx

        environment: production

    spec:

      containers:

      - name: nginx

        image: nginx

Apply the changed configuration:
kubectl apply -f app-deployment.yaml --record

Wait for a few seconds to allow the Deployment to recreate the Pods with the updated Pod configuration:

kubectl rollout history deployment app-deploymen
```



**Rolling Back to a Previous Revision**

```YAML

Now you've decided to undo the current rollout and rollback to the previous revision:

kubectl rollout undo deployment/nginx-deployment
The output is similar to this:
deployment.apps/nginx-deployment rolled back

Alternatively, you can rollback to a specific revision by specifying it with --to-revision:
kubectl rollout undo deployment/nginx-deployment --to-revision=2

The output is similar to this:
deployment.apps/nginx-deployment rolled back

Check if the rollback was successful and the Deployment is running as expected, run:
kubectl get deployment nginx-deployment

Get the description of the Deployment:
kubectl describe deployment nginx-deployment


```



## Scaling a Deployment



```YAML
You can scale a Deployment by using the following command:

kubectl scale deployment/nginx-deployment --replicas=10
```


