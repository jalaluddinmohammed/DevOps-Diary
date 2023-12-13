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
## Commands:

 az group create -n agic -l westeurope
   
az network vnet create -n agic-vnet -g agic   --address-prefix 192.168.0.0/24 --subnet-name agic-subnet   --subnet-prefix 192.168.0.0/24
   

az network application-gateway create -n agic -l westeurope  -g agic --sku Standard_v2 --public-ip-address agic-pip  --vnet-name agic-vnet --subnet agic-subnet --priority 1

appgwId=$(az network application-gateway show -n agic -g agic -o tsv --query "id") 
 

az aks create --resource-group rg-handsonaks --name handsonaks --generate-ssh-keys

az aks enable-addons -n handsonaks -g rg-handsonaks -a ingress-appgw --appgw-id $appgwId

alias k=kubectl
   

az aks get-credentials -g rg-handsonaks -n handsonaks

k apply -f guest-book-all.yaml 


k apply -f guest-book-all.yaml 


vi agic1-service.yaml


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

 k apply -f agic1-service.yaml 

 k get svc

 k delete -f agic1-service.yaml

 k delete -f guest-book-all.yaml

 az aks delete --resource-group rg-handsonaks --name handsonaks



