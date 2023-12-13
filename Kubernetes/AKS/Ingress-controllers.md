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


