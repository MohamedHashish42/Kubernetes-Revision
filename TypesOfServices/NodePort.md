# NodePort
## Description
Services of type NodePort build on top of ClusterIP type services by exposing the ClusterIP service outside of the cluster on high ports (default 30000-32767). If no port number is specified then Kubernetes automatically selects a free port.   

The local kube-proxy is responsible for listening to the port on the node and forwarding client traffic on the NodePort to the ClusterIP.  
    
By default, every node in the cluster listens on this port, including nodes where the pod that matches the label selector does not run.

## Use Case
When you need to expose the service outside the cluster for external access.

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
         port: 80                     # Port on the service within the cluster
         targetPort: 9376             # Port on the pods selected by the service
         nodePort: 30000              # Optional: If not specified, Kubernetes will assign a random port from the range 30000-32767 by default
     type: NodePort
   ```


 ## Figure  
<div align="left">

  ![NodePort Figure](./NodePort%20Figure.PNG)

</div>
