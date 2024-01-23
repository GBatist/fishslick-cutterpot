# Theory

## Summary

- [Theory](#theory)
  - [Summary](#summary)
  - [What is a Container](#what-is-a-container)
    - [Resource Isolation](#resource-isolation)
    - [Comparison with Virtual Machines](#comparison-with-virtual-machines)
  - [What is Docker](#what-is-docker)
  - [What is a Container Image](#what-is-a-container-image)
    - [Key Points](#key-points)
  - [Crafting a Container Image](#crafting-a-container-image)
    - [Visualizing Layers](#visualizing-layers)
    - [Delving into Image Details](#delving-into-image-details)
      - [Key Points](#key-points-1)
  - [Namespaces and Cgroups: Pillars of Container Isolation](#namespaces-and-cgroups-pillars-of-container-isolation)
    - [Key Points](#key-points-2)
  - [Orchestrating Containers with Docker Compose](#orchestrating-containers-with-docker-compose)
    - [Beyond the Dockerfile](#beyond-the-dockerfile)
    - [The Heart of Composition: compose.yaml](#the-heart-of-composition-composeyaml)
    - [Key Principles](#key-principles)

---

## What is a Container

Containers are a **lightweight virtualization technology** that allows you to run applications in isolated environments. This isolation provides a number of benefits, including:

- **Security**: Containers can be used to isolate applications from each other, which can help to prevent security breaches.
- **Performance**: Containers can be more efficient than virtual machines, as they share the underlying operating system.
- **Portability**: Containers can be easily moved between different environments.

### Resource Isolation

Containers isolate resources such as CPU, memory, processes, network, and file system. This isolation is achieved by using a container runtime. A container runtime is a piece of software that manages the lifecycle of containers.

### Comparison with Virtual Machines

Virtual machines (VMs) are another virtualization technology that can be used to run applications in isolated environments. However, VMs are more heavyweight than containers. This is because VMs require a full operating system to run.

The following image illustrates the **difference between containers and VMs**:

![vms vs container](/images/vms_containerized.png)

Unlike virtual machines (VMs), **containerized applications don't require resource duplication (overhead)**. This is because they leverage the host machine's resources directly, **eliminating the need for redundant operating systems and hardware** layers.

## What is Docker

In a quick definition **Docker is a containerization platform** that lets you create, deploy, and run applications in standardized and isolated containers. This provides benefits like portability, efficiency, and security for your applications.

## What is a Container Image

In the world of containers, a container image serves as **a blueprint or template for creating a running container**. It's like a snapshot of **everything an application needs to function**, but it's not yet a running application itself.

### Key Points

- **Blueprint for Containers**: A container image is the foundation upon which actual containers are launched.
- **Not a Running Application**: It's important to remember that an image is not yet a live, executable entity. It's the recipe, not the cake itself.
- **Contains Essential Ingredients**: Within a container image, you'll find:
  - The base image (like a starting point)
  - Application code
  - Necessary libraries and dependencies
  - Instructions for startup (e.g., commands like npm start)
  - Exposed TCP ports
  - Environment variables

## Crafting a Container Image

- **Dockerfile**: This specially formatted text file acts as a set of instructions for building a container image. Each line in the Dockerfile represents a layer in the final image.
- **Layers, Like Building Blocks**: Think of layers as incremental steps in constructing the image. Each layer adds or modifies something, leading to the complete blueprint.

### Visualizing Layers

![image layer representation](/images/docker_image.png)

Remember: Container images don't contain the operating system or hardware resources. They focus on the application's specific needs and dependencies, ensuring portability and efficiency across different environments.

### Delving into Image Details

Let's unpack the insights from the visual representation of container image layers:

- **Base Image**: Acts as the foundation for subsequent layers. In this example, Debian is the base image, but it doesn't mean an entire operating system is included. Instead, it signifies that we're leveraging the Debian kernel and its associated utilities (*like apt-get*) for our application.
- **Permissions and Layer Protection**: The "named layers" (Debian, Nginx install, copy App) are read-only. This immutability ensures consistency and prevents unintended changes from affecting other containers that might rely on the same layers.
- **From Image to Container**: When you launch a container from an image, an additional, "unnamed layer" is created. This layer is read-write and exists only during the container's lifetime. It's where temporary data, logs, and any modifications made within the container are stored. Once the container terminates, this layer vanishes, leaving the original image intact.
- **File Changes and Layer Duplication**: Any modifications to files within the container occur in the read-write layer, not the original image layers. This means that if a file is created during image build and later modified within a running container, a copy of the file is made in the read-write layer. This mechanism prevents unintended changes to the original image and allows containers to have their own unique, writable states. However, it can also lead to file duplication and potential increased storage usage, as you've can imagine.

#### Key Points

- Container images are carefully constructed with layers to ensure consistency, efficiency, and isolation.
- The base image provides a foundational starting point.
- Read-only layers protect shared components and enable efficient reuse.
- The read-write layer allows containers to have their own unique state without affecting the base image.
- Understanding these concepts is essential for effectively working with containers and optimizing their resource usage.

## Namespaces and Cgroups: Pillars of Container Isolation

Within the realm of containers, two Linux kernel features play crucial roles in enabling isolation and resource management: namespaces and cgroups. Let's explore their essence:

- **Namespaces**: Imagine each container as a self-contained universe, unaware of others sharing the same machine. Namespaces make this illusion possible. They partition system resources, creating isolated environments for processes. Common namespace types include:
  - Process namespace: Each container has its own unique process tree, independent of others.
  - Network namespace: Containers get their own virtual network stack, including interfaces and IP addresses.
  - User namespace: Isolates user IDs and permissions, preventing processes in one container from tampering with others.
  - Mount namespace: Controls the file system view, ensuring containers can only access their designated files.

- **Cgroups**: While namespaces provide isolation, cgroups govern resource usage. They enable fine-grained control over how much CPU, memory, disk I/O, and network bandwidth each container can consume. This prevents resource-hungry containers from overwhelming the system and ensures fair allocation among multiple containers.

### Key Points

- Namespaces create virtual spaces, while cgroups manage resources within those spaces.
- They work in tandem to ensure containers operate in isolated and resource-controlled environments.
- This foundation is essential for secure and efficient container orchestration.

## Orchestrating Containers with Docker Compose

### Beyond the Dockerfile

The Dockerfile helps create a container image's blueprint, but Docker Compose is a powerful tool for managing multiple containers. It simplifies creating and managing complicated applications that use many containers.

### The Heart of Composition: compose.yaml

This YAML file is the central configuration hub for Docker Compose. It describes the application's structure and how its containers interact with each other. This is where we will connect MySQL to our application to make sure everything works well and data stays safe.

### Key Principles

- **Separation of Concerns**: Compose.yaml champions a separation of application-specific configurations and environment-specific settings, promoting maintainability and adaptability.
- **Customization**: Here, we'll meticulously tailor the environment for our application's MySQL needs, ensuring optimal data storage and retrieval.
