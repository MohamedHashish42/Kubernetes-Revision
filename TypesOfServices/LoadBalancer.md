# LoadBalancer
## Description
The LoadBalancer service type is built on top of NodePort service types by provisioning and configuring external load balancers from public and private cloud providers.

## Use Case 
When you are running your cluster in a cloud environment and you want external access with load balancing.

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
     type: LoadBalancer
   ```

## Figure
<div align="left">

  ![LoadBalancer Figure](./LoadBalancer%20Figure.PNG)

</div>

## The advantage of LoadBalancer over NodePort

The advantage of LoadBalancer over NodePort is that it provides a stable external IP that remains the same, making it more suitable for scenarios where the external IP needs to be consistent, such as when exposing services to the internet.

While NodePort provides external access, it doesn't offer the same load balancing capabilities as a LoadBalancer. It exposes the service on a specific port on all nodes, and the external IP is typically that of the nodes themselves.
