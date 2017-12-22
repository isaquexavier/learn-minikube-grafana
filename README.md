# learn-minikube-grafana
A straightforward tutorial regarding monitoring resource usage in [Kubernetes](https://kubernetes.io) (minikube) of containers, pods, services, and whole clusters using cAdvisor, Heapster and Grafana.

## Target description
**Note:** this tutorial should be used locally for testing purposes. The intention is not deploying on Production, but only for local tests tasks.

This tutorial is addressed for those who are using Kubernetes as container orchestrator for a containerized application environment.

The main goal here is to show how can we monitor resources from a Cluster in Kubernetes. For that, we'll set up an environment with [minikube](https://kubernetes.io/docs/getting-started-guides/minikube/ "What is Minikube") as well as grafana which will play as the monitoring tool.

- Minikube: in short words  is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

- A very nice definition regarding *"Resource Usage Monitoring in Kubernetes"*:

   *Understanding how an application behaves when deployed is crucial to scaling the application and providing a reliable service. In a Kubernetes cluster, application performance can be examined at many different levels: containers, pods, services, and whole clusters. As part of Kubernetes we want to provide users with detailed resource usage information about their running applications at all these levels. This will give users deep insights into how their applications are performing and where possible application bottlenecks may be found. In comes Heapster, a project meant to provide a base monitoring platform on Kubernetes.*

   Resource [Kubernetes blog](http://blog.kubernetes.io/2015/05/resource-usage-monitoring-kubernetes.html)


### Requirements

- For this tutorial, you should have a minimum knowledge regarding containerized applications environments, Kubernetes, and tools for application monitoring ion general.
- VirtualBox (Version 5.2.2 r119230) 
- The VT-x or AMD-v virtualization must be enabled in your computer’s BIOS.
- Manual installation: a Virtual machine - Ubuntu 16.04.3 LTS
- Automated Installation: Vagrant (https://www.vagrantup.com/downloads.html)

## Manual Installation

### Minikube

- **Note:** *minikube requires a [Hypervisor](https://kubernetes.io/docs/tasks/tools/install-minikube/#install-a-hypervisor) installation in order to work.*

    As we'll not use an ordinary Hypervisor, we should install [Docker](https://www.docker.com/what-docker), for that we followed the tutorial from [DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) which works fine for ours purposes. *You must follow the guide until the end of the second step.*

- The Minikube installation is very straightforward you may use the [Official Reference](https://kubernetes.io/docs/tasks/tools/install-minikube)  
For our tutorial we decided to use ["none"](https://github.com/kubernetes/minikube#requirements) option for the driver. It's not very recommended, but decided no to not nest Virtual Machines.

- After the installation you can start minikube with the following command:
    ```
    $ minikube start --vm-driver=none
    ```

    **Note:** Because we are using "none" option for driver, a warning will be presented at the end of starting process.
    It's up to you to take in consideration or not.

    **Note:** In addition, due to the same reason above, in order to prevent you have to type ```sudo``` for all commands you must properly assign the right permissions to the installation resources for ```kubectl``` and ```minikube```.

- Finally check whether everything is running properly or not
    ```
    $ minikube status
    minikube: Running
    cluster: Running
    kubectl: Correctly Configured: pointing to minikube-vm at 127.0.0.1
    ```

- You may check the IP address for minikube using the following command:
    ```
    $﻿minikube ip
    ```

### Minikube Add-ons
*"Minikube has a set of built in [addons](https://github.com/kubernetes/minikube/blob/master/docs/addons.md#add-ons) that can be used enabled, disabled, and opened inside of the local k8s environment."*
For sake of simplicity we'll use the addon for Heapster and Grafana.

You may list the addons and their status using the following command:

```
$ minikube addons list
- coredns: disabled
- kube-dns: enabled
- heapster: disabled
- efk: disabled
- ingress: disabled
- registry: disabled
- dashboard: enabled
- storage-provisioner: enabled
- registry-creds: disabled
- addon-manager: enabled
- default-storageclass: enabled
```

### Heapster and cAdvisor

*"Heapster is a cluster-wide aggregator of monitoring and event data. It currently supports Kubernetes natively and works on all Kubernetes setups. Heapster runs as a pod in the cluster, similar to how any Kubernetes application would run. The Heapster pod discovers all nodes in the cluster and queries usage information from the nodes’ Kubelets, the on-machine Kubernetes agent. The Kubelet itself fetches the data from cAdvisor. Heapster groups the information by pod along with the relevant labels. This data is then pushed to a configurable backend for storage and visualization. Currently supported backends include InfluxDB (with Grafana for visualization), Google Cloud Monitoring and many others described in more details here. The overall architecture of the service can be seen below."*


![monitoring-architecture](/monitoring-architecture.jpeg?raw=true)


If you want more details about cAdvisor and Heapster you may access the following article. *"[Resource Usage Monitoring in Kubernetes](http://blog.kubernetes.io/2015/05/resource-usage-monitoring-kubernetes.html)"*

- Once we have minikube in place let's move on and "install", better saying enable the addon (Heapster), which is responsible for get resource usage metrics in partnership with cAdvisor.

- In order to enable Heapster addon let's follow the steps below
  1. List all minikube addon - run the following command:
     ```
     ﻿$ minikube addons enable heapster
     heapster was successfully enabled
     ```

  2. If you list the addons again you'll see Heapster enabled
  3. You can list the services for minikube using the following command
    ```
    $﻿minikube service list
     |-------------|----------------------|------------------------|
     |  NAMESPACE  |         NAME         |          URL           |
     |-------------|----------------------|------------------------|
     | default     | kubernetes           | No node port           |
     | kube-system | heapster             | No node port           |
     | kube-system | kube-dns             | No node port           |
     | kube-system | kubernetes-dashboard | http://127.0.0.1:30000 |
     | kube-system | monitoring-grafana   | http://127.0.0.1:30002 |
     | kube-system | monitoring-influxdb  | No node port           |
     |-------------|----------------------|------------------------|

    ```  
  5. Once you access Grafana ([```http://{MINIKUBE-IP}:30002```](http://{MINIKUBE-IP}:30002)) you may check the defaults Dashboards for POD monitoring
    ![grafana-dashboard](/grafana-dashboard.jpeg?raw=true)
  6. You may access Kubernetes Dashboard ([```http://{MINIKUBE-IP}:30000```](http://{MINIKUBE-IP}:30000)) and take a look through all the resources and information
    ![kubernetes-dashboard](/kubernetes-dashboard.jpeg?raw=true)

## Automated Installation

### Vagrant

- Download and install Vagrant from [here](https://www.vagrantup.com/downloads.html) 
- change into git root directory and run vagrant up
- change into the VM with vagrant ssh

### Script

- To install and configure the Virtual Machine run the command: ```$ vagrant up```
- Login in the Virtual Machine (after the installation has been completed)
  - Credentials can be found into the file ```~/.vagrant.d/boxes/ubuntu-VAGRANTSLASH-xenial64/{vagrant-version}/virtualbox/Vagrantfile```
  - To start the Linux UI run the command: ```startx``` 

- **Note**: further improvements would be set a default password for the user and configure the Linux GUI to start by default.

### References

Official reference from [Kubernetes](https://kubernetes.io)

Official reference from Kubernetes for [minikube](https://kubernetes.io/docs/getting-started-guides/minikube).

Git repo for [minikube](https://github.com/kubernetes/minikube#what-is-minikube).

Installing [Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04) on Ubuntu.

You may find the official Grafana documentation [here](https://grafana.com/)
