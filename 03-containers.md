# Containers with Docker

## Introduction to Containers
- Mainframe Virtualization
  - CP/CMS 
  - Mainframe operating system in the late 1960s/early 1970s
  - Time Sharing
  - Virtual Machine
  - Earliest reference to use of a virtual machine

- Unix Chroot
  - Change root
  - Change root directory for a process and its children
  - Restricted access
    - Process and children could only see the files and directories in the new root directory
  - Limited
    - Hostname, IP Addresses are still visible and accessible
    - Cannot run superuser processes
    - Directories must be root:root owned
  - Use Cases
    - SSH Shells with access to Chroot enviornment
    - Applications like Apache running via Chroot
    - User Isolation

- FreeBSD Jails
  - Lightweight virtualization technology
  - Open source operating system based on the BSD version of Unix
  - Widely used by internet service providers in the 2000s
  - Jails
    - Introduced in FreeBSD 4.0
    - Allowed partioning of FreeBSD into isolated environments with their own set of users, process, file systems, and networking stack
  - Complexity
    - FreeBSD Jails was difficult to use and manage
    - Didn't gain widespread adoption

- Sun Solaris
  - Provided virtualization technology called Zones
  - Allowed the Solaris operating system to be divided into multiple isolated enviornments

- HP-UX
  - Provided virtualization technology called Virtual Partitiions (VPs or VPARs)
  - Segmented system into logical partitions for virtualization

- Linux
  - Namespaces
    - User
      - Isolated user ID space allowing processes within a namespace to have their own unique set of ids seperate from the rest of the system
    - pid
      - Allows processes in a namespace to have their own unique set of process IDs without conflicting with each other
    - network
      - Processes within a namespace have their own networking stack allowing communication between processess in the same namespace
    - mount
      - Independent filesystem hierarchy providing processes with an independent view of the file system
      - Able to mount and unmount filesystems in a namespace
    - uts
      - Unix Time Sharing for isolating hostname and domain names
    - ipc
      - Inter-process communication namespace
      - Similar to independent posix message queues
    - Cgroups
      - Adds the ability to isolate resource units
      - Development by Google started in 2006
      - Originally called process containers then changed to Cgroups
      - Released in 2007, merged into the Linux kernal in 2008
    - Resource Limits
      - How much ofa a resource (CPU/Memory) can be used
    - Prioritization
      - Priority each resource has
      - Related to resource limits
    - Accounting
      - Monitoring and reporting of resource limits
    - Control
      - Abiltiy to control processes within a cgroup
        - start, stop, frozen, restarted

- Virtual Machines
  - VMware
  - Hypervisor

- Containers with Docker
  - Docker started in 2010 as dotCloud
  - Renamed to Docker and open sourced in 2013
  - Shared Kernal
    - Container runtime makes use of a shared Linux kernal between containers
  - User Namespaces and Cgroups
    - Utilizes Linux kernal technologies to provided isolated environments with their own users, hostnames, IP addresses, mount points and resource allocations

## Docker Desktop Installation and Configuration

- Traditional Docker
  - Installed on a Linux system
  - Uses Containerd and Runc to to run containers
  - Use CLI to run an build containers

- Docker Desktop
  - Uses a hiden VM or subsystem to run an isolated instance of Docker
  - Depending on the host operating system, uses different virtualization technologies
    - Windows - HyperV/WSL
    - Mac - Hyperkit/QEMU
    - Linux - KVM/QEMU
  - Uses hidden VM to configure a Linux operating system with the Docker runtime

## Container Images

- What is a container image
  - A portable, self contained bundle of sofware and dependencies
  - Allows us to run software consistently across compute enviornments
  - Sometimes refered to as a Docker image
    - Should be: OCI Compliant Container Image
    - OCI Compliant Container Images can be used anywhere that support OCI container images
      - Docker
      - Kubernetes
      - AWS ECS

- Container Image vs Container
  - Container Image
    - Bundle of Software
  - Container
    - Instance of the software that is being run
  - Tag
    - Label used to distinguish the a version of the container image
    - Tags are flexible and can be used to satisfy your container image requirements
    - `latest` tag
      - Default tag used if tag is not specified
      - Doesn't always apply to the latest version of a container image.. you may hve new images with different tags

- Container Layers
  - Container image is composed of multiple layers stacked on top of each other
  - Each layer represents a modification to the filesystem inside the container
  - Each layer is immutable
  - Thin accessible writeable layer on the top
  - Only layer that changes is the thin writable layer
  - Union Filesystem
    - Merges each layer into a single view
  - Digest
    - Hash of the images content, layers and metadata using sha256
    - Verify image integrety
    - Should match digest in Docker hub
    - Can pull image using digest if you want to be specific
      - `docker pull docker.io/wernight/funbox@sha256.... digest here`
    - Can add digest to `docker images` command
      - `docker images --digests`
  - Can inspect layers using `docker manifest`
    - `docker manifest inspect wernight/funbox`

- Running Containers
  - `docker version`
    - Version of Docker client and server
    - Docker desktop runs both the client and the server
    - containerd
      - High level container runtime
    - runc
      - Low level container runtime
  - `docker --help` or `docker --help | more`
    - See all Docker commands
    - Add help to specific commands
      - `docker run --help | more`
  - `docker ps` or `docker ps -a`
    - List currently running containers or all containers (`-a`)




 



