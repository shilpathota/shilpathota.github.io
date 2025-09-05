---
title: Kubernetes Deployment Strategies
layout: default
id: k8s-deployment-strategies
---

# Kubernetes Deployment Strategies

**Resiliency** in Kubernetes begins with selecting the correct workload. It is about designing the application to withstand and ensure high availability. Rather than relying on a singular pod which lacks automatic resiliency capabilities, opt for scalable workloads.
This allows applications to recover gracefully from disruptions, minimizing downtime. By distributing application across multiple pods, you increase **fault tolerance** and enhance the overall resilience of your system.

In kubernetes, **security** is paramount.It involves granting each Pod precisely the permissions it needs to function effectively. However, it is essential to note that when it comes to the root user within a Pod, there are typically no restrictions imposed by default. This lack of restriction can pose a significant security risk, making it crucial for administrators and developers to implement proper access controls and restrict root access within the pods to ensure overall cluster security. 

There are two types of Kubernetes pods which are stateful and stateless pods. Understanding which one to use is critical to designing effecient and reliable containerized applications.

**Stateless Pods** are used in circumstances where information does not need to be preserved or persisted. These pods are well suited for applications that can function independently of past interactions and do not rely on retaining user-specific data and are designed to be easily replaceable and scalable as they dont store any critical state information. 

**Stateful Pods** comes into play when you have input data that needs to be preserved and maintained over time like the applications that use database and preserving state. 

Unlike stateless pods, stateful pods require careful handling to ensure data persistence and consistency. The primary difference between stateful and stateless pods lies in how they handle the data and state. Stateless pods are state-agnostic and don't rely on preserving data between instances.

Stateful pods are designed to preserve data making them suitable for applications where maintaining state is essential. In Kubernetes deployment strategies, effective planning and organization is essential. It begins with a clear understanding of your data and your objectives, which helps determine the most suitable deployment stratgy for your application.

Kubernetes offers various options including standard pods, replica sets, deployments, stateful sets, daemon sets, each tailored to specific use cases. Choosing right strategy ensure that your workloads are maintained efficiently and align with your project's goals whether it involves scaling fault tolerance or stateful application requirements. Workload creation for kubernetes, numerous factors can be considered, including resource needs, security measures and health checks. Any pods play a role in executing pre-pod creation tasks, such as running initialization scripts.

Secreats are essential for securing sensitive information like passwords, ensuring they are not embedded in the container's image to enhance the security.

## Differnces between Deployment Vs StatefulSet Vs DaemonSet Vs ReplicaSet

The critical difference between 3 types of sets, Deployment, StatefulSets, DaemonSets is each serves distinct purposes and usecases in the kubernetes eco system.
### Deployment
These are ideal for stateful applications that need scalability and smooth image upgrades. Use deployments when you want to horizantally scale a web app or perform rolling updates with minimal disruption. 
### Stateful Sets
These are designed for applications requiring data persistence and suitable network identities. Use StatefulSets when you need to preserve data like databases or clustered systems ensuring ordered deployment and persistent storage.
### DaemonSets
These are best suited for deploying specific applications that must run on every node within the cluster, enforcing rules of providing cluster level functionality. Use DaemonSets for tasks like log collection for running security agents on all your nodes.
### ReplicaSets
These are used for scaling applications by managing multiple instances of same pod. Deployments built on Replicasets offer advanced features like rolling updates and rollbacks for smoother application management. This also helps reduce the traffic to one particular instance and also helps in load-balancing traffic between each of these instances.

## Kubernetes Deployment Strategy Components
These are used in workload creation to define how application updates and scaling operations are managed.
 Kuberenetes provides the deployment resource, which is commonly used to create and manage workloads. You define the desired state of the application, including a number of replicas and container images you wish to use. Deployments are a high-level abstraction built on top of Kubernetes replica sets, making it easier to manage and update applications in a kubernetes cluster.
An update strategy is a defined approach or set of rules for how changes or updates to a workload are managed and applied. Update strategies include rolling updates, recreate and custom each with its own characteristics and behavior. 

Rolling updates are the default strategy that gradulaly update the application while maintaining a balance between the old and new replicas.

Recreate Strategy replaces all the old pods with new ones potentially causing downtime during updates.

Custom strategies are generally utilized for more advanced scenarios in which you can create custom controllers and specify a custom update strategy. These strategies specify how new versions of an application are rolled out while ensuring high availability and minimizing downtime.

Kubernetes rollback strategies are predefined procedures for mechanism for reverting to a previous stable state of an application or workload when an update or deployment foes away or causes issues. These stategies ensure that you can quickly and safely return to a known good state, minimize downtime, and maintain the reliability of application.

Kubernetes offers horizantal pod autoscaling, HPA and vertical pod autoscaling, VPA for automatic scaling based on resource usage and matrix. 
**HPA** is kubernetes feature that automatically adjusts the number of pods replicas based on the metrics like the amount of CPU or memory usage.
It scales the number of pods horizontally, adding or removing replicas to match the desired matrix targets. 

**VPA** is kubernetes feature that adjusts the resource requests and the limits of individual pods based on their actual resource usage. It optimizes resource allocations vertically, ensuring each pods gets the appropriate CPU and memory resources to operate efficiently. 

Tools like Helm, Argo Rollouts and Operators provide advanced deployment strategies and management capabilities for complex workloads. 
Kubernetes REsource Manager involves specifying resource requests and limits for containers within pods to allocate CPU and memory resources effectively. To ensure performance and stability by preventing resource contention and ensuring the pods have the necessary resources to run smoothly is essential.

Resource requests indicate the minimum required resources, while limits set an upper ceiling to be bound to prevent resource hogging, ensuring that worklaods don't interface with each other and maintaining the overall cluster stability.




