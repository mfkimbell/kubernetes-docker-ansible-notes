# kubernetes-docker-notes

## Docker

<img width="1387" alt="Screenshot 2024-05-28 at 3 49 00 PM" src="https://github.com/mfkimbell/aws-and-other-cloud-notes/assets/107063397/20cdb1f4-0e5c-4a90-bc90-983fd8abf301">
* has all your code, dependencies (package.json, pyproject.toml), runtime (python 3.9)
* islated from one another but share the host system's kernal
* avoids the "works on my machine" problem, used docker for a webscraper before because of this


## Docker Compose

<img width="187" alt="Screenshot 2024-05-28 at 3 49 00 PM" src="https://github.com/user-attachments/assets/4848648a-3a26-4754-92d6-9267b7873d7d">

* a tool for building up multiple containers with a YAML filee
* You can define networks, volumes, and environment variables easily within the YAML file.
* good for testing docker containers as you can enable "hot reloading" which forces containers to use local files so updates are immediate
* can be used for very small projects on one server (spaceify)

<img width="756" alt="Screenshot 2025-02-19 at 12 21 51 PM" src="https://github.com/user-attachments/assets/8b89b395-a5e6-412b-974d-aa1983bd717b" />


## Kubernetes
<img width="1407" alt="Screenshot 2024-05-28 at 3 49 17 PM" src="https://github.com/mfkimbell/aws-and-other-cloud-notes/assets/107063397/9247ef6d-4a38-4a88-a0ad-7edfda1c4eaf">

## EKS (Elastic Kubernetes Service)
![Kubernetes-architecture-diagram-1-1-1024x698](https://github.com/mfkimbell/aws-and-other-cloud-notes/assets/107063397/6ab0ea6c-24ce-4181-8b7c-e6485c1f1a4a)



# Docker

* A way of packaging an application with all its dependencies, code, and enviornment variables/configurations. 
* As opposed to VM's Docker doesn't need it's own OS. It shares the host's OS (specifically the kernal). So it's more lightweight. 
* Containers start up faster than VMs
* portability, consistency, and isolation are the benefits

* We start with a `Dockerfile` that generally contains a slimmed down OS, a runtime enviornment (like Node), Application files, libraries etc. This can be used to create an `Image`. when you run that image, it's considered a `Container`


### Fireship video
https://www.youtube.com/watch?v=rIrNIzy6U_g

### CMD
* Defines the main command for the container in the dockerfile
<img width="341" alt="Screenshot 2024-08-09 at 4 54 52 PM" src="https://github.com/user-attachments/assets/5d80d2cb-0666-4101-87b8-847e6026bda4">
<img width="744" alt="Screenshot 2024-08-09 at 4 55 21 PM" src="https://github.com/user-attachments/assets/c9aca227-58bb-407c-9956-36a25b5e76f1">

<img width="760" alt="Screenshot 2024-08-09 at 4 55 54 PM" src="https://github.com/user-attachments/assets/665f5c2f-b281-471a-a2ec-c61834e1f35a">
<img width="818" alt="Screenshot 2024-08-09 at 4 56 28 PM" src="https://github.com/user-attachments/assets/64730eff-5fe2-4c17-808c-797cf448e66a">

### Override example
<img width="830" alt="Screenshot 2024-08-09 at 4 58 02 PM" src="https://github.com/user-attachments/assets/bf135c3a-4c20-4b47-8a08-8d95100139e9">
<img width="841" alt="Screenshot 2024-08-09 at 4 58 46 PM" src="https://github.com/user-attachments/assets/d9ee5ecc-2379-464a-8d44-570e2a60cec6">

### Build speed
* Each layer when running "docker build" is hashed
* Its also Cached, so isn't rebuilt if it doesn't change
<img width="878" alt="Screenshot 2024-08-09 at 5 00 45 PM" src="https://github.com/user-attachments/assets/f25d2f8f-51e5-4aab-9ea7-cd71d20a7bbe">

<img width="1102" alt="Screenshot 2024-07-07 at 12 20 17 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/cb2c7720-9013-4547-8155-e0f999ad1afc">

### Docker Compose
* used for multi-container applications
* like frontend, backend, database
* also can establish a network
* Docker Compose is a tool for defining and running multi-container Docker applications. It uses YAML files to configure the application's services (containers), networks, and volumes, making it easier to manage and orchestrate complex applications composed of multiple Docker containers.

# Kubernetes

<img width="1109" alt="Screenshot 2024-07-07 at 12 20 39 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/5e8cba67-274b-4f1d-aff8-796815cd21ee">

<img width="556" alt="Screenshot 2024-05-20 at 4 04 51 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/8a539931-b1d6-41b2-b3d1-201b9135da07">

In a Kubernetes cluster, the control plane manages the cluster's overall state and configuration, while the worker nodes run the application workloads. Each worker node runs a kubelet, which ensures that containers are running in pods as specified. A pod is the smallest deployable unit in Kubernetes and can contain one or more containers that share the same network namespace and storage.


<img width="556" alt="Screenshot 2024-05-20 at 4 04 51 PM" src="https://github.com/user-attachments/assets/f577a64a-dabe-42b7-a33e-1f2542b5b454">

### Service and Worker Nodes Interaction:

* When a Service is created, it is not directly tied to specific worker nodes. Instead, it selects Pods based on labels, regardless of which nodes those Pods are running on.
* Kubernetes uses a component called kube-proxy that runs on every worker node. The kube-proxy is responsible for forwarding traffic to the correct Pods that the Service routes to.
* Kube-proxy sets up the necessary network routes so that traffic sent to the Service's IP address is distributed among the Pods. This distribution happens regardless of which worker nodes those Pods are located on.


![image](https://github.com/user-attachments/assets/5cb101c9-1c7c-44d6-8414-46dfcf0c5278)

### Kubernetes Cluster
So the biggest unit is the **kubernetes cluster**, which is to run applications and manage resources. You can have multiple clusters if you want isolation for different enviornments. 

Each **cluster** contains a **control plane** and **worker nodes**

### Control Plane
* holds the kubernetes api and UI capabiliites
* stores cluster data
* makes decisions about controlling the scaling of the application
* etcd: stores cluster state and information
* scheduler: schedules pods for worker node acceptance
* controller manager: managers controllers like the deployment controller and replication controller (into weeds)

### Worker nodes
* run the actual pods and containers
* has kublet: A DAEMON/agent that runs on each worker node, ensuring containers are running in the Pods. It also decides which pods to run onto which nodes. Ensuring that containers are running in the Pods.
Communicating with the Kubernetes control plane to receive instructions on which Pods should be run on that node. However, it does not decide which Pods should go where—this decision is made by the **kube-scheduler**.
* has kubeproxy: Handles network traffic for the Pods, ensuring traffic is properly routed between services and Pods. (distrubutes traffic evenly between pods, provides loadbalancingRe)
* has docker aka a runtime enviornment for the pods.

<img width="686" alt="Screenshot 2024-08-09 at 12 07 54 PM" src="https://github.com/user-attachments/assets/24dc381d-b675-4b7d-8cf6-7757ce78475a">

### Services

* set of pods, abstracted from individual worker nodes

### Pods
* hosts containers
* has shared networking and storage for those containers

### Docker Swarm

In Docker Swarm, the architecture is similarly structured for orchestrating containerized applications. The Swarm manager is responsible for the overall management of the cluster, including scheduling tasks and maintaining the desired state. The worker nodes execute the tasks assigned to them by the manager, running these tasks within Docker containers. This ensures a distributed and resilient deployment of applications across the cluster.

## Ansible
<img width="1116" alt="Screenshot 2024-07-07 at 12 21 00 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/98d71dce-94bb-4137-a5e2-3696cfa939cd">

Ansible also uses a `Declarative Language`.

* **Configuration Management:** Ansible is primarily a configuration management tool. It is designed to automate the provisioning and configuration of servers and software.
* **Agentless:** Ansible is agentless, meaning it doesn't require any software to be installed on the target machines. It communicates with them using SSH.'
* **SSH ON STEROIDS**

## Ansible

<img width="1413" alt="334571210-01702314-8e57-46f4-92de-0aa6e9e26cbe" src="https://github.com/user-attachments/assets/efd53885-fb53-4bea-82a0-33c23e84af90" />

#### Ansible Ansible also uses a `Declarative Language`.

Configuration Management: Ansible is primarily a configuration management tool. It is designed to automate the provisioning and configuration of servers and software.
Agentless: Ansible is agentless, meaning it doesn't require any software to be installed on the target machines. It communicates with them using SSH.'
SSH ON STEROIDS
ssh on steroids
No requirement for ansible agent on recieving VM
