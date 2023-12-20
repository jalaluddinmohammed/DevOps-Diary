# Kubernetes Ingress Components Overview

## 1. Ingress Controller:

- **Definition:**
  - An Ingress controller is a component responsible for managing external access to services in a Kubernetes cluster. It acts as a reverse proxy, handling external requests and routing them to the appropriate services based on rules defined in Ingress resources.

- **Functionality:**
  - Listens for Ingress resource changes.
  - Configures the underlying load balancer or proxy to route traffic based on the Ingress rules.

- **Example:**
  - In the context of your provided YAML, the Ingress controller is likely the Azure Application Gateway Ingress Controller (AGIC), given the annotation `kubernetes.io/ingress.class: azure/application-gateway`.

## 2. Kubernetes Add-On:

- **Definition:**
  - Kubernetes add-ons are additional components or features that enhance the functionality of a Kubernetes cluster. They are optional and can be enabled to provide additional capabilities.

- **Functionality:**
  - Enhance or extend Kubernetes functionality.
  - Can include features like monitoring, networking solutions, and integrations with cloud providers.

- **Example:**
  - In your context, the add-on is the `ingress-appgw` add-on, which is enabling the Ingress controller (AGIC) integration with Azure Application Gateway for external access.

## 3. Ingress Resource:

- **Definition:**
  - An Ingress resource is a Kubernetes object that defines how external HTTP/S traffic should be processed and routed to services within the cluster.

- **Functionality:**
  - Specifies rules for routing external traffic to services based on paths, hosts, and other criteria.
  - Acts as a configuration file for the Ingress controller.

- **Example:**
  - Your provided YAML is an Ingress resource. It directs traffic to the `frontend` service on port 80 when the path is `/`. The `kubernetes.io/ingress.class: azure/application-gateway` annotation indicates that the Ingress controller (AGIC) with the specified class should handle this Ingress resource.
## 4. Commands:

- **Create the RG:**
    - az group create -n agic -l westeurope

- **Create the VNET:**
    - az network vnet create -n agic-vnet -g agic   --address-prefix 192.168.0.0/24 --subnet-name agic-subnet   --subnet-prefix 192.168.0.0/24
- **Create the PublicIP:**
    - az network public-ip create -n agic-pip -g agic --allocation-method Static --sku Standard --dns-name "agic"


- **Create the Application Gateway:**
    - az network application-gateway create -n agic -l westeurope  -g agic --sku Standard_v2 --public-ip-address agic-pip  --vnet-name agic-vnet --subnet agic-subnet --priority 1

- **Getting the application gateway ID:**
     - appgwId=$(az network application-gateway show -n agic -g agic -o tsv --query "id") 

 - **Create the AKS Cluster:**
     - az aks create --resource-group rg-handsonaks --name handsonaks --generate-ssh-keys

 - **Connect to the AKS Cluster:**
     - az aks get-credentials -g rg-handsonaks -n handsonaks

 - **Setting an alias for kubectl:**
    - alias k=kubectl

 - **Enabled the ingress addons the AKS Cluster:**
     - az aks enable-addons -n handsonaks -g rg-handsonaks -a ingress-appgw --appgw-id $appgwId
     **az aks enable-addons:**
This Azure CLI command is used to enable add-ons for an AKS cluster.
- **-n handsonaks:** Specifies the name of the AKS cluster (in this case, "handsonaks").
- **-g rg-handsonaks:** Specifies the resource group name where the AKS cluster is located.
- **-a ingress-appgw:** Specifies the add-on to enable, which is the AGIC for integrating with Azure Application Gateway.
- **--appgw-id $appgwId:** Specifies the Application Gateway ID obtained in the previous step. This connects the AKS cluster to the specified Application Gateway.

In summary, these commands are part of the process to integrate an AKS cluster with an Application Gateway using the Application Gateway Ingress Controller add-on. The Application Gateway ID is obtained, and then the AGIC add-on is enabled for the AKS cluster, specifying the Application Gateway ID for the integration.
     

- **Create the guest-book yaml file:**
    - vi guest-book-all.yaml 

- **Apply the guest-book yaml file:**
    - k apply -f guest-book-all.yaml 

- **Create the agic resource yaml file:**
    - vi agic1-service.yaml

```yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-frontend-ingress
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend
            port:
              number: 80

```

- **Apply the agic resource yaml file:**
     - k apply -f agic1-service.yaml 

- **Getting the service:**
     - k get svc

- **Delete the agic:**
    - k delete -f agic1-service.yaml

- **Delete the guest-book application:**
    -  k delete -f guest-book-all.yaml

- **Delete the AKS Cluster:**
    - az aks delete --resource-group rg-handsonaks --name handsonaks



