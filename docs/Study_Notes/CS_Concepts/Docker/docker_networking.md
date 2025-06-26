---
id: sw_essentials
aliases:
  - Docker Networking
tags:
---

# Docker Networking

Docker networking refers to the system and tools provided by Docker to facilitate communication and connectivity between containers, as well as between containers and the external world (such as the host machine, other containers, or external networks). Docker’s networking capabilities enable containers to communicate with each other and with external services while maintaining isolation and security.

## Types of Docker Networking

Docker offers several networking options to meet different requirements:

1. **Bridge Network**: By default, Docker creates a bridge network on the host machine. Each container is connected to this bridge network by default. Containers on the same bridge network can communicate with each other using IP addresses and they can be accessed from the host machine as well. This network provides isolation and basic networking functionality.
2. **Host Network**: Containers can share the network namespace with the host machine, allowing them to use the host’s network stack directly. This option can provide better network performance but may compromise isolation to some extent.
3. **Overlay Network**: Docker supports overlay networks, which are used for connecting containers running on different Docker hosts. This is particularly useful in distributed and clustered setups, such as when managing containers using Docker Swarm or Kubernetes.
4. **Macvlan Network**: Macvlan allows containers to have their own MAC addresses, making them appear as separate physical devices on the network. This can be useful for scenarios where containers need to be directly accessible on the local network.
5. **Custom Bridge Network**: Users can create their own custom bridge networks with specific configurations, allowing more control over container communication and connectivity.
6. **None Network**: Containers can also be launched without network access, which can be useful for certain isolation scenarios.
7. **Port Mapping**: Docker provides port mapping (port forwarding) to allow containers to expose and receive traffic on specific ports. This is essential for accessing containerized services from the host or external networks.

Docker networking is a crucial aspect of containerization because it allows developers to build and deploy applications in isolated environments while ensuring they can communicate with each other and the outside world. Properly configuring Docker networking is important for achieving optimal performance, security, and scalability in containerized applications.

## Basics

Try running the command:

```bash
sudo docker network ls
```

This lists all the networks the Engine daemon knows about. This includes the networks that span across multiple hosts in a cluster. This will output:

```bash
root@localhost:~# sudo docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
2d2ed734e75a   bridge    bridge    local
daf8213c0a1d   host      host      local
abfb7773f8c2   none      null      local
```

Here: - `DRIVER` is equivalent to the Network Type we have in our system.
