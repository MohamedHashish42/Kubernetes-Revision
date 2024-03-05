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
