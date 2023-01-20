# KCNA Preparation 
## Pre-Knowledge/Skills requirements (LFS250)


<img width="479" alt="image" src="https://user-images.githubusercontent.com/24993672/210115793-3dfc5f75-37d3-458a-85c9-c4f64dd89e9f.png">

## Basics of Cloud Native Technologies
### 1. Traditional/Legacy apps

Monolithic approach (self-contained and include all the functionality and components)\
Pro: easy to develop and deploy\
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

  Tune configurations: minimum & maximum limit of instances, metric that triggers the scaling -> run many load tests, analyze the behavior and load balancing while scaled.
* Serverless\
  Infrastructure as a Service\
  Platform as a Service\
  Function as a Service\
  <img width="921" alt="image" src="https://user-images.githubusercontent.com/24993672/210119211-2c3be7d5-3186-490f-953d-0e190e41678f.png">
  
  __Serverless architecture example__
  <img width="970" alt="image" src="https://user-images.githubusercontent.com/24993672/210119489-191c2ab1-4b92-481b-87bd-0d6a9a530b04.png">
  
  > Writing small, stateless applications make them a perfect fit for event or data streams, scheduled tasks, business logic or batch processing.
* Open Standards\
  How to build and distribute software packages? \
  Containers (with Docker) -- [Open Container Initiative](https://opencontainers.org/) (OCI): image, runtime and distribution specification on how to run, build an distribute containers\
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

  The problems to be solved:
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
   4. kube-controller-manager: container different non-terminating control loops (like control flow within lifecycle?) that watch and manage the state of the cluster, make or request changes where needed. Try to move the current cluster state closer to the desired state.
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
  
__Interaction__
  
  * kubectl
    ~~~~
    kubectl api-resources
    kubectl explain pod
    kubectl explain pod.spec
    kubectl config view
    kubectl --help
    kubectl get pod -o wide
    ~~~~
  * Helm
    > package manager for Kubernetes, which allows easier updates and interaction with objects

__Pod__
  
  > A pod describes a unit of one or more containers that share an isolation layer of namespaces and cgroups
  <img width="690" alt="image" src="https://user-images.githubusercontent.com/24993672/210287436-654dbdcc-598f-47ca-bb76-7beb2f28783f.png">

  * sidecar container: second container that supports the main application
  * initContainers: to start containers before your main application starts
  * Important settings for every container: resources, livenessProbe, securityContext

__Workload Objects__

  * ReplicaSet
  * Deployment
  * StatefulSet
  * DaemonSet: Used for infrastructure-related workload, for ex, monitoring or logging tools
  * Job
  * CronJob
  
  Visualization tools: KubeView
  
__Networking Objects__

  * ClusterIP: virtual IP that can be used as a single endpoint for a set of pods, can be used as a round-robin load balancer
  <img width="466" alt="image" src="https://user-images.githubusercontent.com/24993672/210288368-277a335e-fef8-4502-8998-4cebb0fbaf9c.png">
  
  * NodePort: extends the ClusterIP by adding simple routing rules, opens a port on every node and maps it to the ClusterIP. Allow routing external traffic to the cluster
  * LoadBalancer: extends the NodePort by deploying an external LoadBalancer instance
  * ExternalName: uses Kubernete internal DNS server to create a DNS alias 
  
__Ingress Object__

  > Ingress provides a means to expose HTTP and HTTPS routes from outside of the cluster for a service within the cluster. It does this by configuring routing rules that a user can set and implement with an ingress controller.
  <img width="619" alt="image" src="https://user-images.githubusercontent.com/24993672/210288783-e41ca5cc-785b-44b2-b7f8-5c894584af51.png">

  Cluster internal firewall: __NetworkPolicy__ -- IP firewall that can control traffic based on rules
  
__Volume & Storage Objects__

  > __Volumes__ allow sharing data between multiple containers within the same Pod (great for sidecar pattern, preventing data loss when a pod crashes)
  >  A cluster environment with multiple servers requires even more flexibility when it comes to __persistent storage__

  Container Storage Interface (CSI): allows the storage vendor to write a plugin (storage driver) that can be used in Kubernetes\
  Two Objects that use this abstraction:
  * PersistentVollumes (PV): configurations that holds information like type of volume, volume size, access mode and unique identifiers and info how to mount it
  * PersistentVolumeClaims (PVC): Request for storage by a user, can reserve a PV

Example
<img width="627" alt="image" src="https://user-images.githubusercontent.com/24993672/211174956-50cd1134-8ed0-42f1-9614-08eb375a87f5.png">

__Configuration Objects__

  > Suggestion: [Storing configuration in the environment](https://12factor.net/config)

  Kubernete: decoupling the configuration from the pods with a ConfigMap\
  Usage:
  * Mount ConfigMap as a volume in Pod
  * Map variables from a ConfigMap to env variables of a Pod

  Similar Object to ConfigMaps --  __Secrets__ base64 encoded.
  
__Autoscaling Objects__

  * Horizontal Pod Autoscaler (HPA)
    To use, need to install an add-on called __metrics-server__.
  * Cluster Autoscaler
  * Vertical Pod Autoscaler
  
  To distinguish them,  refer [here](#5-knowledge-points)


## Cloud Native Application Delivery (CI/CD) and Observability

### Automation
   
### CI/CD
   
__App Lifecycle__
   * Write Codes and Version Control
     > Git: decentralized system that can be used to track changes in your source code
   
   * Build: for ex, container image
   * Extensive and automatic testing of the app
   * Delivering: for ex, use Kubernete as target platform


__Automation and Infrastructure as Code (IaS)__

  > __Continuous Integration__ is the first part of the process and describes the permanent building and testing of the written code. High automation and usage of version control allows multiple developers and teams to work on the same code base.

  > __Continuous Delivery__ is the second part of the process and automates the deployment of the pre-built software. In cloud environments, you will often see that software is deployed to Development or Staging environments, before it gets released and delivered to a production system.
   
__Approaches how CI/CD implement changes__

  * Push-based
    > The pipeline is started and runs tools that make the changes in the platform. Changes can be triggered by a commit or merge request.
  * Pull-based (GitOps frameworks exs -- Flux, ArgoCD)
    > An agent watches the git repository for changes and compares the definition in the repository with the actual running state. If changes are detected, the agent applies the changes to the infrastructure.
    
    <img width="461" alt="image" src="https://user-images.githubusercontent.com/24993672/211176597-9dab6703-28d9-43ce-bceb-8eb2139566f0.png">

__Observability__

  The concept is similar to Control Theory -- describes how external outputs of systems can be measured to manipulate the behavior of the system\
  Example: Cruise Control System of A Car: can set the desired speed of the car, which is measured and observed by a person with the speedometer. To maintain the speed in changed conditions (ex. up to mountain), the power of the motor must be modified. 
  
  * Telemetry
      Built-in Tools in container systems with collecting and transferring data in a centralized system:
      1. __Logs__: Emitted from apps about errors, warnings or debug info.
         Unix and Linux Programs Three I/O streams (to output logs from containers):
         - stdin: file descriptors 0
         - stdout: 1
         - stderr: 2
         __Logs of containerized processes__ are stored inside __/dev/stdout or /dev/stderr__ (which can be accessed by commands of some command line tools like docker or kubectl or podman).\
         For example, to view the logs of a container named ngix
         ~~~~
         docker logs nginx -f # Option -f to stream the logs in real time
         kubectl logs nginx # Pod nginx with only one container
         
         kubectl logs -p -c ruby web-1 # Return snapshot of previous terminated ruby container logs from pod web-1
         kubectl logs --tail=20 nginx # Display only the most recent 20 lines of output in pod nginx
         kubectl logs --since=1h nginx # Show all logs from pod nginx written in the last hour
         ~~~~
         
         To Manage the huge amount of data, these logs need to be shipped to a system that stores the logs. To __ship the logs__, different methods are below:
         - Node-level logging: An admin configures a log shipping tool to collect logs and ships them to a central store
         - Logging via sidecar container: Use a sidecar container to collect and ship the logs to a central store
         - Application-level logging: Configure the logging adapter in every app in the cluster and app can push the logs directly to the central store
       
         __Tools__: fluentd or filebeat can do first two methods. OpenSearch or Grafana Loki can store logs. 
         To make logs easy to process and searchable, use __structured logging__ (for ex, use structured format like JSON).
         
      2. __Metrics__: Quantitative measurements taken over time. For ex, num of requests or an error rate.
         __Prometheus__ (open source monitoring system)
         > It can collect metrics that were emitted by apps and servers as time series data

         Prometheus data model provides 4 core metrics:
         - Counter: Increasing value, ex. request or error count
         - Gauge: Increasing or decreasing value, ex. memory size
         - Histogram: A sample of observations, ex. request duration or response size
         - Summary: Extension of histogram, also provides total count of observations

         Query lanugage used by Prometheus is __PromQL__ (Prometheus Query Language)\
         __Ecosystem tools__\
           __Grafana__ can be used to build dashboards from the collected metrics\
           __Alertmanager__ Alert monitoring and notification 
      
      3. __Traces__: Progression of a request while it's passing thru the system. Are used in a distributed system to provide info about like start, finish time, name, tags or a log message. 
         It describes the tracking of a request while it passes thru the services. \
         Traces can be stored and analyzed in a tracing system like Jaeger.\
         __OpenTelemetry__\
         = OpenTracing + OpenCensus
         > = a set of app programming interfaces(APIs), software development kits(SDKs) and tools that can be used to integrate telemetry such as metrics, protocols, but especially traces into apps and infrastructures. \
         > The OpenTelemetry clients can be used to export telemetry data in a standarized format to central platforms like Jaeger. 
     
    
  * Cost Management
    Cloud computing allow us to draw from a "infinite" pool of resources and only pay for them when needed (on-demand pricing models). Cost optimization is important. Some ways to do the optimization:
    - Identify wasted and unused resources (manually find or auto find like autoscaling)   
    - Right-Sizing
    - Reserved Instances (Reserve resources pricing method is better if you have good estimate about needed resources for years)
    - Spot Instances (For batch job or heavy load for a short amount of time. Idea of spot instances is that you get unused resources that have been over-provisioned by the cloud vendor for very low prices, con is that the resources are not reserved and may be taken by others paying "full price".

## Other Knowledge 

### Deployment Strategy
  * Recreate: Terminate the old version and release the new one. With downtime, no rollback
  * Blue/Green: when you deploy an exact duuplicate of your workload. You can easily switch back and forth between the Blue and Green environemnt and terminate the old one when you are happy with the state of the new environment.
  * A/B Testing: release a new version to a subset of users in a precise way (HTTP headers, cookie, weight, etc.). This doesn't come out of the box with Kubernetes, it imply extra work to setup a smarter loadbalancing system.
  * Canary: Canary is when you deploy your changes and only roll it out for a subset of your users. If there are no reported problems then all users are shifted over to the new environment and the old environment is terminated. Rollback is slow.


<img width="802" alt="image" src="https://user-images.githubusercontent.com/24993672/213583195-5bd09e49-3ccf-445d-89e3-ae63b64a2d92.png">

  Reference: \
  https://github.com/ContainerSolutions/k8s-deployment-strategies \
  https://blog.container-solutions.com/kubernetes-deployment-strategies#kubernetes-blue-green
  
### Distinguishment

  #### Package, deploy and manage a Kubernetes application?
  - Helm Charts
  - Kubernetes Operator
  - Wrong Choices:
    - Terraform: it is IaC, intended for deploying infrastructure not applications

  #### CNCF project for CI/CD with both CLI and Web UI
  - Argo
  - Wrong Choices:
    - Flux: reply completely on CLI
    - Jenkins X: not CNCF
    - Harness: not CNCF, paid service

  #### Knowledges:
  - Lightweight Kubernetes distribution for IoT and Edge Computing: K3s
  - Gurantee of pods available on every node: __DaemonSet__ will ensure that one copy of a pod defined in our configuration will always be available on every worker node
  - __Container Runtime Interface__ allows you to run enable kubelet to use a wide variety of container runtimes, without having a need to recompile the cluster components
  - Owners mechanism cleans up related resources before deleting an object.
  - kubectl api-versions can list the available API groups and their versions
  - The kubectl “api-resources” is a command to view the available resource types as well as the API group they are associated with
  - Field selectors let you select Kubernetes resources based on the value of one or more resource fields
  - LXC is a well-known Linux container runtime that consists of tools, templates, and library and language bindings. It's pretty low level, very flexible and covers just about every containment feature supported by the upstream kernel
  - TTL-after-finished Controller prevents a job from living after a certain amount of time when it has finished execution.
  - Selectors fetch a group of objects based on a key/value pair in Kubernetes
  - two areas of concern for securing Kubernetes: 
    1. Securing the cluster components that are configurable 
    2. Securing the applications which run in the cluster
  - RBAC API declares four kinds of Kubernetes objects: Role, ClusterRole, RoleBinding and ClusterRoleBinding
  - EndpointSlices provide a simple way to track network endpoints within a Kubernetes cluster. They offer a more scalable and extensible alternative to Endpoints 
  - When managing the scale of a group of replicas using the HorizontalPodAutoscaler, it is possible that the number of replicas keep fluctuating frequently due to the dynamic nature of the metrics evaluated. This is sometimes referred to as __thrashing, or flapping__. 
  - As part of Cluster Autoscaler graduation to GA we want to guarantee a certain level of scalability limits that Cluster Autoscaler supports. We declare that Cluster Autoscaler scales to 1000 nodes with 30 pods per node.
  - Names of three CNCF landscapes: Cloud-native landscape, Serverless landscape, Member landscape
  - Fission is a fast serverless framework for Kubernetes with a focus on developer productivity and high performance
  - <img width="934" alt="image" src="https://user-images.githubusercontent.com/24993672/213595572-58eba840-6abd-4291-a7dc-4bdad1b6e97e.png">

#### Cloud Native Trail Map
<img width="507" alt="image" src="https://user-images.githubusercontent.com/24993672/213596942-46fdc308-4686-448c-b4c4-b55c327c5169.png">

#### Kubernetes Components Overview 
Cluster: A logical grouping of all components within a cluster.

Namespace: A named logical grouping of Kubernetes components within a cluster. Used to Isolate different workloads on the same cluster

Node: A virtual machine or underlying server. There are two types of nodes: Control Plane and Worker nodes. Worker nodes are where your application or workloads run. Control Plane nodes manage worker nodes.

Pod: The smallest unit in K8s. It is an abstraction over a container. Generally defines an application workload

Service: A static IP address and DNS name for a set of pods (persists an address even if a pod dies) and a load balancer. A “service” can also mean a container that continuously runs.

Ingress: Translates HTTP/S rules to point to services

API Server: The API Server allows users to interact with K8s components using the KubeCTL or by sending HTTP requests.

Kubelet: Kubelet is an agent installed on all nodes. Kublelet allows users to interact with nodes via the API Server and KubeCTL

KubeCTL: A command-line interface (CLI) that allows users to interact with the cluster and components via the API Server

Cloud Controller Manager: Allows you to link a Cloud Service Provider (CSP) eg. AWS, Azure GCP to leverage cloud services.

Controller Manager: A control loop that watches the state of the cluster and will change the current state back to the desired state.

Scheduler: Determines where to place pods on nodes. Places them in a scheduling a queue

Kube Proxy: An application on worker nodes that provides routing and filtering rules for ingress (incoming) traffic to pods.

Network Policy: Acts as a virtual firewall as the namespace-level or pod-level

ConfigMap: allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable. Used to store non-confidential data in key-value pair

Secret: small amount of sensitive data such as a password, a token, or a key

Volumes: mounting storage eg. locally on the node, or remote to cloud storage

StatefulSet: provides guarantees about the ordering and uniqueness of these Pods. Think of databases where you have to determine read and write order or limit the amount of containers
StatefulSets are hard, when you can host your db externally from K8s cluster

ReplicaSets: Maintain a stable set of replica pods running at a given time. Can provide a guarantee of availability.

Deployment: Is a blueprint for a pod (think Launch Template)

#### Controle Plane and Worker Nodes
<img width="1796" alt="image" src="https://user-images.githubusercontent.com/24993672/213618435-dc0bb058-6f5e-42ad-87c2-dfb4966269d6.png">


<img width="754" alt="image" src="https://user-images.githubusercontent.com/24993672/213619231-34c8682b-4227-4682-a4a3-f2e63cd0ed45.png">

__Deployment__

<img width="1260" alt="image" src="https://user-images.githubusercontent.com/24993672/213620163-a7b9b743-10cf-456d-80a3-1f10688425e9.png">

__Namespaces__
<img width="1546" alt="image" src="https://user-images.githubusercontent.com/24993672/213627772-364537a3-0010-4378-bb61-092a75efb548.png">

<img width="1696" alt="image" src="https://user-images.githubusercontent.com/24993672/213628192-c69f1dad-bbe8-4842-840d-346150ca934e.png">

__Endpoints__

<img width="750" alt="image" src="https://user-images.githubusercontent.com/24993672/213628734-1016285d-2f61-4b58-b4e6-b40fc2bcaab1.png">

__Selector__

<img width="1010" alt="image" src="https://user-images.githubusercontent.com/24993672/213629397-bede252a-cbda-4c3b-b423-3de88e823e71.png">

<img width="750" alt="image" src="https://user-images.githubusercontent.com/24993672/213628734-1016285d-2f61-4b58-b4e6-b40fc2bcaab1.png">

Recommended Labels:

<img width="947" alt="image" src="https://user-images.githubusercontent.com/24993672/213629969-d02063fa-5dd9-4b5a-a203-ea58237cd86c.png">

__gRPC__

<img width="1782" alt="image" src="https://user-images.githubusercontent.com/24993672/213630846-9274d6e5-13bc-4272-a9a1-4ae87e8f428c.png">
