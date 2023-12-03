![Uploading image.png…]()
# Azure Kubernetes Service (AKS) Overview

Azure Kubernetes Service (AKS) simplifies the creation and management of Kubernetes clusters. In a typical Kubernetes cluster, there are master nodes responsible for the Kubernetes API and cluster state, and worker nodes that run the actual workload.

## Cluster Creation with AKS

AKS streamlines cluster creation. When you create an AKS cluster, it automatically sets up the Kubernetes master. Virtual Machine Scale Sets (VMSS) are created in your subscription, and their VMs become worker nodes in your Kubernetes cluster. You have the option to use a free Kubernetes control plane or opt for a paid plan with a financially backed SLA. In either case, you pay for the VMs hosting your worker nodes.

## Integration with Azure Load Balancer and Application Gateway

In AKS, services running on Kubernetes integrate with Azure Load Balancer, and Kubernetes Ingresses can be integrated with Azure Application Gateway. This integration ensures that creating a service or Ingress in Kubernetes results in the creation of rules in Azure Load Balancer or Azure Application Gateway. These services then route traffic to the appropriate node in your cluster hosting your pods.

## Cluster Management Features

AKS includes functionalities that enhance cluster management. It manages cluster upgrades to newer Kubernetes versions, facilitates easy scaling of clusters by adding or removing nodes, and offers integration options for operational ease. AKS clusters can be configured with integration with Azure Active Directory (Azure AD) for straightforward management of identities and Role-Based Access Control (RBAC). RBAC defines user access to resources and the actions they can perform. Additionally, AKS can be seamlessly integrated into Azure Monitor for containers, simplifying monitoring and troubleshooting of applications.

