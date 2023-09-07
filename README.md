# Deploying a Node.js Application to Azure Kubernetes Service (AKS)

This repository provides step-by-step instructions for deploying a Node.js application to Azure Kubernetes Service (AKS) using Docker and Kubernetes. Follow these guidelines to set up and deploy your application on AKS.

## Prerequisites

Before you begin, ensure that you have the following prerequisites in place:

1. **Azure Account:** You'll need an Azure account to create and manage the AKS cluster. [Create an Azure account](https://azure.microsoft.com/en-us/free/).

2. **Docker:** Install Docker on your local machine. You can download it from [Docker's official website](https://www.docker.com/get-started).

3. **Azure CLI:** Install the Azure CLI to interact with your Azure resources. You can download it from [Azure CLI installation guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

4. **Kubectl:** Install kubectl, the Kubernetes command-line tool, to manage your AKS cluster. You can find installation instructions [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## Step 1: Create a Node.js Application

1. Clone this repository to your local machine:

   ```
   bash
   git clone https://github.com/yourusername/your-repo-name.git
   cd your-repo-name
2. Create a Node.js application and Dockerize it. Replace the content of app.js with your Node.js application code.

3. Build a Docker image for your application using the provided Dockerfile:   
 
## Step 2: Push the Docker Image to a Registry
1. Push the Docker image to a container registry of your choice (e.g., Azure Container Registry, Docker Hub):
   ```
docker login <registry-url>
docker tag your-docker-image-name:latest <registry-url>/your-docker-repo:tag
docker push <registry-url>/your-docker-repo:tag

## Step 3: Set Up AKS Cluster

1. Create an AKS cluster using the Azure CLI. Replace <resource-group-name> and <aks-cluster-name> with your desired values:
   ```
   az aks create --resource-group <resource-group-name> --name <aks-cluster-name> --node-count 2 --enable-addons monitoring --generate-ssh-keys
2. Connect to your AKS cluster:
   ```
   az aks get-credentials --resource-group <resource-group-name> --name <aks-cluster-name>

## Step 4: Deploy the Application to AKS

1. kubectl apply -f deployment.yaml
  ```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs-app
spec:
  replicas: 3  # Number of replicas to run (adjust as needed)
  selector:
    matchLabels:
      app: nodejs-app
  template:
    metadata:
      labels:
        app: nodejs-app
    spec:
      containers:
        - name: nodejs-app
          image: your-docker-registry/your-docker-repo:your-tag
          ports:
            - containerPort: 3000  # Port exposed by the Node.js app

2. Deploy your Node.js application to AKS using Kubernetes manifests. You can use the provided deployment.yaml as a starting point:

## Step 5: Expose the Application

1. Kubernetes Service Configuration (service.yaml):

```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-service
spec:
  selector:
    app: nodejs-app
  ports:
    - protocol: TCP
      port: 80  # Port exposed by the service
      targetPort: 3000  # Port your Node.js app listens on
  type: LoadBalancer  # Exposes the service externally via a cloud provider's load balancer (use "ClusterIP" for internal access)


1. Create a Kubernetes Service to expose your application:
```
kubectl apply -f service.yaml



