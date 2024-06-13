<h2 dir="rtl" align="center">
بسم الله الرحمن الرحيم
</h2>

# Kubernetes  Revision
This revision is designed to provide a concise overview of Kubernetes, including basic and some advanced concepts. It is intended for developers, software engineers, and anyone interested in using Kubernetes.

<br>

---

## Introduction

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, management, and operation of containerized applications, providing a framework for efficient and resilient application hosting across clusters of machines.

### Why you need Kubernetes and what it can do?           
- **Orchestration:**  
Kubernetes provides a platform for orchestrating containers, which means it automates the process of 
deploying, scaling, and managing containers in a cluster of machines. It ensures that containers are always running 
as expected.  

- **Service discovery and load balancing:**  
Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable. 
- **Automated rollouts and rollbacks:**  
In Kubernetes, a rollout is the process of updating an application to a new version. This is done with rolling update stratgey, which allow for the update to take place with zero downtime by incrementally updating Pod instances with new ones.  
Automated rollouts and rollbacks is a feature in Kubernetes that allows for the automatic deployment of new versions 
of an application, as well as the ability to roll back to a previous version if necessary.
- **Automatic bin packing:**  
You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources 
- **Self-Healing:**  
When a container fails, Kubernetes can restart or replace it automatically to prevent downtime. It can also take down containers that don’t meet your health-check requirements. 
- **Horizontal scaling:**  
it allow you to scale your application up and down with a simple command, with a UI, or automatically based on CPU usage. 
- **Designed for extensibility:**  
It allow you add features to your Kubernetes cluster without changing upstream source code `(upstream source code refer to official codebase of Kubernetes maintained by the Kubernetes project)`.  
This is useful because it simplifies the process of upgrading Kubernetes to newer versions, as users can maintain their custom features separately from the core codebase.
- **Batch execution:**  
it can manage your batch and CI [workloads](https://www.armosec.io/glossary/kubernetes-workload/), replacing containers that fail, if desired.
- **Secret and configuration management:**  
Kubernetes lets you store and manage sensitive information, such as passwords,
OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your 
container images, and without exposing secrets in your stack configuration.



<br><br>

---

## Kubernetes Concepts
### Node
In Kubernetes (K8s), a node refers to a physical or virtual machine that is part of a cluster and runs containerized applications.   
Nodes are the workhorses of a Kubernetes cluster, responsible for executing and managing the containers that make up your applications.  

#### Some nodes commands
| Command                                               | description                                            |
|-------------------------------------------------------|--------------------------------------------------------|
|`kubectl get node`                                     |List nodes                                              |
|`kubectl get node -o wide`                             |List nodes with additional information                  |
|`kubectl describe node [node_name]`                    |Provides detailed information about a node              |
|`kubectl describe nodes \| grep Allocated -A 5`        |Resource allocation per node                            |
|`kubectl get pods -o wide \| grep <node_name>`         |Pods running on a node                                  |
|`kubectl delete node [node_name]`                      |Delete a node                                           |
|`kubectl top node`                                     |Display Resource usage (CPU/Memory/Storage) for nodes   |

### Cluster
In Kubernetes (K8s), a cluster is a collection of nodes that work together to manage and run containerized applications.  
When you deploy Kubernetes, you are essentially running a Kubernetes cluster. 

A Kubernetes cluster consists of two main components:

1. **Master Nodes**:   
Master Node hosts the control plane.  
Control plane is the brain of the cluster, where all the decisions and coordination happen, It includes 
[components](https://www.stackstate.com/blog/kubernetes-architecture-part-2-control-plane-components/) like the API server, controller manager, scheduler, and etcd.  
Master Node communicates with worker nodes to ensure that the desired state is maintained.  
A Kubernetes cluster can consists of one or more master nodes,but at least, it has one master node running a container pod and a control plane that manages the cluster.  

1. **Worker Nodes**:  
These are the nodes where your containerized applications run.

In a nutshell, a Kubernetes cluster is a system for automating the deployment, scaling, and management of containerized applications,
It provides a level of abstraction that lets you interact with your applications without needing to manage underlying infrastructure.

#### Some cluster management commands
| Command                           | description                                                   |
|-----------------------------------|---------------------------------------------------------------|
|`kubectl version`                  |Display the version information of the client `(kubectl command-line tool installed on your local machine)` and server `(Version of the Kubernetes API server running in your cluster)` for the current context|
|`kubectl cluster-info`             |Display addresses of the control plane and services with label kubernetes.io/cluster-service=true|
|`kubectl cluster-info dump`        | `dump` for further debug and diagnose cluster problems        | 
|`kubectl config view`              |Get the configuration of the cluster                           |
|`kubectl api-resources`            |List the API resources that are available                      |
|`kubectl api-version`              |List the API versions that are available                       |
|`kubectl get all --all-namespaces` |List everything                                                |


### Pod
In Kubernetes (K8s), a Pod is the smallest deployable unit where your application's code runs.   

Pods contain one (Single Container Pod) or more (Multi Container Pod) containers.  

Within the pod, containers share the same system resources such as storage and networking, This design allows the application composed of multiple containers that are tightly coupled to be co-located within the same Pod, facilitating seamless communication.    

Each Pod represents a running single instance of a given application. 

Each pod gets a unique IP address.

#### Pod Lifecycle
The [lifecycle](https://devopscube.com/kubernetes-pod-lifecycle/) of a Kubernetes Pod consists of several phases, each representing a different state in the Pod's journey from creation to termination. Here's a brief overview of the Pod lifecycle:

| Pod Phases | Description                                                                        |
|------------|------------------------------------------------------------------------------------|
| Pending    | The Pod is created but not yet running.                                            |
| Running    | At least one container is running, or is in the process of starting or restarting. |
| Succeeded  | All containers have been completed successfully.                                   |
| Failed     | At least one container has failed.                                                 |
| Unknown    | The Pod status couldn’t be obtained by the API server.                             |

#### Some pods commands
| Command                                               | description                                            |
|-------------------------------------------------------|--------------------------------------------------------|
|`kubectl create -f [pod-definition.yml]`               |Create pod                                              |
|`kubectl get pod`                                      |List pods                                               |
|`kubectl get pod -o wide`                              |List pods with additional information                   |
|`kubectl describe pod [pod_name]`                      |Provides detailed information about a pod               |
|`kubectl exec -it [pod_name] [command]`                |Get interactive shell on a single-container pod         |
|`kubectl exec [pod_name] -c [container_name] [command]`|Execute a command against a container in a pod          |
|`kubectl delete pod [pod_name]`                        |Delete a pod                                            |
|`kubectl get pod [pod_name] -o yaml > file.yaml`       |Extract the pod definition in YAML and save it to a file|
|`kubectl top pods`                                     |Display Resource usage (CPU/Memory/Storage) for pods    |

### Namespace
In Kubernetes (k8s), a namespace is like a virtual cluster inside a physical Kubernetes cluster, it is a mechanism to partition and isolate resources within the single cluster, Names of resources need to be unique within a namespace, but not across namespaces.

Namespaces are used to:

1. **Isolate Resources**:  
You can use namespaces to separate different applications or teams, preventing them from interfering with each other. For example, you can have one namespace for your web application and another for your database.

2. **Resource Quotas**:  
You can set resource quotas at the namespace level, limiting how much CPU, memory, and other resources can be consumed by the resources (pods, services, etc.) in that namespace.

3. **Access Control**:  
Kubernetes RBAC (Role-Based Access Control) policies can be applied at the namespace level. This allows you to control who can access and modify resources within a specific namespace.

4. **Naming and Organization**:  
Namespaces provide a way to give meaningful names to different parts of your application or project, making it easier to manage and maintain.

In summary, a Kubernetes namespace is a way to create virtual clusters within a Kubernetes cluster, helping you manage, isolate, and organize your resources more effectively.

#### Some namespaces commands
| Command                                               | description                                              |
|-------------------------------------------------------|----------------------------------------------------------|
|`kubectl create namespace [namespace_name]`            |Create namespace                                          |
|`kubectl get namespace`                                |List namespaces                                           |
|`kubectl get namespace -o wide`                        |List namespaces with additional information               |
|`kubectl describe namespace [namespace_name]`          |Provides detailed information about a namespace           |
|`kubectl edit namespace [namespace_name]`              |Edit and update the definition of a namespace             |
|`kubectl config set-context --current namespace=[namespace_name]` |Set the current context to use a namespace     |
|`kubectl delete namespace [namespace_name]`            |Delete a namespace                                        |
|`kubectl top namespace`                                |Display Resource usage (CPU/Memory/Storage) for namespaces|


### Service

In Kubernetes (k8s), service is an abstraction to help you expose groups of Pods over a network. 

Each Service object defines a logical set of endpoints (usually these endpoints are Pods) along with a policy about how to make those pods accessible. It acts as a stable endpoint to access a set of pods (instances of your application).

Imagine you have multiple pods running your web application, without a service, you'd have to manage each pod's IP address, and if a pod goes down or new pods are created, these IP addresses would change. the service abstracts this complexity.


#### Types Of Services 
In Kubernetes, services are used to expose applications running within a cluster to other applications or users outside the cluster. 
There are several **[types of services](https://www.ibm.com/docs/en/cloud-private/3.1.1?topic=networking-kubernetes-service-types)** in Kubernetes, each serving a specific purpose. Here are the main types:

1. **[ClusterIP](./TypesOfServices/ClusterIP.md)**
2. **[NodePort](./TypesOfServices/NodePort.md)**
3. **[LoadBalancer](./TypesOfServices/LoadBalancer.md)**


#### Some services commands
| Command                                               | description                                              |
|-------------------------------------------------------|----------------------------------------------------------|
|`kubectl expose po [pod_name] --port=80 --target-port=8080 --name-frontend --type=[service_type]`|Create a service to expose a pod|
|`kubectl expose deploy [deploy_name] --port=80 --target-port=8080 --type=[service_type]`|Create a service to expose a deployment|
|`kubectl create -f [Definition.yml]`                   |Deploy the service                                        |
|`kubectl get service`                                  |List services                                             |
|`kubectl get service -o wide`                          |List services with additional information                 |
|`kubectl describe service [service_name]`              |Provides detailed information about a service             |
|`kubectl edit service [service_name]`                  |Edit and update the definition of a service               |
|`kubectl delete service [service_name]`                |Delete a service                                          |



### Ingress
An Ingress is an API object that manages external access to services within a cluster. It commonly uses HTTP and HTTPS routing to services based on the rules you define.

ingress is a collection of rules, not a service. Instead, Kubernetes ingress sits in front of  **multiple services**  and acts as the entrypoint for an entire cluster of pods.

Ingress is made up of an Ingress API object and the Ingress Controller

1. The Ingress API   
is a set of rules and configurations defined in the Kubernetes API that describe how external HTTP and HTTPS traffic should be processed and routed to services within the cluster.

1. An Ingress Controller   
If Kubernetes Ingress is the API object that provides routing rules to manage external access to services, Ingress Controller is the actual implementation of the Ingress API. The Ingress Controller is usually a load balancer for routing external traffic to your Kubernetes cluster.

#### Ingress vs load balancer

The main difference is that ingresses are native objects inside the cluster that can route to multiple services, while load balancers are external to 
the cluster and only route to a single service.

#### Figure  
<div align="left">

  ![Ingress Figure](./Ingress/Ingress%20Figure.PNG)

</div>


### ReplicaSet
A [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) in Kubernetes is a resource that ensures a specified number of identical 
replicas (copies of a pod) are running at all times. It helps maintain the desired level of availability and scalability 
for your applications. If a pod fails or is deleted, the ReplicaSet automatically replaces it to meet the desired replica 
count. 

it is used by [Deployments](#deployment) internally to manage the desired number of replicas.

####


#### Some replicasets commands
| Command                                               | description                                              |
|-------------------------------------------------------|----------------------------------------------------------|
|`kubectl get replicaset`                               |List replicasets                                          |
|`kubectl get replicaset -o wide`                       |List replicasets with additional information              |
|`kubectl describe replicaset [replicaset_name]`        |Provides detailed information about a replicaset          |
|`kubectl delete replicaset [replicaset_name]`          |Delete a replicaset                                       |
|`kubectl scale replicaset [replicaset_name] --replicas=[number]`|Scale a replicaSet                               |


### Deployment <a id="deployment"></a>
A Kubernetes deployment (k8s) describe the desired state application, It specifies how many replicas of a pod should run on the cluster. If a pod fails, the deployment creates a new one.

it designed to provide declarative updates to applications. 

it handle the details of updating, scaling, and rolling back as needed.

**Key Features :**
- **Updates :** There are some way to update your application will be discussed in deployment strategies section.
- **Scaling :** Deployments can scale the number of replicas up or down based on the defined configuration.
- **Rollbacks :** If something goes wrong during an update, Deployments allow you to easily roll back to a previous version.
- **Declarative Configuration :**: You specify the desired state of your application in a YAML file.

####  Deployment Strategy
In Kubernetes, a deployment strategy refers to the approach used to release and update applications within a cluster. It defines how new versions of your application are rolled out and how the existing instances are scaled up or down during the deployment process. 
There are different deployment strategies to choose from based on your application's requirements and the desired balance between availability and reliability.

The two basic commonly used K8s deployment strategies
1. [Rolling Deployment](./DeploymentStrategies/Rolling%20Deployentment%20Example.md)
2. [Recreate Deployment](./DeploymentStrategies/Recreate%20Deployment%20Example.md)


#### Some deployments commands
| Command                                               | description                                              |
|-------------------------------------------------------|----------------------------------------------------------|
|`kubectl create deployment [deployment_name]`          |Create deployment                                         |
|`kubectl get deployment`                               |List deployments                                          |
|`kubectl get deployment -o wide`                       |List deployments with additional information              |
|`kubectl describe deployment [deployment_name]`        |Provides detailed information about a deployment          |
|`kubectl edit deployment [deployment_name]`            |Edit and update the definition of a deployment            |
|`kubectl delete deployment [deployment_name]`          |Delete a deployment                                       |    
|`kubectl delete deployment --all`                      |kubectl all deployments                                   |   

### Labels and Selectors
In Kubernetes (K8s), **Labels** and **Selectors** are simple but powerful concepts used for organizing and managing your containerized applications. Here's a very accurate and simple explanation:

#### Labels
- Labels are key-value pairs that you can attach to Kubernetes objects like pods, services, or nodes.
- They are used to add metadata or tags to these objects.
- For example, you can label pods with `app=frontend` and `env=production` to indicate that they are part of the production environment and belong to the frontend application

#### Selectors
- Selectors are a way to query and filter Kubernetes objects based on their labels.
- They allow you to group or target specific sets of objects that share certain labels.
- You can use selectors to define which objects should be affected by actions, like scaling, networking policies, or load balancing.
- For example, if you want to expose all pods with the label `app=frontend` to the internet, you can create a service with a selector that matches this label.

In a nutshell, **Labels** provide a lightweight way to add descriptive information to your Kubernetes objects, while **Selectors** enable you to efficiently manage and manipulate those objects based on their labels.

**Example**  
Let's say you have a basic pod definition in YAML with a label:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: myapp
spec:
  containers:
  - name: mycontainer
    image: nginx:latest
```

In this example, we have a pod named `mypod` with a label "app" set to `myapp`.   
This label is a key-value pair associated with the pod. 
You can apply this configuration to your Kubernetes cluster using the `kubectl apply` command:

```bash
kubectl apply -f pod-definition.yaml
```

Then, you can use labels to select and filter pods.   
For example, to get all pods with the label `app=myapp` you can use the following 
`kubectl` command:

```bash
kubectl get pods -l app=myapp
```
## Imperative and Declarative Configurations

This will show you a list of pods that have the specified label. Labels are powerful because you can use them for various purposes, such as grouping, filtering, and managing your Kubernetes resources.

In Kubernetes (K8s), imperative and declarative configurations are two different approaches to managing your resources and 
defining the desired state of your applications. Here's a simple and accurate explanation of both with examples:

### 1. Imperative Configuration:

   - **Definition**: Imperative configuration involves giving explicit step-by-step instructions to Kubernetes on how to create or modify a resource. You tell Kubernetes what to do, not what the desired end state should be.

   - **Example**: To create a deployment imperatively, you might use the `kubectl` command like this:
     ```
     kubectl create deployment my-app --image=my-image:latest
     ```
### 2. Declarative Configuration:

   - **Definition**: Declarative configuration involves specifying the desired state of a resource in a YAML or JSON file and applying it to Kubernetes. You tell Kubernetes what the desired end state should be, and Kubernetes takes care of making it happen.

   - **Example**: To create a deployment declaratively, you create a YAML file, such as `my-app-deployment.yaml`, like this:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: my-app
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: my-app
       template:
         metadata:
           labels:
             app: my-app
         spec:
           containers:
           - name: my-app-container
             image: my-image:latest
     ```
     You then apply this configuration with `kubectl`:
     ```bash
     kubectl apply -f my-app-deployment.yaml
     ```
- **Structure of declarative configurations:**  
The structure of declarative configurations in Kubernetes typically follows a specific format and hierarchy. Here's a breakdown of the key elements 
and their structure:
1. **API Version**: Specifies the API version of the Kubernetes resource object. It is used to define which version of the Kubernetes
    API should be used for this resource. For example:

   ```yaml
   apiVersion: apps/v1
   ```

2. **Kind**: Defines the type of Kubernetes resource being created or configured. Common kinds include Deployments, 
    Services, ConfigMaps, and Pods:

   ```yaml
   kind: Deployment
   ```

3. **Metadata**: Contains information about the resource, including its name, namespace, labels, and annotations. For example:

   ```yaml
   metadata:
     name: my-deployment
     labels:
       app: my-app
   ```

4. **Spec**: Specifies the desired state of the resource. The structure of the `spec` section depends on the kind of resource. 
For a Deployment, it might include information about the desired number of replicas, container images, and other settings:

   ```yaml
   spec:
     replicas: 3
     template:
       metadata:
         labels:
           app: my-app
       spec:
         containers:
         - name: my-container
           image: my-image:latest
   ```

1. **Status**: Kubernetes updates this section to reflect the actual state of the resource, and it is typically not included in the declarative configuration files. 
The status section is maintained by Kubernetes itself.
### Conclusion
Declarative configuration is generally considered better for managing Kubernetes resources in most cases, especially in production environments. It promotes a clear and reproducible desired state for your applications and infrastructure. It's more predictable, supports 
version control, and allows for easier collaboration and automation. Imperative commands have their place for quick tasks and debugging, but they are not recommended for ongoing resource management due to their limitations and potential for errors.

In summary, for production workloads in Kubernetes, it's generally better to use declarative configurations. They help maintain the desired state of your resources, making it easier to manage and update applications as your infrastructure evolves. Imperative commands are more suitable for quick tasks and experimentation.



