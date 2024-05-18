
# 1- Rolling deployment
Is the default deployment strategy in Kubernetes. it replaces pods, one by one, of the previous version of your application with pods of new version without cluster downtime.


When using the RollingUpdate strategy, two options let us fine-tune the update process:

1. **maxSurge:**   
- This is the maximum number of additional pods that can be created above the desired replica count during a rolling update.   
This can be an absolute number or percentage of the replicas count. The default is 25%.


- For example, if you have 5 pods running (desired count) and set `maxSurge` to 1 during an update, Kubernetes may temporarily create 6 pods (5 desired + 1 surge) to introduce the new version gradually. After ensuring the new pods are healthy, it then scales down the old pods.

- This helps to prevent downtime by ensuring there's always a sufficient number of pods available to handle the workload during updates.   
The `maxSurge` parameter provides flexibility and control over the update process, allowing you to balance between ensuring high availability and minimizing resource consumption.

2. **maxUnavailable:** 
- This is the maximum number of pods that can be unavailable  during a rolling update. 
This can be an absolute number or percentage of the replicas count. The default is 25%.

- For example, if you set maxUnavailable to 1, Kubernetes ensures that only one old pod is taken down at a time while new pods are being deployed. 
This gradual replacement minimizes the impact on the overall availability of your application during the update.


# Example
Here is an example of a Deployment that creates three nginx pods and uses the rolling update strategy with `maxUnavailable` set to 1 and `maxSurge` set to 2:

1. Create a file named `nginx-rolling-update-deployment.yaml` with the following content:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-rolling-update-deployment
     labels:
       app: nginx
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx
     strategy:
       type: RollingUpdate
       rollingUpdate:
         maxUnavailable: 1
         maxSurge: 2
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
   ```

2. Apply the deployment file to create the deployment and the pods:

   ```bash
   kubectl apply -f nginx-rolling-update-deployment.yaml
   ```

3. You can Check that the Deployment has created a ReplicaSet and three pods by running:

   ```bash
   kubectl get rs
   kubectl get pods
   ```
4. update the Deployment by changing the image version to 1.16.1 as the following :
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-rolling-update-deployment
     annotations:
       kubernetes.io/change-cause: "Updating to nginx:1.16.1"
     labels:
       app: nginx
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx
     strategy:
       type: RollingUpdate
       rollingUpdate:
         maxUnavailable: 1
         maxSurge: 2
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
         - name: nginx
          image: nginx:1.16.1 
           ports:
           - containerPort: 80
   ```

   **Note**
   I have added the following metadata to specify version when showing history  
   ```yaml
     annotations:
        kubernetes.io/change-cause: `Updating to nginx:1.16.1`
   ```
   and run: 
   ```bash
      kubectl apply -f nginx-rolling-update-deployment.yaml
   ```

5. You can check the status of the rollout by running:
   ```bash
      kubectl rollout status deployment/nginx-rolling-update-deployment
   ```

You can see the rollout history by running:
```bash
kubectl rollout history deployment/nginx-rolling-update-deployment
```

You can rollback to a previous revision by running:

```bash
kubectl rollout undo deployment/nginx-rolling-update-deployment
```

You can pause and resume the rollout by running:

```bash
kubectl rollout pause deployment/nginx-rolling-update-deployment
kubectl rollout resume deployment/nginx-rolling-update-deployment
```
