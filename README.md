# Kubernetes from Vagrant

Vanilla Kubernetes from Vagrant and VirtualBox.

## About
This is an educational project to learn Kubernetes, the not-too-hard way.

## What Comes Right Out of The Box
This project automatically:
1. Creates 1 Control Node.
2. Creates 2 Worker Nodes.
3. Configures Linux Kernel (networking) settings on each Node.
4. Installs containerd as the CRI (Container Runtime Interface).
5. Installs (the previous version of) kubeadm, kubelet, and kubectl on each Node.
6. Installs cri-tools.
7. Installs etcd-client.

## What Does NOT Come Right Out of The Box
The Vagrantfile is kept intentionally minimal to allow others the freedom to choose what concepts to practice, and what components to administer on their cluster.

This project does **not** automatically:
1. Initialize the cluster using `kubeadm`
2. Attach the Worker Nodes to the cluster.
3. Set up external communication with the cluster using a ["kubeconfig" file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).
4. Install a CNI (Container Network Interface) Plugin like Calico or Cilium.
5. Install a metrics provider, which is needed to use Horizontal Pod Autoscalers (HPAs). One option is to install the [Kubernetes Metrics Server](https://github.com/kubernetes-sigs/metrics-server#installation).

## Prerequisites
The following tools must be installed in order to use this project:
1. [Install Virtualbox](https://www.virtualbox.org/wiki/Downloads).
2. [Install Vagrant](https://developer.hashicorp.com/vagrant/install).

## Get Started

The project can be spun up with a simple command on your terminal:
```
vagrant up
```
