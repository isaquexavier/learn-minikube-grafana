# learn-minikube-grafana
A straightforward tutorial regarding monitoring resource usage in [Kubernetes](https://kubernetes.io) (minikube) of containers, pods, services, and whole clusters using cAdvisor, Heapster and Grafana.

### Target description
**Note:** this tutorial should be used locally for testing purposes. The intention is not deploying on Production, but only for local tests tasks. 

This tutorial is addressed for those who are using Kubernetes as container orchestrator for a containerized application environment.

The main goal here is to show how can we monitor resources from a Cluster in Kubernetes. For that, we'll set up an environment with [minikube](https://kubernetes.io/docs/getting-started-guides/minikube/ "What is Minikube") as well as grafana which will play as the monitoring tool.

- Minikube: in short words  is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

- A very nice definition regarding *"Resource Usage Monitoring in Kubernetes"*:
   
   *Understanding how an application behaves when deployed is crucial to scaling the application and providing a reliable service. In a Kubernetes cluster, application performance can be examined at many different levels: containers, pods, services, and whole clusters. As part of Kubernetes we want to provide users with detailed resource usage information about their running applications at all these levels. This will give users deep insights into how their applications are performing and where possible application bottlenecks may be found. In comes Heapster, a project meant to provide a base monitoring platform on Kubernetes.*
   
   Resource [Kubernetes blog](http://blog.kubernetes.io/2015/05/resource-usage-monitoring-kubernetes.html) 


### Requirements

- For this tutorial, you should have a minimum knowledge regarding containerized applications environments, Kubernetes, and tools for application monitoring ion general.
- Operation System used: Ubuntu 16.04.3 LTS running on a VirtualBox VM﻿Version 5.2.2 r119230
- The VT-x or AMD-v virtualization must be enabled in your computer’s BIOS.
  

### Minikube

- **Note:** *minikube requires a [Hypervisor](https://kubernetes.io/docs/tasks/tools/install-minikube/#install-a-hypervisor) installation in order to work.*

    As we'll not use an ordinary Hypervisor, we should install [Docker](https://www.docker.com/what-docker), for that we followed the tutorial from [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) which works fine for ours purposes. *You must follow the guide until the end of the second step.*

- The Minikube installation is very straightforward you may use the [Official Reference](https://kubernetes.io/docs/tasks/tools/install-minikube)  
For our tutorial we decided to use ["none"](https://github.com/kubernetes/minikube#requirements) option for the driver. It's not very recommended, but decided no to not nest Virtual Machines.

- Add-ons: *"Minikube has a set of built in [addons](https://github.com/kubernetes/minikube/blob/master/docs/addons.md#add-ons) that can be used enabled, disabled, and opened inside of the local k8s environment."*
For sake of simplicity we'll use the add-on for Heapster and Grafana.

### Heapster and cAdvisor

*"Heapster is a cluster-wide aggregator of monitoring and event data. It currently supports Kubernetes natively and works on all Kubernetes setups. Heapster runs as a pod in the cluster, similar to how any Kubernetes application would run. The Heapster pod discovers all nodes in the cluster and queries usage information from the nodes’ Kubelets, the on-machine Kubernetes agent. The Kubelet itself fetches the data from cAdvisor. Heapster groups the information by pod along with the relevant labels. This data is then pushed to a configurable backend for storage and visualization. Currently supported backends include InfluxDB (with Grafana for visualization), Google Cloud Monitoring and many others described in more details here. The overall architecture of the service can be seen below."*


![monitoring-architecture](/monitoring-architecture.jpeg?raw=true)


If you want more details about cAdvisor and Heapster you may access the following article. *"[Resource Usage Monitoring in Kubernetes](http://blog.kubernetes.io/2015/05/resource-usage-monitoring-kubernetes.html)"*

- Once we have minikube in place let's move on and "install", better saying enable the add-on (Heapster), which is responsible for get resource usage metrics in partnership with cAdvisor.



### Grafana

### Dashboards

## References

Official reference from [Kubernetes](https://kubernetes.io)

Official reference from Kubernetes for [minikube](https://kubernetes.io/docs/getting-started-guides/minikube).

Git repo for [minikube](https://github.com/kubernetes/minikube#what-is-minikube).

Installing [Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) on Ubuntu.

https://github.com/kubernetes/heapster