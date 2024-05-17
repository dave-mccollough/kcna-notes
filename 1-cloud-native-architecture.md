# Cloud Native Architecture

- Best Practices
    - Application availability
    - Cost management
    - Efficiency
    - Reliability

- CNCF Mission
These techniques enable loosely coupled systems that are resilient, manageable, and observable. Combined with robust automation, they allow engineers to make high-impact changes frequently and predictably with minimal toil.


- Monolithic Architectures
    - Tightly coupled
    - Difficult to deploy and manage
    - A change to one area could impact other areas

- Microservices Architecture
    - Loosely coupled
    - Traffic routing is controlled by rules (Ingress Routing)
    - Each microservice typically has it's own networking and connectivity
        - Each microservice can use the same port

- Design cloud native applications with resilance in mind
    - Applications can fail at any time

- Self healing
    - Deployment
        - Declare desired state
            - How an applcation should be running
            - How many replicas should be running
        - ReplicaSet
            - Managed by deployments
            - Provide a declarative resource for managing the desired # of pods
        - Pods
            - Smallest deployable unit that can be created and managed using Kubernetes
            - Provides shared networking and storage for one or more containers
            - Pod = Isolated Host

        - If a running pod fails, kubernetes will automatically attempt to replace the failed pod

- Application Automation
    - Speed and agility
        - Rapid infrastructure and application deployment
        - Frequent updates

    - Infrastructure as Code

- CI/CD (Continuous Integration/Continuous Delivery)
    - Code is commited to Git
    - Pipeline(s) are triggered
        - Build
        - Unit Tests
        - Integration Tests
        - Automation
        - Release

    - Automated Rollback procedure

- Secure by default
    - Zero trust
        - Never trust, always verify
        - Mutual authentication

    - Secure channels for connectivity between components
    - Least privilege

- Speed/efficiency and Cost
    - Autoscalling
    - Serverless - Scale to zero

- Service Discovery
    - Automatic detection of services on a network
    - Minimize manual configuration requirements
    - Cloud native tooling for service discovery
        - Enviromental Variables and DNS
        



            