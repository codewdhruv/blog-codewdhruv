---
title: "Kubernetes: The Easy Way"
seoTitle: "Kubernetes: The Easy Way"
seoDescription: "In this tutorial we learn about Kubernetes and how it can be used to orchestrate containerized applications"
datePublished: Thu Mar 09 2023 11:24:22 GMT+0000 (Coordinated Universal Time)
cuid: clf10uc37000309maeg0t7enb
slug: kubernetes-the-easy-way
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678359660335/1ba0cdea-366d-48ad-8a4b-51e967998b95.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1678361034443/7b34909a-f969-42c8-bca0-5081aa8a14e6.png
tags: kubernetes, containers, k8s, container-orchestration

---

Kubernetes also known as K8s is an open-source container orchestration system that automates the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/). Kubernetes has become the de facto standard for container orchestration as it enables teams to manage and deploy containerized applications with ease. However setting up a Kubernetes cluster can be a daunting task, and many developers and system administrators struggle to get started with the platform. In this article, we will explore some of the easy ways to set up a Kubernetes cluster and learn how it can be used to orchestrate containerized applications.

### History Behind Kubernetes

Kubernetes was not born out of thin air rather it was conceived from the decade-long expertise that Google accumulated while managing containerized systems on a massive scale through its proprietary [Borg](https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/) and [Omega](https://queue.acm.org/detail.cfm?id=2898444) technologies.

These technologies empowered Google to run thousands of applications comprising hundreds of thousands of jobs, across numerous clusters. Eventually, Google made Kubernetes (K8s) publicly available as an open-source incarnation of Borg, opening up its battle-tested platform to the wider developer community.

### Kubernetes and Containers

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678349812191/c243ab22-2812-4b9b-8eb7-d9bd45d1c904.webp align="center")

Containers have revolutionized the way we develop and deploy applications by allowing us to bundle all the required dependencies and libraries into a single package, making it easier to ship applications across different environments. Unlike traditional virtual machines, containers share the host operating system kernel, making them lightweight and more efficient.

However, in production environments, it is crucial to ensure that services are available at all times. In the event of a container failure, it is imperative to have a mechanism in place to spawn a new container and continue the workload seamlessly. This is where container orchestration comes into play, which enables us to provision, schedule, and manages containers in a scalable and fault-tolerant manner.

Kubernetes is a powerful container orchestration system that provides an extensive set of features for deploying, scaling, and managing containerized applications. With Kubernetes, applications can be automatically deployed and updated, and health checks can be performed regularly to ensure they are functioning correctly. In the event of a failure, Kubernetes can automatically restart or replicate the affected application, providing high availability and failover capabilities.

Kubernetes provides a robust set of features for automating container deployments, including automated rollouts and rollbacks, traffic management, load balancing, and auto-scaling. Kubernetes also allows for easy integration with other container runtimes, such as [Docker](https://www.docker.com/), [rkt](https://www.redhat.com/en/topics/containers/what-is-rkt#:~:text=rkt%20is%20an%20application%20container,for%20integration%20with%20other%20systems.), and [Containerd](https://containerd.io/), making it a versatile platform for container orchestration. Its flexibility and scalability make it an ideal choice for organizations looking to deploy and manage containerized applications at scale, ensuring maximum availability and resilience.

### Understanding the K8s Architechture

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678350860757/7d8ec302-8a9e-4d4d-845b-cf574c8eb2d1.png align="center")

Kubernetes is designed with a decentralized architecture that follows a declarative model, where an administrator specifies the desired state of an application, and Kubernetes automatically applies and maintains that state.

To achieve this, the administrator creates a manifest file that captures the desired state of an application, which is then passed to the Kubernetes API Server using the [kubectl](https://kubernetes.io/docs/reference/kubectl/) command-line tool or a user interface.

The Kubernetes API Server stores the desired state of the application in a Key-Value Store ([etcd](https://etcd.io/)) and ensures that the current state of the application matches the desired state. The Key-Value Store serves as a single source of truth for the entire cluster, allowing Kubernetes to track changes and maintain consistency across the cluster.

Once Kubernetes receives the desired state of an application, it schedules and deploys the relevant containers across the cluster, ensuring the application's desired state is met. Kubernetes continuously monitors the state of the containers to ensure they remain in the desired state and performs automatic self-healing actions in case of any failures or discrepancies.

Moreover, Kubernetes has a robust and extensible ecosystem of plugins and APIs that allow for easy integration with other tools and technologies. This makes it a powerful platform for managing containerized applications at scale, ensuring high availability and scalability.

**Master Node in Kubernetes**

The Kubernetes Master Node serves as the central control plane for the entire Kubernetes cluster, responsible for managing and coordinating all the operations that take place within it. It acts as the primary interface for external communication with the cluster, receiving input from various sources such as CLI and UI via the Kubernetes API server.

The API server serves as the main entry point for all incoming API requests, handling everything from managing cluster resources, including pods, replica sets, and services, to managing the desired state of applications running in the cluster. Through the API server, users can define the specifications of these resources, including which container image to use, which ports to expose, and how many pod replicas to run.

The Master Node also encompasses other critical components, such as the Kubernetes scheduler, which is responsible for assigning pods to nodes based on available resources and workload requirements. The Kubernetes controller manager is another essential component responsible for maintaining the desired state of the cluster by continuously monitoring the current state and taking corrective action as needed.

The Master Node plays a crucial role in ensuring the stability and reliability of the Kubernetes cluster. Its centralization of management and control enables efficient and effective management of resources, provides automatic failover and redundancy, and simplifies the deployment and scaling of containerized applications. Ultimately, the Master Node is a vital component that forms the foundation of a highly scalable, reliable, and agile Kubernetes architecture.

**API Server**

The API Server serves as the entry point for all internal and external communications with the cluster. It acts as a gateway for users, management tools, and other system components to interact with the Kubernetes control plane. The API Server exposes a RESTful interface that enables users to query and manipulate the state of the cluster.

**Etcd (Key Value Store)**

The Key-Value Store, also known as etcd, is a distributed database that stores the entire configuration and state of the cluster. It provides a reliable and consistent way to store and retrieve information about the nodes, pods, and containers in the cluster. The Master node communicates with etcd to retrieve the current state of the cluster and to update it as needed.

**Controller Manager**

The Controller is responsible for ensuring that the current state of the nodes, pods, and containers matches the desired state specified by the user. It continuously monitors the state of the cluster and makes adjustments as needed to ensure that the desired state is maintained.

**Scheduler**

The Scheduler is responsible for placing new pods on suitable nodes within the cluster. It evaluates the available resources on each node and selects the best-suited node for each new pod. The Scheduler uses a range of criteria to determine the best-suited node, including available resources, affinity and anti-affinity rules, and other constraints. If there are no suitable nodes available, the Scheduler will place the pod in a pending state until a suitable node becomes available.

Overall, these components work together to provide a reliable and scalable platform for managing containerized applications. By leveraging the Kubernetes control plane, users can deploy and manage containerized applications with ease, while ensuring high availability, scalability, and fault-tolerance.

**What is Worker Node in Kubernetes Architecture?**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678352024260/f5f34f3f-2a54-436c-9123-4f071b5c2ebe.png align="center")

In a Kubernetes cluster, the worker nodes are responsible for executing the tasks assigned to them by the Kubernetes Master node. The worker nodes continuously listen to the API Server for new work assignments, which are communicated to them in the form of a Pod specification.

Once a worker node receives a Pod specification, it uses the container runtime to create and launch the corresponding containers. The worker node then monitors the status of the containers and reports back to the Kubernetes Master node on their progress.

During the execution of the work assignments, the worker nodes also perform a variety of other tasks, such as monitoring the health of the containers, managing networking and storage resources, and enforcing security policies.

Upon completion of the assigned work, the worker node reports the results back to the Kubernetes Master node, which updates the cluster state accordingly. This feedback loop enables the Kubernetes cluster to maintain a high level of reliability, scalability, and fault-tolerance, ensuring that applications continue to run smoothly and seamlessly even in the face of unexpected events or failures.

### Setting up a Kubernetes Cluster

1. **Managed Kubernetes services:**
    

The easiest way to set up a Kubernetes cluster is to use a managed Kubernetes service from a cloud provider. Many cloud providers, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), and Microsoft Azure, offer managed Kubernetes services. These services enable users to create and manage Kubernetes clusters easily without worrying about the underlying infrastructure. Managed Kubernetes services also provide features such as automatic scaling, high availability, and security.

1. **Kubernetes distributions:**
    

Another easy way to set up a Kubernetes cluster is to use a Kubernetes distribution. Kubernetes distributions are pre-packaged versions of Kubernetes that include all the necessary components and tools required to deploy and manage Kubernetes clusters. Kubernetes distributions, such as OpenShift, Rancher, and Tectonic, provide users with a web-based interface that simplifies the management of Kubernetes clusters. Additionally, these distributions often include additional features and tools, such as monitoring and logging, that make it easier to manage Kubernetes clusters.

1. **Kubernetes installers:**
    

For users who want more control over their Kubernetes deployment, Kubernetes installers can be an easy way to set up a Kubernetes cluster. Kubernetes installers, such as kubeadm, kops, and kubespray, provide users with a set of scripts and tools that automate the installation and configuration of a Kubernetes cluster. These installers can be used to set up Kubernetes clusters on bare metal servers or virtual machines. Additionally, Kubernetes installers can be used to set up Kubernetes clusters on cloud providers, such as AWS, GCP, and Azure.

1. **Containers-as-a-Service (CaaS):**
    

Containers-as-a-Service (CaaS) platforms provide users with an easy way to set up and manage containerized applications. These platforms, such as Docker Swarm and Mesosphere DC/OS, provide users with a web-based interface that simplifies the deployment and management of containerized applications. Additionally, CaaS platforms often include features such as automatic scaling, load balancing, and security that make it easier to manage containerized applications.

### What are K8s services and how does it work?

k8s is a powerful container orchestration platform that provides automatic pod replacement to ensure optimal application availability. Pods, which are the fundamental unit of deployment in Kubernetes, are ephemeral and can fail or terminate at any time. This can cause instability in the network environment, as the newly created pods often have different IP addresses.

To address this issue, k8s services provide a stable networking endpoint by assigning a fixed IP address, DNS name, and port to the pod. This allows any pod to be added or removed without affecting the basic network information.

k8s services work by using labels and selectors to associate pods with services. When a new pod is created with labels that match the selector, it is automatically discovered by the service and added to the cluster. Conversely, terminated pods are removed from the service, ensuring that the current state of the service always matches the desired state.

For instance, if a node running one replica of a pod fails and the desired state is three replicas, k8s detects the discrepancy and schedules a new replica to replace the failed one. The same applies when updating or scaling the application by adding or removing pods. The control panel runs background reconciliation loops that continuously check the environment to ensure it matches the user-defined requirements.

Thus the k8s services play a critical role in ensuring that containerized applications are reliable and scalable. By providing a stable networking endpoint, the services help maintain the desired state of the environment, even as pods are added or removed.

### Conclusion

Kubernetes offers a straightforward model for container orchestration, making it easy to deploy and manage applications at scale. By specifying the desired state of our system, it automatically works to ensure that the current state of our application within a cluster is aligned with the desired state. This approach simplifies the management of containerized applications, providing greater flexibility and scalability.

With this understanding of k8s architecture you are now equipped to embark on the practical task of creating and maintaining your own k8s clusters. Whether you are a developer or a systems administrator, k8s offers a powerful and versatile platform for deploying and managing containerized applications with ease. By leveraging to the full potential, you can take your application deployment and management to the next level, ensuring that your services are always available and performing at their best.