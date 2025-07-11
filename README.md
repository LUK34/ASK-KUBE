# 1. AWS EKS Setup:
- Refer AWS EKS setup from ashok website. Refer the below URL link
- https://www.ashokit.in/yt-content/softwares-installation
- or refer the below youtube URL link
- https://www.youtube.com/watch?v=is99tq4Zwsc

# 2. Kubernetes Architecture:
- 
=> K8S will follow cluster architecture.

=> Cluster means group of servers will be available.

=> In K8s Cluster we will have master node (control plane) and worker nodes.


## K8S Cluster Components

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


## K8S Cluster Setup

- https://github.com/ashokitschool/DevOps-Documents/blob/main/05-EKS-Setup.md


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
- eksctl create cluster --name ashokit-cluster4 --region us-east-1 --node-type t2.medium  --zones us-east-1a,us-east-1b
- eksctl delete cluster --name ashokit-cluster4 --region us-east-1


## K8S Manifest YML

=> To deploy our application in kubernetes we need MANIFEST YML file.

=> In k8s manifest YML we will write 4 sections.

apiVersion: <resource-version-number>

kind: <resource-type>

metadata: <resource-info>

spec: <container-info>

POD Maniest YML

---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
...

Note: Save the above content in .yml file.

$ kubectl get pods

$ kubectl apply -f <yml-file-name>

$ kubectl get pods


$ kubectl logs <pod-name>

### Display in which worker node our pod is running
$ kubectl get pods -o wide

Note: By default PODS can be accessed only with in the cluster. We can't access PODS outside.

=> To access PODS outside the cluster we need to expose the PODS by using K8S Services concept.

## K8S Service

=> K8S service is used to group the pods and expose them for outside access.

=> In K8S we have 3 types of services

		1) Cluster IP
		2) Node PORT
		3) Load Balancer

=> When we deploy our application in k8s then PODS will be created for our application..

=> For every POD one IP will be generated.

=> PODS are short lived objects. We should not access PODS using POD IP because PODS can be destroyed and re-created at any point of time.

### 1) ClusterIP (Default): It generates one static IP for all PODS based on POD label.
- Purpose: Exposes the service internally within the Kubernetes cluster.
- Access: Only other pods or services inside the cluster can access it.
- Use Case: Good for internal communication between microservices (e.g., frontend → backend).
- Note: You cannot access a ClusterIP service from your local machine or the internet.
- Ex: 192.168.10.89
- Note: using ClusterIP we can access PODS only with in the cluster.
- Note: PODS created , PODs Destroyed, PODS increased or decreased still ClusterIP will not change.
- Ex: Database PODS we should access only within in the cluster. Outside ppl should not access DB pods. In this scenario we can use CLUSTER IP service.
### 2) NodePort : It is used to expose the PODS for outside access. 
- Purpose: Exposes the service on each node's IP at a static port (between 30000–32767).
- Access: Can be accessed from outside the cluster using NodeIP:NodePort.
- Use Case: For testing or simple external access when no cloud load balancer is available.
- Note: It still uses a ClusterIP internally, so pods communicate via ClusterIP, and external users access via NodePort.
- Using NODE PUBLIC IP we can access PODS running in that node outside of the cluster also.
 
### 3) LoadBalancer: It is used to expose the PODS for outside access.
- Purpose: Provisions an external load balancer (from cloud provider like AWS, Azure, GCP).
- Access: Accessible via external IP from the internet.
- Use Case: When you want to expose your app publicly and use cloud-native load balancing.
- When we can access LOAD BALANCER URL, it will distribute the load to all the nodes and all the PODS available for our application.

## K8S Service Manifest YML

---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080
    nodePort: 30070
...

Note: Save this data with .yml extesion

$ kubectl get svc

$ kubectl apply -f <svc-manifest-yml>

$ kubectl get svc

Note: Enable NODE PORT in security group inbound rules.

Note: To access our application use below URL

		URL : http://worker-node-public-ip:node-port/java-web-app/

### To delete all pods and services inside the cluster
$ kubectl delete all --all

## K8S namespaces

=> Namespaces are used to group the resources logically.

			mysql-db-pods =====> mysql-db-ns

			backend-app-pods =====> backend-ns

			frontend-app-pods =====> frontend-ns

=> Inside k8s cluster we can create multiple namespaces.

=> Each namespace is isolated with another namespace.

Note: When we delete a namespace all the resources belongs to that namespace also gets deleted.


### display k8s namespaces
kubectl get ns

### KCNA Exam Prep Material
Risabh -> How he passed KCNA
https://youtu.be/6no8zauQH88?si=L2lTOLActyl9qucE

Notes Material:
https://notes.kaiwalyakoparkar.com/kcna


### get pods of specific namespace
kubectl get pods -n kube-system

Note: In kubectl command if we don't specify any namespace then it will consider "default" namespace.


=> In K8s we can create namespace in 2 ways

		1) using "kubectl create ns" command

		2) Using manifest YML file 

## Approach-1 : 

### create namespace
kubectl create ns ashokitns

### delete namespace
kubectl delete ns ashokitns

Note: When we delete a namespace all the resources belongs to that namespace also gets deleted.

## Approach-2 : 

---
apiVersion: v1
kind: Namespace
metadata:
 name: ashokit-backend-ns
...

### CMD:
- kubectl apply -f <yml-file-name>
- kubectl delete ns ashokit-backend-ns

## Namespace + POD + Service
- kubectl delete all --all
- kubectl delete ns ashokit-backend-ns

---
apiVersion: v1
kind: Namespace
metadata:
 name: ashokit
---
apiVersion: v1
kind: Pod
metadata:
 name: javawebapppod
 namespace: ashokit
 labels:
  app: javawebapp
spec:
 containers:
  - name: javac1
    image: ashokit/javawebapp
    ports:
     - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
 name: javawebappsvc
 namespace: ashokit
spec:
 type: NodePort
 selector:
  app: javawebapp
 ports:
  - port: 80
    targetPort: 8080
    nodePort: 30070
...

- kubectl apply -f 07-ns-pods-serv.yml
- kubectl get pods -n ashokit
- kubectl get svc -n ashokit
- kubectl get all -n ashokit

