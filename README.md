# aks-simple-webapp-deployment
Deploying a Simple Web Application on Azure Kubernetes Service (AKS)
🚀 Deploying a Simple Web Application on Azure Kubernetes Service (AKS)
📌 Task Description

Task Assigned:
Deploying a Simple Web Application on Azure Kubernetes Service (AKS)

The task involves creating all required Azure resources from scratch, containerizing a simple web application using Docker, storing the image in Azure Container Registry (ACR), deploying the application on Azure Kubernetes Service (AKS) using Kubernetes manifests, and exposing the application to the internet using a Kubernetes LoadBalancer service.

The objective of this project is to gain hands-on understanding of:

Containerization using Docker

Kubernetes core components

Azure-managed Kubernetes services

Real-world AKS deployment workflow

🧩 What You Will Build

You will deploy:

A simple web application (NGINX with sample HTML)

Packaged as a Docker container

Image stored in Azure Container Registry (ACR)

Application deployed on Azure Kubernetes Service (AKS)

Application exposed publicly using Azure LoadBalancer

🧠 High-Level Architecture (Big Picture)
User Browser
     ↓
Azure Load Balancer (Kubernetes Service)
     ↓
AKS Pods (NGINX Containers)
     ↓
Azure Container Registry (Docker Image)

🧱 Prerequisites (And Why They Are Needed)
Item	Purpose
Azure Subscription	To create and manage cloud resources
Azure Portal	GUI-based AKS creation
Azure CLI	Command-line interaction with Azure
Docker	To containerize the application
kubectl	To manage Kubernetes resources
Azure Cloud Shell	Avoids local setup issues
🛠️ Resource Names Used
Resource	Name
Resource Group	aks-rg
AKS Cluster	aks-cluster
Azure Container Registry	aksacrmsdn4xyz
Deployment	aks-webapp-deployment
Service	aks-webapp-service
🔹 STEP 1: Login to Azure
az login

Purpose

Authenticates the user with Azure

Allows creation of Azure resources in the subscription

🔹 STEP 2: Create Resource Group
az group create --name aks-rg --location eastus

Purpose

Resource Group acts as a logical container

Helps in organization, billing, and cleanup

Deleting the resource group deletes all resources inside it

🔹 STEP 3: Create Azure Container Registry (ACR)
az acr create --resource-group aks-rg --name aksacrmsdn4xyz --sku Basic

Purpose

Stores Docker images securely

AKS pulls application images from ACR

More secure than public registries like Docker Hub

🔹 STEP 4: Create Simple Web Application
mkdir aks-webapp
cd aks-webapp

Create index.html
<!DOCTYPE html>
<html>
<head>
  <title>AKS Web App</title>
</head>
<body>
  <h1>Hello from Azure Kubernetes Service!</h1>
</body>
</html>

Purpose

Simple application to focus on AKS concepts

Avoids unnecessary coding complexity

🔹 STEP 5: Create Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html

Purpose

Uses NGINX web server

Packages the application into a portable container

🔹 STEP 6: Build Docker Image
docker build -t aks-webapp:v1 .

Purpose

Converts the application into a Docker image

Image acts as a blueprint to run containers anywhere

🔹 STEP 7: Push Image to Azure Container Registry
Login to ACR
az acr login --name aksacrmsdn4xyz

Tag Image
docker tag aks-webapp:v1 aksacrmsdn4xyz.azurecr.io/aks-webapp:v1

Push Image
docker push aksacrmsdn4xyz.azurecr.io/aks-webapp:v1

Purpose

AKS cannot access local images

Image must be stored in a cloud registry

Enables AKS to pull the application image securely

🔹 STEP 8: Create AKS Cluster (Azure Portal)
Steps (GUI)

Azure Portal → Kubernetes services → Create

Resource Group: aks-rg

Cluster name: aks-cluster

Node count: 2

Authentication: Local accounts with Kubernetes RBAC

Node security channel: Node Image

Attach ACR: aksacrmsdn4xyz

Purpose

AKS provides managed Kubernetes

Node Image ensures automatic security patching

Attaching ACR allows AKS to pull private images

🔹 STEP 9: Connect to AKS Cluster
az aks get-credentials --resource-group aks-rg --name aks-cluster

Purpose

Downloads cluster credentials

Configures kubectl to manage AKS

🔹 STEP 10: Verify AKS Cluster
kubectl get nodes

Purpose

Confirms cluster health

Nodes should be in Ready state

🔹 STEP 11: Create Kubernetes Deployment
deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-webapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: aks-webapp
  template:
    metadata:
      labels:
        app: aks-webapp
    spec:
      containers:
      - name: aks-webapp
        image: aksacrmsdn4xyz.azurecr.io/aks-webapp:v1
        ports:
        - containerPort: 80

kubectl apply -f deployment.yaml

Purpose

Deployment manages Pods

Ensures high availability and self-healing

🔹 STEP 12: Expose Application Using Service
service.yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-webapp-service
spec:
  type: LoadBalancer
  selector:
    app: aks-webapp
  ports:
  - port: 80
    targetPort: 80

kubectl apply -f service.yaml

Purpose

Creates Azure Load Balancer

Assigns a public IP

Routes traffic to Pods

🔹 STEP 13: Access the Application
kubectl get service aks-webapp-service


Open in browser:

http://<EXTERNAL-IP>


🎉 Application is live on AKS!

🔐 Security & Best Practices

Use ACR instead of public registries

Use multiple replicas for high availability

AKS provides self-healing

Azure Load Balancer ensures scalability

Can be extended with:

Ingress Controller

HTTPS

Autoscaling (HPA)

CI/CD pipelines

🧹 Cleanup (IMPORTANT)
az group delete --name aks-rg --yes --no-wait

Purpose

Deletes all Azure resources

Prevents unwanted billing
👤 Author

Name: Sameeksha Y S
Role: Cloud / DevOps Learner
Focus Areas: Azure, Kubernetes (AKS), Docker, Cloud Computing

📚 Acknowledgements

Microsoft Azure Documentation

Kubernetes Official Documentation

Azure Cloud Shell and Azure Portal

Online learning resources and labs used for understanding AKS concepts

📌 Notes

This project is created for learning and demonstration purposes.

The deployment uses basic and recommended configurations suitable for beginners.

For production environments, additional security, monitoring, and scalability configurations are required.

🔮 Future Enhancements

Add Ingress Controller with HTTPS

Enable Horizontal Pod Autoscaling (HPA)

Implement CI/CD pipeline using Azure DevOps or GitHub Actions

Integrate Azure Monitor and Log Analytics

Apply security best practices such as network policies and secrets management

📝 License

This project is licensed for educational use only.
You are free to use, modify, and extend this project for learning purposes.

📬 Contact

GitHub: https://github.com/12-sam-git

LinkedIn: https://www.linkedin.com/in/sameeksha-ys-02s18
