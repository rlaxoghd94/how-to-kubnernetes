# Overview

## What is Kubneretes and what it can do

Kubernetes is an container orchestration tool you can run distributed systems resiliently; taking care of scaling and failover for your application, provides deployment patterns, and more.

Kubernetes provides you with:

- **Service discovery and load balancing**: Kubernetes can expose a container using the DNS name or using their own IP address. If a traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- **Storage orchestration**: Kubernetes allow syou to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks**: You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing**: You provide Kubernetes with a cluster of nodes that it can use to run containerized takss. You tell Kubernetes how much CPU and memory(RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- **Self-healing**: Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to client until they are ready to serve.
- **Secret and configuration management**: Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configurations.
- **Batch execution**: In addition to service, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
- **Horizontal scaling**: Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
- **IPv4/IPv6 dual-stack**: Allocation of IPV4 and IPV6 addresses to Pods and Services.
- **Designed for extensibility**: Add features to your Kubernetes cluster without changing upstream source code.

## What Kubernetes is not

Kubernetes is an container orchestration tool; anything not related to a container(running with already built docker image), Kubernetes does not do.

