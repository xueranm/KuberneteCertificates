# KCNA Preparation 
## Pre-Knowledge/Skills requirements


<img width="479" alt="image" src="https://user-images.githubusercontent.com/24993672/210115793-3dfc5f75-37d3-458a-85c9-c4f64dd89e9f.png">

## Basics of Cloud Native Technologies
### 1. Traditional/Legacy apps

Monolithic approach (self0contained and include all the functionality and components)
Pro: easy to develop and deploy
Con: hard to manage complexity, scale development across multiple teams, implement changes fast, scale the app out efficiently with a lot of load


### 2. Cloud native architecture 

break down your app in smaller pieces which makes them more manageable.


### 3. Characteristics of Cloud Native Architecture


* High Level of Automation\
  Achieved by using __Continuous Integration/Continuous Delivery (CI/CD) pipelines__ that are backed by a version control system like git. Building, testing and deploying apps and infrastructure with minimal human involvement allows for fast, frequent and incremental changes to production. Great for much easier disaster recovery.
* Self Healing\
  Monitor: health checks, compartmentalized applications
* Scalable\
  Application metrics: CPU or memory -> ensure availability and performance of your services
* Cost-Efficient\
  Scale down if traffic is low
* Easy to Maintain\
  __Microservices__ allows to break down apps in smaller pieces and make them more portable, easier to test and distribute
* Secure by default\
  Calls for different security models for different teams and users. For ex, __Zero trust computing__


### 4. Starting point example: [the 12-factor app](https://12factor.net/)

### 5. Knowledge Points


* Autoscaling\
  Horizontal scaling: spawning new compute resources (new copies). \
  Vertical scaling: Change in size within certain hardware limits. 
  > To illustrate the difference between scaling vertically and horizontally, imagine that you have to carry a heavy object that you cannot pick up. You can build muscle to carry it yourself, but your body has an upper limit of strength. That's vertical scaling. You can also call your friends and ask them to help you and share the work. That's horizontal scaling.
  Configurations: minimum & maximum limit of instances, metric that triggers the scaling -> tune -> run many load tests, analyze the behavior and load balancing while scaled.
* Serverless\
  Infrastructure as a Service\
  Platform as a Service\
  Function as a Service\
  <img width="921" alt="image" src="https://user-images.githubusercontent.com/24993672/210119211-2c3be7d5-3186-490f-953d-0e190e41678f.png">\
  __Serverless architecture example__
  <img width="970" alt="image" src="https://user-images.githubusercontent.com/24993672/210119489-191c2ab1-4b92-481b-87bd-0d6a9a530b04.png">
  > Writing small, stateless applications make them a perfect fit for event or data streams, scheduled tasks, business logic or batch processing.
* Open Standards\
  How to build and distribute software packages? Containers (with Docker) -- [Open Container Initiative](https://opencontainers.org/) (OCI): image, runtime and distribution specification on how to run, build an distribute containers\
  __Some Other Standards__:\
  Container Network Interface (CNI): A specification on how to implement networking for Containers.\
  Container Runtime Interface (CRI): A specification on how to implement container runtimes in container orchestration systems.\
  Container Storage Interface (CSI):A specification on how to implement storage in container orchestration systems.\
  Service Mesh Interface (SMI): Ap specification on how to implement Service Meshes in container orchestration systems with a focus on Kubernetes.
  
  

## Container Orchestration (CO)
### 1. Container vs Virtual Machines


* Containers\
  Legacy way to run an app on a server: install and configure the basic OS, install the Core PL(for ex python) packages to run the program, install PL extensions that your program uses, configure networking for your system, connect to third party systems like a database, cache or storage.\
  Then VM comes.\
  Containers lower the overhead: Managing the dependencies of an application and running much more efficiently than spinning up a lot of virtual machines.
* Difference between VM and Container\
  VM emulate a complete machine, including the OS and a kernel (greater isolation, with some overhead caused by boot time)\
  Containers share the kernel of the host machines and are only isolated processes (faster and smaller footprint)
  <img width="628" alt="image" src="https://user-images.githubusercontent.com/24993672/210155962-79cb2ac6-14c2-4fef-b2f9-186437d495d1.png">
  <img width="565" alt="image" src="https://user-images.githubusercontent.com/24993672/210156165-d1b59d56-d0e5-4088-9710-17444539a43c.png">


### 2. Fundamentals of Container Orchestration
  > Container orchestration systems provide a way to build a cluster of multiple servers and host the containers on top
  Microservice architecture
  Most container orchestration systems consist of two parts
  * __Control Plane__ (management of the containers)
  * __Worker Nodes__ (host the containers)
  The problems to be solved:\
  * Providing compute resources like virtual machines where containers can run on
  * Schedule containers to servers in an efficient way
  * Allocate resources like CPU and memory to containers
  * Manage the availability of containers and replace them if they fail
  * Scale containers if load increases
  * Provide networking to connect containers together
  * Provision storage if containers need to persist data.


### 3. Challenges of Container Networking and Storage
  * Networking
  Each continer has its own unique IP address thus they can open the __same network port__.\
  To make app __accessible from outside the host system__, containers can map a port from the container to a a port from the host system. \
  To allow __communication between containers across host__, use overlay network which puts them in a __virtual network__ across the host systems.\
  <img width="595" alt="image" src="https://user-images.githubusercontent.com/24993672/210156392-3296e702-fd3c-4f26-80ef-ed6fd3370fcf.png">\
  Container Network Interface (CNI)
  * Service Discovery & DNS
  > Instead of having a manually maintained list of servers (or in this case containers), all the information is put in a __Service Registry__. Finding other services in the network and requesting information about them is called __Service Discovery__.
  
  Approaches to Service Discovery:
  1. DNS
  2. Key-Value-Store

  
### 4. Service Mesh
  * Proxy: Manage network traffic (nginx, haproxy or envoy)
  
  <img width="558" alt="image" src="https://user-images.githubusercontent.com/24993672/210157122-6ca0f187-1e87-46c3-bc0b-a11ba4c5861f.png"> [Istio Architecture](https://istio.io/v1.10/docs/ops/deployment/architecture/)
  > The proxies in a service mesh form the data plane. This is where networking rules are implemented and shape the traffic flow.

  > These rules are managed centrally in the control plane of the service mesh. This is where you define how traffic flows from service A to service B and what configuration should be applied to the proxies.\
   Service Mesh Interface (SMI)
   * Storage
     Container and its storage are ephemeral. As pic, the R/W layer is lost when container is stopped or deleted. 
     <img width="626" alt="image" src="https://user-images.githubusercontent.com/24993672/210157290-b9a3cb9b-dd49-41f8-982f-2da2d1ad7e8a.png">\
     To Persist data on a host, use __volume__: 
     > Instead of isolating the whole filesystem of a process, directories that reside on the host are passed through into the container filesystem
     <img width="605" alt="image" src="https://user-images.githubusercontent.com/24993672/210157354-0de3684b-8cd9-45a8-adfe-e5f63573d6c0.png">\
     Data is shared between containers on the same host. Storage is provisioned via a central storage system. Containers on Server A and Server B can share a volume to read and write data\
     Container Storage Interface (CSI) 
     
## High level architecture of Kubernetes
> Kubernetes is a highly popular open-source container orchestration platform that can be used to automate deployment, scaling and the management of containerized workloads
   ### Architecture
   <img width="714" alt="image" src="https://user-images.githubusercontent.com/24993672/210282591-5952f531-27f9-4cdd-a5e6-0e5590c61c55.png">
   
   __Control Plane__
   1. [Kube-apiserver](#kubernetes-api-kube-apiserver-restful-inerface-over-https): where user access the cluster and other components interact with 
   2. etcd: a database that holds the state of the cluster, a standalone project 
   3. [kube-scheduler](KCNA-Preparation.md#scheduling-kube-scheduler): Based on different properties like CPU and memory, it chooses new worker node to fit new workload 
   4. kube-controller-manager: container different non-terminating control loops (like control flow within lifecycle?) that manager the state of the cluster
   5. cloud-controller-manager (optional): interact with the API of cloud providers to create external resources like load balancers, storage or security groups (for ex, aws resources)
   
   
   __Worker Nodes__
   1. container runtime: used to run the containers on the worker node, for ex Docker
   2. kubelet: a small agent on every worker node, it talks to the api-server and container runtime to handle the final stage of starting containers
   3. kube-proxy: network proxy for inside/outside communication of the cluster\
   __One Pro__ of the separation design is even the control plane is not available, the started applications on worker nodes will continue to run\
   __Kubernete namespaces__ is used to divide a cluster into multiple virtual clusters (multi-tenancy)
   
   
   
   ### Kubernetes API (kube-apiserver: RESTful inerface over HTTPS)
   <img width="727" alt="image" src="https://user-images.githubusercontent.com/24993672/210284285-4e1f9e9c-fc51-45eb-ada3-a4647c6063cd.png">
   
   1. Authentication: require identity. For ex, digital signed certificate. Kubernetes users: external identity; Technical users: Service Accounts
   2. Authorization: Decide what the requester is allowed to do. Kubernetes Role Based Access Control (RBAC)
   3. Admission Control: used to modify or validate the request. For ex, Open Policy Agen (OPA) can be used to manage admission control externally


  ### Running Containers on Kubernetes
  * Kubernete translate the pod (the smallest compute unit) into a running container, image the pod as a wrapper around a container
  * Several container runtime: containerd, docker, CRI-O
  * Flow:
  <img width="740" alt="image" src="https://user-images.githubusercontent.com/24993672/210285382-0fa02069-9fda-4e0a-b68a-cc6672d5759a.png">
  <img width="756" alt="image" src="https://user-images.githubusercontent.com/24993672/210285467-47246a90-fa50-4f5f-98d3-caf7acd71cb7.png">
  
  
  ### Networking
  
  * Container-to-Container communications
  * Pod-to-Pod communications
  * Pod-to-Service communications
  * External-to-Service communicatioins
  
  [High level solutions](#3-challenges-of-container-networking-and-storage)
  > Most Kubernetes setups include a DNS server add-on called __core-dns__, which can provide service discovery and name resolution inside the cluster
  
  
  ### Scheduling (kube-scheduler)
  
  * Makes the scheduling decision, but not start the workload
  * After user declares the configurations, the scheduler will use that information to filter all nodes that fit these requirements. If multiple nodes fit the requirements equally, Kubernetes will schedule the Pod on the node with the least amount of Pods
  * If the desired state cannot be established, the scheduler will retry to find an appropriate node until the state can be established.
  
  
## Kubernetes Objects, Purposes, Interactions

  ### Kubernetes Objects
  
  1. Workload-oriented Objects (handle container workoads)
  2. Infrastructure-oriented Objects (handle configuration, networking and security)
  
  * Required fileds: apiVersion, kind, metadata, spec

  For more details, see Kubernetes official doc. 
  
  ### Interaction
  
  * kubectl
    ~~~~
    kubectl api-resources
    kubectl explain pod
    kubectl explain pod.spec
    kubectl config view
    kubectl --help
    ~~~~
  * Helm
    > package manager for Kubernetes, which allows easier updates and interaction with objects

  
  ### Pod
  
  > A pod describes a unit of one or more containers that share an isolation layer of namespaces and cgroups
  <img width="690" alt="image" src="https://user-images.githubusercontent.com/24993672/210287436-654dbdcc-598f-47ca-bb76-7beb2f28783f.png">

  * sidecar container: second container that supports the main application
  * initContainers: to start containers before your main application starts
  * Important settings for every container: resources, livenessProbe, securityContext


  ### Scale and Schedule Pods with workload resources
  ### Abstract Pods with services and expose them

   
   
   
## How CO differes from legacy deployments 




## Deliver and monitor your app in a distributed system


