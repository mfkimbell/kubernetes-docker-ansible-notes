# kubernetes-docker-notes



## Docker Compose

<img width="187" alt="Screenshot 2024-05-28 at 3 49 00 PM" src="https://github.com/user-attachments/assets/4848648a-3a26-4754-92d6-9267b7873d7d">

* a tool for building up multiple containers with a YAML filee
* You can define networks, volumes, and environment variables easily within the YAML file.
* good for testing docker containers as you can enable "hot reloading" which forces containers to use local files so updates are immediate
* can be used for very small projects on one server (spaceify)

<img width="756" alt="Screenshot 2025-02-19 at 12 21 51 PM" src="https://github.com/user-attachments/assets/8b89b395-a5e6-412b-974d-aa1983bd717b" />





## Docker

<img width="1387" alt="Screenshot 2024-05-28 at 3 49 00 PM" src="https://github.com/mfkimbell/aws-and-other-cloud-notes/assets/107063397/20cdb1f4-0e5c-4a90-bc90-983fd8abf301">

* has all your code, dependencies (package.json, pyproject.toml), runtime (python 3.9)
* islated from one another but share the host system's kernal
* avoids the "works on my machine" problem, used docker for a webscraper before because of this

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

* ENTRYPOINT is harder to overwrite, you generally use this when the docker container is going to be acting more as an executable
* CMD can be more easily overwritten
  
<img width="775" alt="Screenshot 2025-02-19 at 12 53 47 PM" src="https://github.com/user-attachments/assets/c767777d-0484-4043-ba69-798c9190c926" />

<img width="764" alt="Screenshot 2025-02-19 at 12 54 50 PM" src="https://github.com/user-attachments/assets/b6475957-cc14-4450-82ba-26349a0c9c8b" />
A user can accidentally change the CMD by adding runtime arguments

### Override example
<img width="830" alt="Screenshot 2024-08-09 at 4 58 02 PM" src="https://github.com/user-attachments/assets/bf135c3a-4c20-4b47-8a08-8d95100139e9">
<img width="841" alt="Screenshot 2024-08-09 at 4 58 46 PM" src="https://github.com/user-attachments/assets/d9ee5ecc-2379-464a-8d44-570e2a60cec6">

### Build speed
* Each layer when running "docker build" is hashed
* Its also Cached, so isn't rebuilt if it doesn't change
<img width="878" alt="Screenshot 2024-08-09 at 5 00 45 PM" src="https://github.com/user-attachments/assets/f25d2f8f-51e5-4aab-9ea7-cd71d20a7bbe">

<img width="1102" alt="Screenshot 2024-07-07 at 12 20 17 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/cb2c7720-9013-4547-8155-e0f999ad1afc">


# Kubernetes


## EKS (Elastic Kubernetes Service)
![Kubernetes-architecture-diagram-1-1-1024x698](https://github.com/mfkimbell/aws-and-other-cloud-notes/assets/107063397/6ab0ea6c-24ce-4181-8b7c-e6485c1f1a4a)

* All of my projects, I ran EKS with the **Worked Nodes on EC2s** but you can also use **Fargate to let AWS handle the nodes**, making it serverless
* AWS ALWAYS manages the control plane (EKS Control Plane)
* `eksctl` is used to manage the EKS cluster

## Scaling
🚀 Best Practice:
1️⃣ Always scale pods first when possible.
2️⃣ Scale worker nodes only when pods need more space.
## Availability
pods are re-upped by the kubelet
  
<img width="603" alt="Screenshot 2025-02-19 at 2 50 42 PM" src="https://github.com/user-attachments/assets/05f4a065-9249-42d7-81a0-ea29e897a365" />

<img width="1109" alt="Screenshot 2024-07-07 at 12 20 39 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/5e8cba67-274b-4f1d-aff8-796815cd21ee">

<img width="556" alt="Screenshot 2024-05-20 at 4 04 51 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/8a539931-b1d6-41b2-b3d1-201b9135da07">

In a Kubernetes cluster, the control plane manages the cluster's overall state and configuration, while the worker nodes run the application workloads. Each worker node runs a kubelet, which ensures that containers are running in pods as specified. A pod is the smallest deployable unit in Kubernetes and can contain one or more containers that share the same network namespace and storage.

The control plane does NOT point to services, we have a routing system that points to services from external traffic (and internal)
<img width="800" alt="Screenshot 2025-02-19 at 2 11 39 PM" src="https://github.com/user-attachments/assets/5dfac4d7-2f44-4782-8cd9-b945ded4d5d4" />


<img width="556" alt="Screenshot 2024-05-20 at 4 04 51 PM" src="https://github.com/user-attachments/assets/f577a64a-dabe-42b7-a33e-1f2542b5b454">

### Service and Worker Nodes Interaction:

* When a Service is created, it is not directly tied to specific worker nodes. Instead, it selects Pods based on labels, regardless of which nodes those Pods are running on.
* Kubernetes uses a component called kube-proxy that runs on every worker node. The kube-proxy is responsible for forwarding traffic to the correct Pods that the Service routes to.
* Kube-proxy sets up the necessary network routes so that traffic sent to the Service's IP address is distributed among the Pods. This distribution happens regardless of which worker nodes those Pods are located on.


![image](https://github.com/user-attachments/assets/5cb101c9-1c7c-44d6-8414-46dfcf0c5278)

### Kube-Scheduler

<img width="747" alt="Screenshot 2025-02-19 at 2 57 15 PM" src="https://github.com/user-attachments/assets/acac8e39-b079-4080-9123-e044723161e6" />

### Kubelet

<img width="627" alt="Screenshot 2025-02-19 at 2 57 59 PM" src="https://github.com/user-attachments/assets/bf25a9cd-425e-4a44-9056-90c815f19965" />

* Pod lifecycle management: Starts, stops, and restarts containers as needed.

## Kubernetes Ingress

#### THERE IS NO DEFAULT, NGINX is commmonly used, but HA-PROXY is also an option
* Directs traffic to Kubernetes Services
* Kubernetes Ingress is a powerful resource that manages external access to services running inside a Kubernetes cluster. It primarily handles HTTP/HTTPS routing, providing load balancing, SSL termination, and name-based virtual hosting.
* 
* is a **reverse proxy** cause it forwards client traffic to the server

NGINX would flow like this through Kube-Proxy:
```
[External User] 🌍 --> [Ingress Controller (L7 routing)] --> [kube-proxy (L4 routing)] --> [Service] --> [Pods]
```
but if
1. The Ingress Controller uses an internal load balancing mechanism (e.g., AWS ALB, Traefik, Istio Gateway).
2. It is configured in "IP mode" (e.g., MetalLB, direct pod routing).
it will flow like this:
```
[External User] 🌍  → [AWS ALB Ingress Controller (L7)] → [Service (NodePort)] → [Pods]
```
### Kubernetes Manifest
* A Kubernetes manifest is a YAML (or JSON) configuration file that describes the desired state of various resources in a Kubernetes cluster.
* Also called creating a "deployment"
* tools like kubectl send the manifest's contents to the API server (on the control plane), which then processes and applies the desired configuration.

### Ectd
* holds the state data (like **terraform.tfstate**) so it knows what to apply to the cluster based on the manifest file

### Statful Set (sts)
<img width="460" alt="Screenshot 2025-02-20 at 9 40 52 AM" src="https://github.com/user-attachments/assets/0a4aa8a6-e9be-4213-bc7e-e874136b6d1b" />

* when we do deployements, we cant just copy databases since they need to share data
* Stateful Set allows us to **copy stateful resources**

### Minikube
* used for local testing
* runs control planes and nodes on one node
  
### Workflow
  <img width="558" alt="Screenshot 2025-02-19 at 2 59 05 PM" src="https://github.com/user-attachments/assets/563ee0ea-e06f-4bec-9ea9-6e629c041f56" />

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

### Kubectl (command line interaction)
<img width="765" alt="Screenshot 2025-02-19 at 3 22 38 PM" src="https://github.com/user-attachments/assets/327257aa-394b-4d38-a7b8-6c2792c91ea8" />

### Kube-proxy
* Service-to-Pod communication via ClusterIP.
* Load balancing across multiple Pod replicas.
* Uses iptables or IPVS rules to route traffic.
* For a Service with multiple Pods, kube-proxy evenly distributes incoming requests.
* Ensures cross-node Pod communication is seamless.

<img width="795" alt="Screenshot 2025-02-19 at 3 20 13 PM" src="https://github.com/user-attachments/assets/532677ce-1a08-4d5d-b362-cd3daa1aabda" />

<img width="774" alt="Screenshot 2025-02-19 at 3 18 56 PM" src="https://github.com/user-attachments/assets/2bd404e3-2f01-4a27-b87d-fc6a5c918dd3" />

<img width="792" alt="Screenshot 2025-02-19 at 3 20 31 PM" src="https://github.com/user-attachments/assets/f2f07d09-b3dd-4379-80c2-9118e54c4bd8" />

### HA-Proxy

![image](https://github.com/user-attachments/assets/18ec4499-7b58-4049-9259-23b416e4323a)

#1 is definitely the most common setup however

<img width="774" alt="Screenshot 2025-03-14 at 11 54 53 AM" src="https://github.com/user-attachments/assets/40c4fcf6-8506-4372-afac-fd522efbfc34" />

<img width="782" alt="Screenshot 2025-03-14 at 11 57 34 AM" src="https://github.com/user-attachments/assets/075fbcbf-c4a7-48da-bf95-719fc76b9344" />


HA proxy is way more likely to be a replacement for the AWS ALB (or any cloud load balancer) rather than replacing the Ingress Controller like NGINX or Kube-proxy (while techincally still possible). 

**Cloud loadbalancer is L7 and directs traffict TO KUBERNETES, Ingress controller directs traffic TO SERVICES (through kube-proxy usually, but not always)**

<img width="764" alt="Screenshot 2025-02-22 at 11 32 22 AM" src="https://github.com/user-attachments/assets/cf9a48ee-e8a1-4932-9a27-5b54129b6c96" />

So when you use it you get, access to protocols like websocket, gRPC, and HTTP2. You get more security and routing flexibility. It's also faster.

## To be clear, other cloud loudbalances (AWS ALB) can also use non-standard protocols



**gRPC**

<img width="762" alt="Screenshot 2025-02-22 at 11 15 00 AM" src="https://github.com/user-attachments/assets/73d1a947-7f27-45f5-b2d2-dc7be8fe9dcb" />
<img width="818" alt="Screenshot 2025-02-22 at 11 17 25 AM" src="https://github.com/user-attachments/assets/d6a5033b-651b-4d0b-9439-e0185915c80c" />
<img width="796" alt="Screenshot 2025-02-22 at 11 17 42 AM" src="https://github.com/user-attachments/assets/4495f3e5-3704-40a9-9ab8-a49a20f4e0f9" />

HA-proxy can handle DIFFERENT PROTOCOLS, that's the appeal

<img width="796" alt="Screenshot 2025-02-22 at 11 13 02 AM" src="https://github.com/user-attachments/assets/48df6afa-610e-41c8-aa3f-b9b039cda695" />

* It's like Kubernetes Ingress, but can do **SSL(TLS) Passthrough** sending unencrypted data in the system, this is not default
* Not in Kuberenetes by default
* 🌐 HAProxy is used as an Ingress controller for external HTTP(S) traffic entering the cluster.
* Handles incoming traffic (service mesh is internal traffic)
* also occurs at Layer 7/Http/https (Kube-proxy only does Layer 4/TCP/UDP routing by default)
* Can perform **content switching**, meaning it can direct traffic to different backends based on HTTP headers, method type (GET/POST), cookies, URLs, and even the body/payload of the request.
  
<img width="711" alt="Screenshot 2025-02-19 at 4 02 32 PM" src="https://github.com/user-attachments/assets/accd8a3b-a042-4f76-a80d-0ae5954bdde2" />

<img width="791" alt="Screenshot 2025-02-19 at 6 37 59 PM" src="https://github.com/user-attachments/assets/9bd5729e-dea7-4dab-b424-8ccb61fa349c" />

<img width="769" alt="Screenshot 2025-02-20 at 9 48 58 AM" src="https://github.com/user-attachments/assets/520a54dd-93a8-434c-887a-1a7a77bcce77" />

#### HA-Proxy vs Service Mesh
* HA-Proxy does more traffic direction from external
* Service mesh is internal to internal
  
<img width="775" alt="Screenshot 2025-02-21 at 2 40 36 PM" src="https://github.com/user-attachments/assets/3783656b-519f-4ebe-942d-a137e8745845" />

### Service Mesh


* The service mesh is typically implemented as a scalable set of network proxies deployed alongside application code (a pattern sometimes called a **sidecar**). These proxies handle the communication between the microservices and also act as a point at which the service mesh features can be introduced. The proxies comprise the service mesh’s data plane, and are controlled as a whole by its control plane.

<img width="769" alt="Screenshot 2025-02-19 at 6 24 22 PM" src="https://github.com/user-attachments/assets/279cc53d-e65e-436d-8415-071b59aaa5af" />

* **Istio** or **Linkerd** (“linker-dee”) for example
*  No, a Service Mesh is **not included by default** in Kubernetes.
*  Kubernetes provides basic networking (via Services, kube-proxy, and CNI plugins) for Pod-to-Pod communication, service discovery, and basic load balancing.
*  However, Service Meshes (like Istio, Linkerd, Consul) provide advanced traffic management and observability features that Kubernetes does not provide by default.
*  A service mesh like Linkerd is a tool for adding observability, security, and reliability
*  The service mesh is typically implemented as a scalable set of network proxies deployed alongside application code (a pattern sometimes called a sidecar).
*  Allows for things like **Canary** and **Red Green** deployments
*  
#### So, How Are New Pods Started Without a Rolling Deployment?
1️⃣ Pre-Provisioning New Pods (Manual or Automated)
You manually or via CI/CD pipelines deploy new pods (v2) alongside the existing stable pods (v1) using:

A separate Deployment for the new version (my-app-v2), or
Additional replicas in the same Deployment but with a different label (e.g., version: v2).

  
<img width="785" alt="Screenshot 2025-02-19 at 6 45 35 PM" src="https://github.com/user-attachments/assets/69ad9141-2d4a-4a08-a324-e4fe96aa0876" />

* ✅ Ensures that only trusted services can talk to each other.
* ✅ Provides zero-trust networking by verifying every request.
* ✅ Protects against Man-in-the-middle attacks, spoofed services.
* Without mTLS: A malicious service could pretend to be payment-service and intercept transactions because Kubernetes networking by default trusts internal traffic.
* Yes, if you care about end-to-end encryption inside the cluster, mTLS becomes essential after TLS termination. Because TLS termination causes unencrypted traffic inside your kubernetes cluster

* Key Point:
CNI plugins ensure packets get delivered from one pod to another. However, they don’t understand the application-layer protocols (like HTTP, gRPC) and don’t provide advanced traffic control or observability.

Yes. A CNI (Container Network Interface) plugin is required for a service mesh to function in Kubernetes because the service mesh depends on basic pod-to-pod network connectivity provided by the CNI.

* CNI plugin ensures Pod A can reach Pod B at the network (IP) level.
* Service mesh sidecar proxies (like Envoy in Istio or linkerd-proxy in Linkerd) intercept the traffic:
* Outbound traffic: The sidecar in Pod A applies routing rules, encrypts the request (mTLS), and forwards it.
* Inbound traffic: The sidecar in Pod B decrypts the request, applies authentication policies, and passes it to the application container.
* The service mesh control plane (e.g., Istiod in Istio) dynamically updates these proxies with configuration, policies, and routing rules.
  
#### What Service Mesh provides
* HTTPS uses TLS (Transport Layer Security) to encrypt packets and send data securely over the internet. However Service Mesh allows for **mTLS** or Mutual TLS. mTLS encrypts traffic between all services. Without a service mesh you need to **manually implement TLS** for all services. this is **END TO END ENCRYPTION**
* Layer 7 (Application Layer) routing means the service mesh understands application protocols like HTTP, gRPC, and TCP, allowing for intelligent traffic control based on application-level information.
* ⚡ Advanced Traffic Control **(A/B Testing, Canary Releases, Blue-Green Deployments)**. By default Kubernetes **only provides rolling updates**
* 💥 Resilience (Retries, Timeouts, Circuit Breakers): **Kubernetes cannot handle: Automatic retries when a service call fails**. Timeouts on service-to-service communication. Circuit breakers to stop calling a failing service (prevent cascading failures).
* 🔍 Observability (Distributed Tracing, Metrics, Logging): **Service to Service logs** via Sidecar proxies (e.g., Envoy) automatically collect telemetry.
  
<img width="755" alt="Screenshot 2025-02-19 at 3 57 55 PM" src="https://github.com/user-attachments/assets/1140191e-0761-4024-9732-2050d88d0349" />

<img width="627" alt="Screenshot 2025-02-19 at 3 59 03 PM" src="https://github.com/user-attachments/assets/a56fe559-84a5-4498-8653-c21effe319b4" />

<img width="730" alt="Screenshot 2025-02-19 at 3 59 27 PM" src="https://github.com/user-attachments/assets/0bbd5e3f-e947-4df0-a53e-4e5e0baca896" />

#### L7 Loadbalancers (Service Mesh and HA-Proxy)

* **TLS termination** refers to the process of decrypting encrypted traffic (using the Secure Sockets Layer protocol) before it reaches a web server, typically done by a load balancer or dedicated device, which then forwards the decrypted data to the server,

<img width="699" alt="Screenshot 2025-02-19 at 6 48 31 PM" src="https://github.com/user-attachments/assets/f9f5c356-de70-49d3-9096-ce011bcd0ab8" />

<img width="780" alt="Screenshot 2025-02-19 at 6 50 05 PM" src="https://github.com/user-attachments/assets/4230a110-3b34-45a6-911e-16698b27c1b8" />

<img width="763" alt="Screenshot 2025-02-19 at 6 52 23 PM" src="https://github.com/user-attachments/assets/c7530a38-1de0-46ea-814e-d4e61604c6bd" />

<img width="764" alt="Screenshot 2025-02-19 at 7 01 31 PM" src="https://github.com/user-attachments/assets/e2467437-cbbf-492a-ab20-2ef99f89521b" />

In Kubernetes, the edge is typically represented by:
* Ingress controllers (like HAProxy, NGINX, or Traefik).
* Load balancers provided by cloud platforms (e.g., AWS ALB, GCP Load Balancer).
* NodePort or LoadBalancer services when they expose cluster resources.

```
[External Client] 🌍 --(HTTPS)--> [HAProxy Ingress Controller 🌐] --(HTTP)--> [K8s Service 🏛️] --> [Pod 🐳]
```

<img width="761" alt="Screenshot 2025-02-19 at 7 11 14 PM" src="https://github.com/user-attachments/assets/407c41b5-c6c2-49c4-b211-22f787608089" />
* ⚡ HAProxy: Best for high-performance, low-latency requirements (TCP-heavy apps).

### Container Network Interface Plugins
* The CNI gives pods unique IPs so **pods can communicate directly** across nodes
* Pod-to-Pod communication **across nodes** won’t work without a **CNI plugin**, but will work within nodes by default across the local network in the node
* 🔗 5. Do We Have Unique Pod IPs by Default?
* ✅ Yes, if your Kubernetes installation includes a CNI plugin.
* **Managed Kubernetes platforms (EKS, AKS, GKE) provide this by default.**
* For kubeadm or bare-metal, you must install a CNI (e.g., Calico, Flannel).
  
<img width="768" alt="Screenshot 2025-02-19 at 7 06 28 PM" src="https://github.com/user-attachments/assets/410ede7e-9227-4f08-81f2-6797ee77d3d7" />

<img width="764" alt="Screenshot 2025-02-19 at 4 04 04 PM" src="https://github.com/user-attachments/assets/4bc2b284-833c-4d44-bca3-56e0f7e57fe3" />

## RBAC
* Kubernetes has RBAC that can be applied for users, but AWS EKS replaces this with IAM users
* 
<img width="532" alt="Screenshot 2025-02-19 at 10 02 04 PM" src="https://github.com/user-attachments/assets/cecd264e-3259-4e18-ba99-da74f89d30dc" />



* 
### Docker Swarm

In Docker Swarm, the architecture is similarly structured for orchestrating containerized applications. The Swarm manager is responsible for the overall management of the cluster, including scheduling tasks and maintaining the desired state. The worker nodes execute the tasks assigned to them by the manager, running these tasks within Docker containers. This ensures a distributed and resilient deployment of applications across the cluster.

## Ansible
<img width="1116" alt="Screenshot 2024-07-07 at 12 21 00 PM" src="https://github.com/mfkimbell/ci-cd-notes/assets/107063397/98d71dce-94bb-4137-a5e2-3696cfa939cd">

Ansible also uses a `Declarative Language`.

* **Configuration Management:** Ansible is primarily a configuration management tool. It is designed to automate the provisioning and configuration of servers and software.
* **Agentless:** Ansible is agentless, meaning it doesn't require any software to be installed on the target machines. It communicates with them using SSH.'
* **SSH ON STEROIDS**
* No requirement for ansible agent on recieving VM

* 🌿 Ansible ≠ Just SSH + Shell Scripts:
<img width="661" alt="Screenshot 2025-02-19 at 9 58 49 PM" src="https://github.com/user-attachments/assets/499d124f-9b8b-4605-bb35-a8cc1d4c0280" />



