# AWS EKS NGINX Deployment using Kubernetes LoadBalancer

This guide walks through the complete installation and deployment process for creating an Amazon EKS cluster and deploying an NGINX application exposed through a Kubernetes LoadBalancer Service.

---

# Table of Contents

* Prerequisites
* Launch Ubuntu EC2 Instance
* Connect to EC2
* Update Ubuntu
* Install AWS CLI
* Configure AWS CLI
* Install kubectl
* Install eksctl
* Verify Installation
* Create Amazon EKS Cluster
* Configure kubectl
* Verify Cluster
* Next Steps

---

# Prerequisites

Before starting this project, make sure you have the following:

* AWS Account
* IAM User with Amazon EKS permissions
* AWS Access Key
* AWS Secret Access Key
* EC2 Key Pair
* Internet Connection

---

# Test Environment

This project was tested using the following environment.

| Component        | Value                   |
| ---------------- | ----------------------- |
| Operating System | Ubuntu Server 24.04 LTS |
| Cloud Provider   | AWS                     |
| Region           | eu-north-1              |
| Cluster Name     | my-cluster              |
| Worker Node Type | c7i-flex.large          |

---

# Step 1 - Launch Ubuntu EC2 Instance

Login to the AWS Console.

Navigate to:

```
EC2 → Launch Instance
```

Configure the instance as follows.

| Setting        | Value                   |
| -------------- | ----------------------- |
| AMI            | Ubuntu Server 24.04 LTS |
| Instance Type  | c7i-flex.large          |
| Storage        | 20 GB                   |
| Key Pair       | Your existing key pair  |
| Security Group | Create new              |

---

## Security Group Rules

Allow the following inbound rule.

| Type | Port | Source |
| ---- | ---- | ------ |
| SSH  | 22   | My IP  |

Click **Launch Instance**.

Wait until the instance state changes to **Running**.

---

# Step 2 - Connect to the EC2 Instance

Open your terminal.

Connect using SSH.

```bash
ssh -i your-key.pem ubuntu@<EC2-Public-IP>
```

Example

```bash
ssh -i mykey.pem ubuntu@13.xxx.xxx.xxx
```

If the connection is successful, you will be logged in to the Ubuntu server.

---

# Step 3 - Update Ubuntu Packages

Update the package index.

```bash
sudo apt update
```

Upgrade installed packages.

```bash
sudo apt upgrade -y
```

Keeping packages updated ensures compatibility with the latest software.

---

# Step 4 - Install AWS CLI

AWS CLI is used to communicate with AWS services such as Amazon EKS.

Install unzip.

```bash
sudo apt install unzip
```

Download AWS CLI.

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

Extract the installer.

```bash
unzip awscliv2.zip
```

Install AWS CLI.

```bash
sudo ./aws/install
```

Verify installation.

```bash
aws --version
```

Expected output

```text
aws-cli/2.x.x
```

---

# Step 5 - Configure AWS CLI

Run

```bash
aws configure
```

Provide the following details.

```
AWS Access Key ID

AWS Secret Access Key

Default Region
```

Use

```
eu-north-1
```

Output format

```
json
```

Verify configuration.

```bash
aws sts get-caller-identity
```

If successful, your AWS Account ID and IAM User ARN will be displayed.

---

# Step 6 - Install kubectl

kubectl is the Kubernetes command-line tool used to manage the EKS cluster.

Download kubectl.

```bash
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
```

Make it executable.

```bash
chmod +x ./kubectl
```

Move it to the system path.

```bash
sudo mv ./kubectl /usr/local/bin
```

Verify installation.

```bash
kubectl version --short --client
```

Expected output

```
Client Version: v1.xx.x
```

---

# Step 7 - Install eksctl

eksctl is the official command-line utility used to create Amazon EKS clusters.

Download eksctl.

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
```

Move it to the system path.

```bash
sudo mv /tmp/eksctl /usr/local/bin
```

Verify installation.

```bash
eksctl version
```

Expected output

```
0.xxx.x
```

---

# Step 8 - Verify Installed Tools

Verify AWS CLI.

```bash
aws --version
```

Verify kubectl.

```bash
kubectl version --short --client
```

Verify eksctl.

```bash
eksctl version
```

All three commands should execute without errors.

---

# Step 9 - Create the Amazon EKS Cluster

Run the following command.

```bash
eksctl create cluster --name my-cluster --region eu-north-1 --node-type c7i-flex.large --zones eu-north-1a,eu-north-1b
```

This command will:

* Create the Amazon EKS control plane
* Create the worker nodes
* Configure networking
* Create the required IAM roles
* Configure VPC resources

Cluster creation typically takes **15–20 minutes**.

Do not interrupt the process.

---

# Step 10 - Configure kubectl

Once the cluster is created, update the local kubeconfig.

```bash
aws eks update-kubeconfig \
--region eu-north-1 \
--name my-cluster
```

This command allows kubectl to communicate with your Amazon EKS cluster.

---

# Step 11 - Verify the Cluster

Verify the worker nodes.

```bash
kubectl get nodes
```

Expected output

```text
NAME                                           STATUS   ROLES    AGE
ip-192-168-xx-xx.eu-north-1.compute.internal   Ready    <none>   xxm
ip-192-168-xx-xx.eu-north-1.compute.internal   Ready    <none>   xxm
```

If both nodes display the **Ready** status, the cluster has been created successfully.

---

# Step 12 - Create the Kubernetes Deployment

The Deployment is responsible for creating and managing the NGINX Pods. Kubernetes ensures the desired number of replicas are always running.

Create a file named **deployment.yaml**.

```bash
nano deployment.yaml
```

Paste the following YAML.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest

        ports:
        - containerPort: 80
```

Save the file.

Apply the Deployment.

```bash
kubectl apply -f deployment.yaml
```

Expected Output

```text
deployment.apps/nginx-deployment created
```

---

# Step 13 - Verify the Deployment

Check the Deployment.

```bash
kubectl get deployments
```

Expected Output

```text
NAME               READY   UP-TO-DATE   AVAILABLE
nginx-deployment   2/2     2            2
```

Check the Pods.

```bash
kubectl get pods
```

Expected Output

```text
NAME                                READY   STATUS    RESTARTS
nginx-deployment-xxxxxxxxxx-xxxxx   1/1     Running   0
nginx-deployment-xxxxxxxxxx-yyyyy   1/1     Running   0
```

View detailed information.

```bash
kubectl describe deployment nginx-deployment
```

---

# Step 14 - Create the Kubernetes Service

A Service provides network access to the Pods.

Create a file named **service.yaml**.

```bash
nano service.yaml
```

Paste the following YAML.

```yaml
apiVersion: v1
kind: Service

metadata:
  name: nginx-service

spec:
  selector:
    app: nginx

  ports:
  - port: 80
    targetPort: 80

  type: LoadBalancer
```

Save the file.

Deploy the Service.

```bash
kubectl apply -f service.yaml
```

Expected Output

```text
service/nginx-service created
```

---

# Step 15 - Verify the Service

Check the Service.

```bash
kubectl get svc
```

Example Output

```text
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP                                         PORT(S)
kubernetes      ClusterIP      10.100.0.1      <none>                                              443/TCP
nginx-service   LoadBalancer   10.100.100.235  xxxxxxxxxxxxxxxxx.eu-north-1.elb.amazonaws.com      80:30882/TCP
```

Wait a few minutes until the **EXTERNAL-IP** (AWS ELB DNS name) is assigned.

---

# Step 16 - Verify Endpoints

Ensure the Service has discovered the Pods.

```bash
kubectl get endpoints nginx-service
```

Expected Output

```text
NAME            ENDPOINTS
nginx-service   192.168.xx.xx:80,192.168.xx.xx:80
```

If the ENDPOINTS column is empty, verify that:

* The Pods are running.
* The labels in the Deployment match the Service selector.

---

# Step 17 - Test the Application

Retrieve the LoadBalancer DNS name.

```bash
kubectl get svc
```

Example

```text
xxxxxxxxxxxxxxxxx.eu-north-1.elb.amazonaws.com
```

Open the DNS name in your web browser.

```text
http://xxxxxxxxxxxxxxxxx.eu-north-1.elb.amazonaws.com
```

Expected Result

```text
Welcome to nginx!
```

Congratulations! Your NGINX application is now accessible from the internet through an AWS Load Balancer.

---

# Step 18 - Verify Resources

Check all Kubernetes resources.

View Nodes

```bash
kubectl get nodes
```

View Deployments

```bash
kubectl get deployments
```

View Pods

```bash
kubectl get pods
```

View Services

```bash
kubectl get svc
```

View Endpoints

```bash
kubectl get endpoints
```

Describe the Service

```bash
kubectl describe svc nginx-service
```

Describe the Deployment

```bash
kubectl describe deployment nginx-deployment
```

View Pod logs

```bash
kubectl logs <pod-name>
```

---

# Step 19 - Troubleshooting

## LoadBalancer remains in Pending

Possible Causes

* AWS is still provisioning the LoadBalancer.
* Worker nodes are not Ready.
* Insufficient IAM permissions.

Check

```bash
kubectl get svc
```

---

## Browser shows ERR_TIMED_OUT

Verify the Service.

```bash
kubectl get svc
```

Verify the Endpoints.

```bash
kubectl get endpoints nginx-service
```

Verify the Pods.

```bash
kubectl get pods
```

Test from inside the worker node.

```bash
curl http://localhost:<NodePort>
```

Check the AWS Load Balancer status in the AWS Console.

---

## Pods are not Running

Describe the Pod.

```bash
kubectl describe pod <pod-name>
```

View logs.

```bash
kubectl logs <pod-name>
```

---

## Service has no Endpoints

Ensure the labels match.

Deployment

```yaml
labels:
  app: nginx
```

Service Selector

```yaml
selector:
  app: nginx
```

The labels must match exactly.

---

# Step 20 - Cleanup

Delete the Service.

```bash
kubectl delete -f service.yaml
```

Delete the Deployment.

```bash
kubectl delete -f deployment.yaml
```

Delete the EKS Cluster.

```bash
eksctl delete cluster \
--name my-cluster \
--region eu-north-1
```

Deleting the cluster also removes the worker nodes and associated AWS resources created by eksctl.

---

# Installation Complete

You have successfully completed the following tasks:

* Installed AWS CLI
* Installed kubectl
* Installed eksctl
* Configured AWS credentials
* Created an Amazon EKS cluster
* Connected kubectl to the cluster
* Deployed an NGINX application
* Exposed the application using a Kubernetes LoadBalancer Service
* Verified the deployment
* Accessed the application through an AWS Classic Load Balancer

Your Amazon EKS cluster is now fully operational and ready for deploying more Kubernetes applications.
