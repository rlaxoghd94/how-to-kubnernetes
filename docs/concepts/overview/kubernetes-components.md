# Kubernetes Components

An overview of the key components that make up a Kubernetes clusters

## Core Components

A Kubernetes cluster consists of a control plane and one or more worker nodes:

### Control Plane Components

- [kube-apiserver](https://kubernetes.io/docs/concepts/architecture/#kube-apiserver)
  - The core component server that exposes the Kubernetes HTTP API
- [etcd](https://kubernetes.io/docs/concepts/architecture/#etcd)
  - Consistent and highly-available key value store for all API server data
- [kube-scheduler](https://kubernetes.io/docs/concepts/architecture/#kube-scheduler)
  - Looks for Pods not yet bound to a node, and assigns each Pod to a suitable node
- [kube-controller-manager](https://kubernetes.io/docs/concepts/architecture/#kube-controller-manager)
  - Runs controllers to implement Kubernetes API behaviour
- [cloud-controller-manager](https://kubernetes.io/docs/concepts/architecture/#cloud-controller-manager) (**optional**)
  - Integrates with underlying cloud provider(s)

### Node Components

Run on every node, maintaing running pods and providing the Kubernetes runtime environment:

- [kubelet](https://kubernetes.io/docs/concepts/architecture/#kubelet)
  - Ensures that Pods are running, including their containers
- [kube-proxy](https://kubernetes.io/docs/concepts/architecture/#kube-proxy) (**optional**)
  - Maintains network rules on nodes to implement Services
- [Container runtime](https://kubernetes.io/docs/concepts/architecture/#container-runtime)
  - Software responsible for running containers (i.e. Docker)

### Addons

Addons extend the functionality of Kubernetes. A few important examples include:

- [DNS](https://kubernetes.io/docs/concepts/architecture/#dns)
  - For cluster-wide DNS resolution
- [Web UI](https://kubernetes.io/docs/concepts/architecture/#web-ui-dashboard) (**Dashboard**)
  - For cluster management via a web interface
- [Container Resource Monitoring](https://kubernetes.io/docs/concepts/architecture/#container-resource-monitoring)
  - For collecting and storing container metrics
- [Cluster-level logging](https://kubernetes.io/docs/concepts/architecture/#cluster-level-logging)
  - For saving container logs to a central log state


