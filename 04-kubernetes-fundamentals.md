# Kubernetes Fundamentals

## Container Orchestartion Introduction

- What is container orchestration
  - Automating the operational needs of running containers

- Can can a container orchestrator help with?
  - Provisioning
  - Deployment
  - Scaling
  - Standards and Frameworks
  - Integrations with core components
  - Etc..

- Where does container orchestration excel
  - Provisioning and deployment of containers
  - Container availablity and self healing
  - Scheduling and effective use of compute resources
  - Exposing container services
  - Authorization and Security
  - Storage for shared and persistent workloads
  - Autoscaling
  - Extended functionality (Custom Resource Definition - CRD)
    - Method to expand Kubernetes functionality outside the core functionality

## Kubernetes Architecture

  - Low Level Container Runtime (runc)
    - Runs on both Control Plane and Node
    - Spawning and running containers
    - Interacts with low level linux components - Namespaces and Cgroups
    - runc is the reference implementation of a (low level) container runtime
    - runc was donated by Docker Inc and is an OCI compatible container runtime
    - Other OCI low level runtimes include crun, kata-runtime and gVisor
    - Low level runtimes are typically installed as a component of a high level container runtime/container engine

- High Level Container Runtime (containerd)
  - Runs on both Control Plane and Node
  - Created by and used within Docker
  - Donated to the CNCF and is a graduated project
  - Operates at a higher level and manages the entire container lifecycle
  - Pulls container images and stores them
  - Interacts with low level container runtimes (runc)
  - An apt/yum install of conainerd will install a low level container runtime (runc)

- Kubelet
  - Runs on both the control plane and the node
  - Often referenced as being the "Node" of the Kubernetes cluster
  - Maintains Pods
    - Pods are the smallest unit of compute in Kubernetes
    - Pods run one or more containers in Kubernetes
  - Makes use of a Pod Spec
    - Description of a Pod in YAML or JSON
  - Can receive requests via an API or by monitoring a directory - typically /etc/kubernetes/manifests

- Static Pods
  - Runs on control plane
  - Files in manifests directory -> Kubelet -> containerd -> runc -> Static Pods
  - Static pods are created and managed based on files in the manifests directory
  - Components that run as Static Pods
    - etcd
      - Strongly consistent, distributed key-value store
      - Handles Leader Elections, Network Partitions
      - Handles machine failures in a highly available configuration
      - Used as a source of truth - Backing store for Kubernetes
      - Production setup:
        - Multiple instances of etcd with an odd number of members
        - Ideally a five member cluster
        - etcd backups should be part of a production implementation
    - kube-apiserver
      - Main gateway for access
        - User access and component communication
        - Provides a RESTful API interface
        - Stores all data in a persistent storage backend i.e. ETCT
        - Communicates with the Kubelet via API - another route for the Kubelet to create pods
    - kube-scheduler
      - Control plan process
      - Started as a Static Pod (via /etc/kubernetes/manifests)
      - Determines valid notdes according to constraints and resources
      - Chooses most appropiate node based on workload
    - kube-proxy (k-proxy)
      - Runs as a DaemonSet on every instance of a cluster
        - A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. 
      - Started as a normal pod (via Kubernetes).. it is not a static pod
      - Communicates with the API server and dynamically configures TCP/UDP and SCTP forwarding on any system that it runs on
    - CoreDNS
      - Flexible and Extensible DNS server
      - CoreDNS is a Kubernetes deployment
        - Deployments are used to describe how an application should be run
          - For example: a deployment will have X replicas (instances)
      - Deployments such as CoreDNS are dependent on Kubernetes controllers
    - Controller-Manager (c-m)
      - Controllers are Control Loops that monitor the state of your cluster
      - Controller will make or request changes as required
      - Controllers include:
        - Replication Controller: Controls # of replicas
        - Node Controller: Checks if nodes are online or offline
        - Deployment Controller: Manages deployments
    - Cloud-Controller-Manager (c-c-m)
      - Not available on all Kubernetes setups - typically found in public cloud Kubernetes offerings
      - Bridges the functionality of the cloud provider to Kubernetes
      - A LoadBalancer service is a good example of CCM in use
        - The user of LoadBalancer in Kubernetes may result in the creation of a Cloud Provided LoadBalancer

- RAFT
  - Distributed systems protocol
    - R: Reliable
    - A: Asynchronous
    - FT: Fault Tolerant

## Pods

- Smallest unit of compute in Kubernetes
- Hosts one or more containers
- Containers share networking provided by the pod and will connecommunicate within the pod using the localhost
- Each pod is assigned a unique IP address within the cluster
- Containers in a Pod can communicate with each other using Inter-Process Communication (IPC)
  - Great for running an entire application in a pod
- A pod can encapsulate an application, its dependancies, shared storage and networking into a single deployable unit
- Run/create a pod
  - `kubectl run <podname> --image<imagename>`
- Get pod logs
  - `kubectl logs pod/<podname>`
  - `kubectl logs <podname`>
- If user is connected to the Kubernetes cluster, pods are accessible to the user
- Pods are not accessible for the user if the user is on a local network and kubectl is connected to a remote kubernetes cluster
- Pods can be created using YAML
- Use `kubectl explain` to get help
- Imperarative Commands
  - `create`, `replace`, or `delete`
  - Provide a way to interact with the cluser by directly instructing it to perform specifc operation
  - These commands focus on the desired outcome vs the underlying configuration or declarative state
  - Imparative commands my result in configuration drift if not carefully managed
    - Desired state is not explicity defined or tracked
- Declarative Approach
  - `apply`
  - Declarative commands are the preferred approach for managing a clusters state ad resources
  - Define the desired state using YAML or JSON files
  - When applying these files, Kubernetes compares the desired state with the current state of the cluster
- Sidecar
  - Container on the side
  - Carries out a specific task within a pod

## Namespaces

 - Namespaces are a fundamental element of the Kubernetes architecture
 - Namespaces divide cluster resources between multiple users, applications or projects
 - Town example
   - Kubernetes is a town
   - Namesoace is house in the town
   - Pods are rooms in the house
 - Benefits
   - Isolation
   - Resource management
     - ResourceQuotas
       - Allows us to set budgets on how much of a resources (memory, CPU) can be used by all pods in a namespace
     - LimitRanges
       - Allow us to set the amount of resources that each pod or container can use
     - Security and access control
       - RBAC integrated with a specific namespace - allow only specific users/groups to access resources in that namespace
     - Organization and Simplifiacation 
       - Namespaces make it easy to manage and organize resources in a large Kubernetes cluster
       - Essential for working with Kubernetes in a large organization
- `kube-system` is a default namespace for objects created by the Kubernetes system
- `kube-node-lease` holds lease objects associaetd with individual Kubernetes nodes - send heartbeats to control plane to detect node failure
- `kube-public` publicaly visible resources
- `default`
- You can create your own namespaces

## Deployments and ReplicaSets

- Update applicationss predictably
- Maintain availability even during an update process
- Deployments provide key features
  - Replication - Should a pod crash or get deleted, the Deployment makes sure the number of pods is as expected
  - Updates - Updates are phased out gradually to prevent all instances from being updated simultaneously
  - Rollbacks - If anything goes wrong during the update or while the a new version is rurrning, Deployments allow you to revert to an older version
- Deployments automatically create a ReplicaSet
- `MaxSurge` 
  - Allows Kubernetes to increase the number of pods above the desired amount during an update, maintaining application availability
- `MaxUnavailable`
  - Sets the % of pds that can be unavailable during the update process, ensuring availabilty while limiting resource usage

## Services

- Kubernetes Services are used to expose an application deployed on a set of pods using a single endpoint.
- Services were introduced to provide reliable networking by bringing stable IP addresses and DNS names to pods

- ClusterIP
  - Default service in Kubernetes
  - Not accessible outside of the cluster
   -Establishes an internal IP address only available within the cluster

- NodePort
  - Technically available outside the cluster if node IP addresses are externally accessible

- LoadBalancer
  - Creates external load balancer for public access (cloud only)

- ExternalName
  - Maps service to an external DNS name, no proxying

- Headless
  - No cluster IP, exposes individual pod IPs

## Jobs

- Supervisors for pods - carries out batch tasks
- Manages task execution
- Tracks task progress
- Retries tasks as required
- Data processing task, system backups, multiple parallel tasks with desired # of completions
- `kubectl create job <jobname> --image<imagenname>`
- `watch kubectl get jobs` - list jobs
- `kubectl describe job/<jobname>`
- Completions
  - Specifies the desired number of successfully finished pods the job should be run with
  - Setting to null means that the success of any pod signals the success of all pods and allows parallism to have a positive value
- Parallelism
  - Specifies the maximum number of pds the job should run at any given time
  - Actual number of pods running in steady state will be less than this number
- Using completions and parallism you could create 20 pods to complete a task with only 5 running at one time
- CronJobs 
  - Time based schedulers
  - Use the Traditional Unix CronJob scheduling system
  - Run tasks on a schedule
  - Create Job Objects as per a schedule
  - crontab guru for specific syntax
  - `kubectl create cronjob <jobname>`
  - `successfulJobsHistoryLimit`
    - Number of completed and failed jobs that should be kept
    - Defaults to 3

## ConfigMaps

- Manage configuration data
- Store Configuration Information
- Store Environmental Variables
- `kubectl create configmap <configmap name> --from-literal=KEY=VALUE`

# Secrets
 
- Object used for storing sensitive information
- Use for passwords, tokens, and keys
- Use for anything sensitive you want to store
- You don't need to store confidential information in your application
- `kubectl create secret generic <secretname> --from-literal=KEY=VALUE`
- Base64 Encoded
- Encoded, not Encrypted
- Generic secrets are assigned `Opaque` type

## Labels

- Identifitying and organizing resources
- Effective mechanism for assigning metadata to a Kubernetes object
- Useful for tagging resources
- Great for grouping resources - App 1, Team 1
- Used for service discovery, load balancing and network policies
- Used for scoping resources - Dev, Test, Prod
- Service can use a label to identify pod it should route traffice to









