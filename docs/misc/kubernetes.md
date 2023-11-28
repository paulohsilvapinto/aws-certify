---
layout: default
title: Kubernetes
parent: Miscellaneous
---

# Kubernetes
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

---

## General

Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications. 

It groups containers that make up an application into logical units for easy management and discovery.

Some benefits of using Kubernetes are:

- **Automated progressive rollouts and rollbacks.**
- **Service discovery and Load Balancing.**
- **Self-healing**: Restarts containers that fail, replaces and reschedules containers when nodes die, and kills containers that don't respond.
- **Secrets management.**

## Concepts

- **Kubernetes Cluster**: A set of one or more worker machines that will run the containers. It must contain at least one worker node. If there is more than one, then one will be assigned as the master node, which will be responsible for controlling and monitoring the other nodes, called slave nodes.
- **Node**: Each worker machine in the Kubernetes Cluster is called *Node*, and each node will run a set of containerized applications. Nodes can be physical or virtual machines.
- **Pods**: A set of running containers in the cluster that must work together for an application to be available.
- **Control plane**: manages the worker nodes and the Pods in the cluster.

![Kubernetes Architecture](https://www.cncf.io/wp-content/uploads/2020/08/Kubernetes-architecture-diagram-1-1.png)

### Node Affinity, Taints, and Tolerance

Node affinity is a property of Pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the exact opposite: they allow a node to repel a set of pods.

Tolerations are applied to pods instead of nodes, allowing the scheduler to schedule the pods to a specific node with matching taints. Tolerations do not guarantee scheduling.

Taints and tolerations work together to ensure pods are not scheduled onto inappropriate nodes. 