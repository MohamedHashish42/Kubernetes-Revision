
# Recreate Deployment

It terminates all existing instances (pods) of the application associated with the deployment and then creates new instances with the updated configuration.

This means that there is a short period of downtime when the old Pods are removed and the new ones are not ready yet. 

Recreate Deployment is useful when your application does not support running different versions at the same time, or when you can tolerate a brief interruption of service.

## Example
Here is an example of a Deployment that creates three nginx pods and uses the recreate deployment strategy
1. Create a file named `nginx-recreate-deployment.yaml` with the following content:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-recreate-deployment
     labels:
       app: nginx
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx
     strategy:
       type: Recreate 
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
   kubectl apply -f nginx-recreate-deployment.yaml
   ```

3. Check the status of the deployment and the pods:

   ```bash
   kubectl get deployment 
   kubectl get pods
   ```
4. update the Deployment by changing the image version to 1.16.1 as the following:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-recreate-deployment
     labels:
       app: nginx
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx
     strategy:
       type: Recreate 
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

    and then run
    ```bash
       kubectl apply -f nginx-recreate-deployment.yaml
    ```

5. Check the status of the deployment and the pods again:

   ```bash
   kubectl get deployment
   kubectl get pods 
   ```
   You should see that the old pods are deleted and the new pods are created with the updated image.
