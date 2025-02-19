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

## Ansible

<img width="1413" alt="334571210-01702314-8e57-46f4-92de-0aa6e9e26cbe" src="https://github.com/user-attachments/assets/efd53885-fb53-4bea-82a0-33c23e84af90" />

#### Ansible Ansible also uses a `Declarative Language`.

Configuration Management: Ansible is primarily a configuration management tool. It is designed to automate the provisioning and configuration of servers and software.
Agentless: Ansible is agentless, meaning it doesn't require any software to be installed on the target machines. It communicates with them using SSH.'
SSH ON STEROIDS
ssh on steroids
No requirement for ansible agent on recieving VM
