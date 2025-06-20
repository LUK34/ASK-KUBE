## 1. AWS EKS Setup:
- Refer AWS EKS setup from ashok website. Refer the below URL link
- https://www.ashokit.in/yt-content/softwares-installation
- or refer the below youtube URL link
- https://www.youtube.com/watch?v=is99tq4Zwsc

## 2. Kubernetes Architecture:
- 
=> K8S will follow cluster architecture.

=> Cluster means group of servers will be available.

=> In K8s Cluster we will have master node (control plane) and worker nodes.

========================
K8S Cluster Components
========================

1) Control Node (Master Node)

	- API Server
	- Schedular
	- Controller Manager
	- ETCD

2) Worker Nodes

	- Kubelet
	- Kube Proxy
	- Docker Runtime
	- POD
	- Container


=> To deploy our application using k8s then we need to communicate with control node.	

=> We will use KUBECTL (CLI) to communicate with control plane.

=> "API Server" will recieve the request given by "kubectl" and it will store that request in "ETCD" with pending status.

=> "ETCD" is an internal database of k8s cluster.

=> "Schedular" will identify pending requests available in ETCD and it will identify worker node to schedule the task.

Note: Schedular will identify worker node by using kubelet.

=> "Kubelet" is called as Node Agent. It will maintain all the worker node information.

=> "Kube Proxy" will provide network for the cluster communication.

=> "Controller Manager" will verify all the taks are working as expected or not.

=> In Worker Node, "Docker Engine" will be available to run docker container.

=> In K8s, container will be created inside POD.

=> POD is a smallest building block that we can create in k8s cluster to run our applications.

Note: In K8s, everything will be represented as POD only.

=> Pods are used to deploy applications and manage their lifecycle within a Kubernetes environment.

==================
K8S Cluster Setup
==================

=> We can setup k8s cluster in multiple ways

1) Self Managed k8S cluster (We need to setup everything)
		
	a) Mini Kube  => Sinle Node Cluster => Only for beginners practice

	b) Kubeadm => Multi Node Cluster (HA)

2) Provider Managed K8S cluster (ready made)

		a) AWS EKS 
		b) Azure AKS
		c) GCP GKE

Note: Provider will give ready made cluster for us.		

#### Note: Provider Managed Clusters are chargable #####