# Cloud Native Architecture Fundamentals

## Characteristics of Cloud Native Applications

- Resiliency
- Agility
- Operability
- Observability

**Anagram RAOO**
- Raccoons
- Are
- Often
- Observant

## Cloud Native Practices

- Self Healing
  - Deployment
    - Declarative means to define desired state
    - Specify how an application shoould run as well as the number of replicas that should be running
  - ReplicaSet
    - Managed by deployments
    - Declarative resource for managing the desired number of pods
  - Pod
    - Smallest deployable unit you can create and manage in Kubernetes
    - Provides shared Networking and storage for one or more containers
    - Pod similar to isolated host

- Application Automation 
  - Speed and Agility
    - Rapid Infrastructure and Application Deployment 
      - No manual steps
    - Frequent updates
  - CI/CD
    - Continous Integration/Continous Delivery
      - Code
      - Commit
      - Pipeline
        - Build
        - Unit Tests
        - Integration Tests
    - Continous Integration/Continous Deployment
      - Code 
      - Commit
      - Pipeline
        - Build
        - Unit Tests
        - Integration Tests
        - Automation
        - Release
    - CI/CD in KCNA exam is Continous Integration/Continous Delivery
  - Solutions
    - Ansible
    - KubeSpray
    - Terraform - IaC

- Secure by Default
  - Zero Trust
    - Never trust, always verify
    - Mutual authentication to verify identity and integrity
  - Secure Channels
    - Use secure channels to connect components
  - Least Privilege

- Speed, Efficiency and Cost
  - Autoscaling
  - Solutions
    - Knative
      - Serverless solution for Kubernetes

- Service Discovery
  - Automatic detection of services on a network
  - Minimize manual configuration
  - Leverage cloud native tooling
    - Kubernetes uses enviornmental variables and DNS for service discovery

## Pillars of Cloud Native Architecture

- Microservices Architecture
- Containerization
- DevOps
- Continous Deliver (CD)
- Morning Coffee Delivers Caffeine Delight

## Autoscaling

- Pattern for automatically scaling application components/infrastructure based on metrics or other requirements
- Automation is critical
- Testing
- Metrics
  - CPU and Memory
- Reactive Autoscaling
  - Scaling that occurs as a reaction to specific metrics, etc
- Scheduled Autoscaling
  - Scheduling autoscaling ahead of time
  - End of month batch processing, etc
- Vertical Autoscaling
  - Scaling up
  - Change of underlying infrastrucure.. more RAM, CPU, etc
- Horizontal Scaling
  - Scaling out
  - Addition or removal of resources in relation to existing resources
- Cluster Autoscaler
  - Tool that automatically adjusts the size of a Kubernetes cluster when pods fail to run or nodes that have been undertilized and pods can be placed on existing nodes
- Pod Autoscaler
  - Vertical Pod Autoscaler (VPA)
    - Scales the reource requests and limits of a pod
  - Horizontal Pod Autoscaler (HPA)
    - Scales the number of replicas of an application
- Keda
  - Event Driven Autoscaling
    - Uses ScaledObject definition to define how an application should scale and what are the related triggers
    - Keda can scale a deployment to zero

## Serverless

- Event driven architecture
- Autoscaling is a core component
- Can scale to zero
- AWS Lambda
  - FaaS - Functions as a Service
- Cloud Native Serverless
  - Knative
  - OpenFaaS
  - Runs on top of Kubernetes
  - CloudEvents Specification
    - cloudevents
    - Describe event data in common formats
    - Hosted by CNCF
    - SDK for most common languages
    - Specifications covering AMQP, HTTP, Kafka, NATS, JSON, etc.

## Community and Governance

- Cloud Native Computing Foundation (CNCF)
- Mission 
  - Make cloud native ubiquitous
- Projects
  - Sandbox
  - Incubating
  - Graduated
- Elections and Voting
- TOC
  - Technial Oversight Committee
  - Provides technical leadership to the cloud native community
- SIG
  - Special Interest Group
  - Open groups that focus on a specific area of a project
  - Anyone can join
- TAG
  - Technical Advisory Groups
  - Provide technical guidance across specific domains
  - Guide and support new projects
  - Support and review CNCF projects transitioning from sandbox to incubation and beyond

## Cloud Native Personas

- DevOps Engineer
  - Mix of development and operations skills/experience
- Site Reliability Engineer (SRE)
  - Focused on availability, scalability and robustness of systems
  - SLA, SLO, SLI
- CloudOps Engineer
  - Management and delivery of cloud workloads
- Security Engineer
  - Broad range of knowledege in IT security
- DevSecOps Engineer
  - DevOps with Security knowledge
- Full Stack Developer
  - Developer
- Cloud Architect
- Data Engineer
- FinOps Engineer
- Machine Learning Engineer
- Data Scientist

## Open Standards

- Docker
  - Defacto container technology

- Open Container Intitiative (OCI)
  - Under control of Linux Foundation
  - Launched by Docker and CoreOS
  - Open standards for container runtimes, images, and distros
  
  - Image specification (image-spec)
    - How to bundle a filesystem into an image
    - Tools
      - Docker
      - Buildkit
      - Podman
      - Buildah

  - Runtime Specification (runtime-spec)
    - runC was donated by Docker
    - runC is Docker's underlying runtime software
    - runC is the reference implemenation of an OCI runtime
    - Outlines how a filesystem bundle should be packaged into an image
    - Other OCI compliant runtimes include
      - Kata Containers
      - gVisor
      - Firecracker
      
  - Distribution Specification (distribution-spec)
    - Open standards and API protocols for distribution of content
    - Built on Docker Registry HTTP API V2 Protocol

- Container Network Interface (CNI)
  - Required by Kubernetes
  - During initial setup, nodes transistion from `Not Ready` to `Ready` post CNI installation
  - CNI setup is hidden, but can be overriden

- Container Storage Interface (CSI)
  - Open standard for interfacing with storage solutions
  - Solutions
    - Rook
    - portworx

- Container Runtime Interface (CRI)
  - Open standard typically used by kubelet to interact with a container runtime engine
  - CRI is a plugin that allows kubelet to use a variety of container runtimes
  - There may be independent CRI plugins for each container runtime

- Service Mesh Interface (SMI)


























