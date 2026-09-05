# 🚀 AWS EKS NGINX Deployment using Kubernetes LoadBalancer

Deploy a highly available **NGINX** application on **Amazon Elastic Kubernetes Service (EKS)** and expose it to the internet using a **Kubernetes Service of type `LoadBalancer`**, which automatically provisions an **AWS Classic Load Balancer**.

---

# 📖 Project Overview

This project demonstrates how to deploy an NGINX application on an Amazon EKS cluster using Kubernetes. It covers the complete deployment workflow, from launching an Ubuntu EC2 instance and creating an EKS cluster to deploying an application and exposing it to the internet through an AWS Load Balancer.

The repository is intended for beginners and aspiring DevOps Engineers who want hands-on experience with Kubernetes on AWS while following production-style documentation.

---

# 🎯 Project Objectives

* Launch an Ubuntu EC2 instance
* Install AWS CLI
* Install kubectl
* Install eksctl
* Configure AWS credentials
* Create an Amazon EKS cluster
* Connect kubectl to the cluster
* Deploy an NGINX application
* Expose the application using a Kubernetes LoadBalancer Service
* Verify the deployment
* Access the application through the AWS Load Balancer
* Learn basic troubleshooting techniques

---

# 🏗️ Architecture

```text
                    Internet
                        │
                        ▼
          AWS Classic Load Balancer
                        │
                        ▼
        Kubernetes Service (LoadBalancer)
                        │
            ┌───────────┴───────────┐
            ▼                       ▼
      NGINX Pod 1             NGINX Pod 2
                        │
                        ▼
             Amazon EKS Worker Nodes
                        │
                        ▼
          Amazon EKS Control Plane
```

---

# 🛠 Technologies Used

| Technology | Purpose                              |
| ---------- | ------------------------------------ |
| AWS EC2    | Ubuntu server for cluster management |
| Amazon EKS | Managed Kubernetes Cluster           |
| Kubernetes | Container orchestration              |
| eksctl     | EKS cluster creation                 |
| kubectl    | Kubernetes management                |
| AWS CLI    | AWS authentication and management    |
| Docker     | Container image platform             |
| NGINX      | Sample web application               |

---

# 📁 Repository Structure

```text
aws-eks-nginx-loadbalancer/

├── README.md
├── INSTALLATION.md
├── ARCHITECTURE.md
├── TROUBLESHOOTING.md
├── INTERVIEW-QUESTIONS.md
├── LICENSE
├── .gitignore
│
├── kubernetes/
│   ├── deployment.yaml
│   └── service.yaml
│
├── diagrams/
│   └── architecture.png
│
└── screenshots/
    ├── 01-launch-ec2.png
    ├── 02-install-awscli.png
    ├── 03-install-kubectl.png
    ├── 04-install-eksctl.png
    ├── 05-create-cluster.png
    ├── 06-get-nodes.png
    ├── 07-get-pods.png
    ├── 08-get-service.png
    ├── 09-load-balancer.png
    └── 10-nginx-homepage.png
```

---

# ⚙️ Prerequisites

Before starting this project, ensure you have:

* An AWS Account
* An IAM User with appropriate permissions for EKS
* An Ubuntu EC2 instance
* Internet connectivity
* An SSH key pair for connecting to EC2

---

# 🚀 Quick Start

Clone the repository:

```bash
git clone https://github.com/<your-username>/aws-eks-nginx-loadbalancer.git

cd aws-eks-nginx-loadbalancer
```

Follow the complete installation guide:

📄 **INSTALLATION.md**

---

# 📋 Deployment Workflow

1. Launch an Ubuntu EC2 instance
2. Install AWS CLI
3. Configure AWS CLI
4. Install kubectl
5. Install eksctl
6. Create an Amazon EKS cluster
7. Configure kubectl
8. Deploy NGINX
9. Create a Kubernetes LoadBalancer Service
10. Verify resources
11. Access the application

---

# ✅ Verification Commands

Check Nodes

```bash
kubectl get nodes
```

Check Deployments

```bash
kubectl get deployments
```

Check Pods

```bash
kubectl get pods
```

Check Services

```bash
kubectl get svc
```

Describe Service

```bash
kubectl describe svc nginx-service
```

Check Endpoints

```bash
kubectl get endpoints nginx-service
```

---

# 🌐 Access the Application

After the LoadBalancer is provisioned, obtain the external DNS:

```bash
kubectl get svc
```

Example:

```text
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP
nginx-service   LoadBalancer   10.xxx.xxx.xxx  xxxxxxxxx.elb.amazonaws.com
```

Open the DNS name in your browser:

```text
http://<LoadBalancer-DNS>
```

Expected Output:

```text
Welcome to nginx!
```

---

# 📸 Screenshots

This repository includes screenshots for every important step:

* EC2 Instance
* AWS CLI Installation
* kubectl Installation
* eksctl Installation
* EKS Cluster Creation
* Kubernetes Nodes
* Deployments
* Pods
* Services
* AWS Load Balancer
* NGINX Welcome Page

---

# 🛠 Troubleshooting

Common issues covered in this project:

* LoadBalancer stuck in Pending
* ERR_TIMED_OUT while accessing the application
* Missing Kubernetes Endpoints
* NodePort verification
* Security Group configuration
* AWS CLI authentication issues
* kubectl connection problems

Refer to **TROUBLESHOOTING.md** for detailed solutions.

---

# 📚 Learning Outcomes

After completing this project, you will understand:

* Amazon EKS architecture
* Kubernetes Deployments
* ReplicaSets
* Kubernetes Services
* LoadBalancer Services
* NodePort networking
* kubectl administration
* AWS Load Balancer integration
* Kubernetes troubleshooting

---

# 🚀 Future Enhancements

* Deploy a custom web application
* Configure AWS Application Load Balancer (ALB) using Ingress
* Package the application using Helm
* Implement Horizontal Pod Autoscaler (HPA)
* Integrate Prometheus and Grafana
* Build a Jenkins CI/CD pipeline
* Push container images to Amazon ECR
* Enable HTTPS using AWS Certificate Manager

---

# 👨‍💻 Author

**Vinoth V**

DevOps | AWS | Kubernetes | Docker | Terraform | Jenkins | Python
