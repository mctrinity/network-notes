# Azure Kubernetes Service (AKS) Network Setup

## Introduction
When setting up networking for Azure Kubernetes Service (AKS), you have two primary options:

1. **Azure CNI (Container Networking Interface)** - Recommended for production environments as it provides full integration with Azure Virtual Network (VNet). Each pod receives an IP address from the subnet, allowing direct communication within the VNet.
2. **Kubenet** - A simpler networking option that uses Network Address Translation (NAT). Pods receive private IPs that are translated, which conserves IP space but requires additional routing.

This guide will focus on **Azure CNI**, ensuring Kubernetes pods are fully integrated with an Azure Virtual Network.

## Ingress and Egress in AKS
Networking in AKS involves both **ingress (incoming traffic)** and **egress (outgoing traffic)**. These are essential for external access and communication between services:

- **Ingress**: Controls how external users access services running in AKS. This is typically managed using an **Ingress Controller** (e.g., NGINX Ingress Controller, Azure Application Gateway) or a **LoadBalancer Service**.
- **Egress**: Governs outbound traffic from pods to external services or the internet. This can be controlled using **NAT Gateway, Azure Firewall, or UDR (User Defined Routes)** for enhanced security and compliance.

## 1. Prerequisites
Ensure you have the following installed:
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## 2. Define Variables
```bash
RESOURCE_GROUP="myResourceGroup"
AKS_CLUSTER="myAKSCluster"
VNET_NAME="myVNet"
SUBNET_NAME="mySubnet"
LOCATION="eastus"
```

## 3. Create Resource Group
```bash
az group create --name $RESOURCE_GROUP --location $LOCATION
```

## 4. Create a Virtual Network and Subnet
```bash
az network vnet create \
  --resource-group $RESOURCE_GROUP \
  --name $VNET_NAME \
  --address-prefix 10.0.0.0/8 \
  --subnet-name $SUBNET_NAME \
  --subnet-prefix 10.240.0.0/16
```

## 5. Get Subnet ID
```bash
SUBNET_ID=$(az network vnet subnet show \
  --resource-group $RESOURCE_GROUP \
  --vnet-name $VNET_NAME \
  --name $SUBNET_NAME \
  --query id -o tsv)
```

## 6. Create AKS Cluster with Azure CNI Networking
```bash
az aks create \
  --resource-group $RESOURCE_GROUP \
  --name $AKS_CLUSTER \
  --network-plugin azure \
  --vnet-subnet-id $SUBNET_ID \
  --enable-managed-identity \
  --node-count 2
```

## 7. Configure Ingress and Egress in AKS
### **Ingress Setup using NGINX Ingress Controller**
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
Verify that the ingress controller is running:
```bash
kubectl get pods -n ingress-nginx
```
Create an ingress resource (`ingress.yaml`):
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```
Apply the ingress configuration:
```bash
kubectl apply -f ingress.yaml
```

### **Egress Setup using Azure NAT Gateway**
1. Create a **Public IP** for NAT Gateway:
```bash
az network public-ip create \
  --resource-group $RESOURCE_GROUP \
  --name myNatPublicIP \
  --sku Standard \
  --allocation-method Static
```

2. Create the **NAT Gateway**:
```bash
az network nat gateway create \
  --resource-group $RESOURCE_GROUP \
  --name myNatGateway \
  --public-ip-addresses myNatPublicIP \
  --sku Standard
```

3. Associate the NAT Gateway with the subnet:
```bash
az network vnet subnet update \
  --resource-group $RESOURCE_GROUP \
  --vnet-name $VNET_NAME \
  --name $SUBNET_NAME \
  --nat-gateway myNatGateway
```

## 8. Verify AKS Cluster
```bash
az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER
kubectl get nodes
```

## 9. Deploy a Sample Application
Create a deployment file `nginx-deployment.yaml`:
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
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

Apply the deployment:
```bash
kubectl apply -f nginx-deployment.yaml
```

## 10. Verify the Deployment
```bash
kubectl get svc
```

## 11. Test the Network Connectivity
```bash
kubectl run curlpod --image=curlimages/curl -it -- sh
curl nginx-service
```

This setup configures **Azure CNI networking**, **Ingress with NGINX**, and **Egress using Azure NAT Gateway**, allowing seamless integration with an Azure Virtual Network (VNet). 🚀
