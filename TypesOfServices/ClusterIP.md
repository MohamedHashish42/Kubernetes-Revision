# ClusterIP

## Description
This is the default type of service. It exposes the service (groups of Pods) on a cluster-internal IP address 
(it assigns a virtual IP address to a service that can only be accessed from within the cluster)
## Use Case
It's used when you want to make the service accessible only within the cluster and not from outside.

## Example
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     selector:
       app: MyApp
     ports:
       - protocol: TCP
         port: 80                         # Port on the service within the cluster
         targetPort: 9376                 # Port on the pods selected by the service
     type: ClusterIP
   ```

## Figure
<div align="left">

  ![ClusterIP Figure](./ClusterIP%20Figure.PNG)

</div>