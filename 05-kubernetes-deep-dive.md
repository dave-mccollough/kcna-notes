# Kubernetes Deep Dive

## Kubernetes Api

- Primary interface for users and system components
- RESTful API
- There are may forms of interaction with the Kubernetes API
- Tools to interact with the Kubernetes API
  - kubectl
  - Helm
  - Client applications
  - Monitoring
    - Promethesus
  - Admission Controller
    - Enforce policies and modify resources
    - Enfore quotas, set defaults, validate configurations and perform other tasks
    - Namespace management
    - Accept or reject requests
- API Routing
  - Request arrival
    - HTTPS endpoint
    - Listneing on por 6443
  - Route Matching
    - Based on URL or method
      - POST, PUT, GET, DELETE
  - Authenication
    - API key or Token
  - Authorization 
    - Permissions allowed
  - Admissions Controller
  - Validation
  - Request Handling
  - Response Generation
  - Response Sending

- Extend the Kubernetes API using Custom Resource Definitions (CRD)

## RBAC - Role Based Access Control

- Regulate access to resources in the Kubernetes cluster
- Policy based access for users, groups and service accounts
- Only authorized users can access or modify resources
- Enhanced security for Kubernetes cluster

- `kubectl config view`
  - Some data may be omitted
- `kubectl config view --raw`
  - View ommitted data

- RBAC CA - Certificate Authority
  - Trusted entity used by our Kubernetes cluster for creating and verifying clusters
  - Core componenet of Kubernetes
  - KubeConfig file will reference the public key of the CA

- Kubernetes does not manage users and/or group accounts
  - Kubernetes expects certificates to be created and authorized by the certificate authority

- Users
  - Individuals or applications that interact with the Kubernetes cluster
  - Managed externally
  - Represented by strings

- Groups
  - Managed outside of Kubernetes cluster
  - Represents multiple users
  - Used to asscoiate certain users with certain permissions

- Service Accounts
  - Used by applications running inside the cluster
  - Managed by Kubernetes
  - Tied to a specific namespace/permissions are scopped to a specific namespace
  - Used to give an application running in a pod the necessary permissions to interact with the Kubernetes API

- ClusterRole
  - `cluster-admin`
  - Non namespaced resource (applies to the entire cluster)
  - Allows us to set permissions on resources across all namespaces

- ClusterRoleBinding
  - Scope: Cluster wide
  - Binds a ClusterRole to a Service Account and/or Users

- Role
  - Scoped to Namespace
  - Grants permissions to resources within a specific namespace

- RoleBinding
  - Scoped to Namespace
  - Binds specific users/groups/service accounts to a Role within a namespace

## Scheduler and nodeName

- Kube-Scheduler
  - Schedules Applications in the form of pods to run on various nodes

- Interaction and Processess
  - Pod is created - Kube-Scheduler will do this
  - Checks available resources and identifies the best node to run on
  - Pod Placement - Assigns a pod to a node
  - Kubelet running on a chosen node starts the pod

- Three sets of operations
  - Filtering
    - Finding nodes that meet the scheduleing requirements
    - If a pod requires more memory than a certain node has availabl, that node will be filtered out
  - Scoring 
    - The scheduler scores the individual nodes to find the most appropiate node
    - Node with the highest score is chosen to run the pod
  - Binding
    - After the Kube-scheduler has choosen the best node, the pod is bound to a node using the Binding Object
    - Pod is offically assigned to the node

- nodeName
  - The specific node that the pod will be scheduled on
- nodeSelector
 - Speciofies labels that must match a nodes labels for the pod to be scheduled on that node

## Kubernetes Storage

- Ephemeral Storage
  - Storage that does not survive across restarts
  - Used as required then discarded
  - Example: EmptyDir
    - Temporary Space
    - Fast Cache Area
      - Set emptyDir:medium to memory

- Persistent Storage
  - Survives Restarts
  - Survives Removal of the container
  - StorageClass
    - Defines the type and characteristics of storage provided by the cluster
    - Administrators can describe the classes of storage offered
  - PersistentVolume(PV)
    - Represents a piece of storage in a cluster that has been provisioned by an administrator or dynamically using StorageClass
    - Resource in the cluster that persists beyond pod lifecycles
  - PersistentVolumeClaim(PVC)
    - Request for storage by a user
    - Specifies the amount and characteristics of storage needed by a pod
    - Once bound to a persistent volume, the persistent volume claim can be used by a pod to access the storage

  - Manual
    - Create persistent volume (PV) and then persistent volume claim (PVC)
  - Dynamic
    - Create the persistent volume claim against the named storage class
    - This cretes the persistent volume for us

- Reclaim Policy
  - Retain
    - Data is retained until the volume is manually deleted

## Kubernetes StatefulSets

- Managing stateful applications in Kubernetes

- Provide stable network IDs for each pod
- StatefulSets are named sequentially starting from zero and prefixed with the StatefulSet name
- Workload API objects used to manage stateful applications
- Maintain a sticky identtity for each pod that is part of the StatefulSet
- Each Pod in a StatefulSet creates its own claim to the StorageClass Provider
- Stable/Unique Network Identifiers
  - Headless - ClusterIP Service with no IP Address
- Stable persistent storage
- Ordered Graceful Deployment and Scaling
- Ordered Automated Rolling Updates
  - `partition` defines the starting point for rolling updates

## Kubernetes Network Policies

- Networking is open in Kubernetes clusters
- Multi-tenancy - Apps should be isolated from each other
- Network policies can help
- Pods can be classified as isolated
- We can limit acces to other pods, namespaces or IP Blocks
- NetworkPolicy is effective after it is applied
- Container Network Interface (CNI) plugin is necessary for enforecement of NetworkPolicies

## Pod Disrutption Budgets

- Service Reliability during voluntary disruptions

- High Availability of applications during voluntary disruptions
- Pod Disruption Budget (PDB) protect during disruptions, Replicas ensure availability during normal operations 
- `kubectl cordon`
  - Command to make node unschedulable
- `kubectl drain`
  - Removes all pods from the node

## Kubernetes Security

- Kubernetes Security Contexts
  - Feature in Kubernetes that defines privilege an access control settings for Pods amd Containers
  - Allows you to specifiy the user ID, group ID and other security related settings for a container
- Admission Controller
  - Integral to Kubernetes Security
  - Act as gatekeepers controlling what pods can and cannot do
  - Enforce security policies
  - Reduce atack vectors
  - Modular approach to implement
- Essential Security Tools
  - Kyverno
    - Policy Engine
  - Falco
    - Identifies abnormal behavior and potential security threats
  - OpenID
  - Open Policy Agent
  - Kubescape
- Practices

- PodSecurityPolicies (PSP)
  - Cluster level restrictions of pods
  - Depricated in Kubernetes 1.21
  - Removed in Kubernetes 1.25

- 4Cs of cloud security
  - Cloud
  - Cluster
  - Container
  - Code
 
- `allowPrivilegeEscalation`
  - Set to false prevents privilige escalation in a container

## Helm and Helm Charts

- Streamlines the Kubernetes experience
- Simplifies the management of Kubernetes applications
- Helm Charts 
  - Packages of preconfigured Kubernetes resources
- Easier deployment, management, and updates of Kubernetes applications
- Helm is the apt/yum of Kubernetes application management
- `helm create <app name>`
- git is required to install Helm
- Use `helm package` command to package Helm Chart for distrobution

## Service Meshes

- Helper for Microservices
- Used for managing microservices applications 
- Communicate efficiently, reliably and securely
- Useful as scale and complexity of application grows
- Data Plane
  - Typically uses a sidecar pattern
- Control Plane
  - Sits above and acts as a management hub

- Benefits
  - Mutual TLS
  - Access Control Policies
  - Improved Observability with Tracing
  - Monitoring Tools
  - Increased reliability with rate limiting and circuit breaking

- Cloud Native Philosophies
  - Architecture
  - Community and Governance
  - Personas
  - Open Standards








