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