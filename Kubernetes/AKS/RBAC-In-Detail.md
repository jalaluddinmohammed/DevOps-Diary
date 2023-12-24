# Managing Access to Azure Kubernetes Service (AKS) Clusters

When building an AKS cluster for your team, one of the primary considerations is managing access for different groups or individuals while maintaining simplicity and security.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/9f2baafb-28dd-451d-8044-7b7c84042b60)



When using the Azure Portal to create a new AKS instance, it provides the following options:

- Local accounts with Kubernetes RBAC
- Azure AD authentication with Kubernetes RBAC
- Azure AD authentication with Azure RBAC

These options are detailed in the referenced documentation. For those less experienced with Kubernetes and Azure, the official documentation can be overwhelming. This article aims to simplify the decision-making process and provide an easier understanding of the options available.

## Scenario Overview

Consider yourself as the cluster admin/owner, responsible for an AKS cluster utilized by various developer teams to deploy their applications. The plan involves:

1. Creating a new namespace in AKS for each developer team.
2. Assigning namespace permissions to each team, divided into two groups:
   - **Namespace Users Group:** Members can deploy and edit applications within the namespace but cannot assign access to others.
   - **Namespace Admins Group:** Members have permissions like the users group but can also manage access for others within the namespace. This delegation helps central admins delegate access management for each team.

This straightforward scenario is common in AKS clusters shared across multiple teams. Let's explore how this scenario translates into practice with each available RBAC option in AKS.

### Scenario Details
For the demonstration purposes in this article, we'll use the following details:

- **Namespace Name:** blog
- **Azure AD Group (Cluster Admins):** aks-admins
- **Azure AD Group (Namespace Admins):** aks-blog-admins
- **Azure AD Group (Namespace Users):** aks-blog-users

# Managing Access to Azure Kubernetes Service (AKS) Clusters - Options Explanation

## Local accounts with Kubernetes RBAC

In this option, there's no integration between Azure Active Directory (AD) and the AKS cluster. Therefore, specific Azure AD user groups cannot be mapped to namespaces within the AKS cluster directly. Access is managed using native Kubernetes methods like client certificates, bearer tokens, etc. Refer to the [official documentation](official-doc-link) for more details.

Users can also use the Az CLI command `az aks get-credentials` to obtain local kubeconfig credentials if they belong to the AKS built-in roles. However, this grants uniform access (clusterAdmin or clusterUser) to all users within the cluster. Without Azure AD integration, user differentiation within Kubernetes becomes challenging.

This configuration is recommended only when all users are not part of Azure AD and cannot be included for some reason. Managing users becomes intricate in this scenario.

## Azure AD authentication with Kubernetes RBAC

In this option, AKS delegates authentication to Azure AD but not authorization. Azure AD manages the "who is logging in," while permissions defining user actions within the AKS cluster are still governed internally, not by Azure AD roles and permissions.

Let's focus more practically on the steps required within an AKS cluster to achieve the scenario described earlier.

### Prerequisites

- Az CLI
- Kubectl
- Basic understanding of Azure AD users and groups
- Ensure the cluster is created or updated to use Azure AD, setting the admin group to 'aks-admins' (refer [here](azure-doc-link))
- Allow both 'aks-blog-admins' and 'aks-blog-users' groups to fetch cluster credentials using "az aks get-credentials" in Azure Portal => AKS => Access Control (IAM) by assigning the "Azure Kubernetes Service Cluster User Role"
- Ensure the existence of a namespace called 'blog' in the cluster. If it doesn't exist, start by obtaining your kubectl credentials using 'az aks get-credentials' using one of the 'aks-admins' users, and then use kubectl:

```bash
# Your commands here for kubectl usage

kubectl create namespace blog
```
### Steps
1) Next step is to create a Kubernetes RoleBinding YAML file with the following content:

``` yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: blog-admin-binding
  namespace: blog
subjects:
  - kind: Group
    name: d89e6a33-eaca-4be4-b1e5-9f5c74d6a35c
    #Azure AD group name: aks-blog-admins
    namespace: blog
roleRef:
  kind: ClusterRole
  name: admin
  apiGroup: rbac.authorization.k8s.io
```
```bash
kubectl apply -f <<filename.yaml>>
```
## Scenario Details: Assigning Access within Namespace 'blog'

Pay attention to the Azure AD group object ID mentioned in line 8; this represents any user in the cluster belonging to that group. Users within this group will receive the built-in Kubernetes admin role (line 13) specifically for the 'blog' namespace (line 10).

At this stage, users from the 'aks-blog-admins' group can log in to the cluster and possess full access rights within the 'blog' namespace. They now have the capability to create RoleBindings within the namespace to grant permissions to other users. Let's proceed to assign access to a developer who will have comprehensive access to the 'blog' namespace except for permissions to create RoleBindings.

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: blog-user-binding
  namespace: blog
subjects:
  - kind: Group
    name: 996844bd-95a3-48fc-93e9-7f1f1177ff83
    #Azure AD group name: aks-blog-users
    namespace: blog
roleRef:
  kind: ClusterRole
  name: edit
  apiGroup: rbac.authorization.k8s.io
```
``` bash
kubectl apply -f <<filename.yaml>>
```
## Managing Authorization within the AKS Cluster

In this configuration, the new RoleBinding (as per the YAML file) assigns the built-in role 'edit' (line 13) instead of 'admin' to the 'aks-blog-users' group (line 8).

From this point onward, the authorization setup within the AKS cluster is correctly established. Utilize Azure AD groups to manage individuals by adding or removing them from the respective groups associated with the given namespace.

It's important to note that in both YAML files, the Azure AD group object ID is used instead of the friendly Azure AD group name. To maintain clarity in your YAML files, it's advisable to add comment lines describing the group names. However, remember that commented lines will be removed by Kubernetes when applying the manifests in the cluster. Therefore, you'll need to refer to the source control files to retrieve this information.


# Azure AD Authentication with Azure RBAC in AKS

In simpler terms, Azure RBAC enhances Azure AD integration by managing both authentication and authorization within an AKS cluster. With Azure RBAC, you no longer need to create YAML manifests to manage user access within namespaces; instead, it operates similarly to other Azure services, utilizing Azure AD roles (IAM) exclusively. Let's replicate the same scenario previously demonstrated for Kubernetes RBAC to provide a clearer understanding.

## Prerequisites

- Az CLI
- Basic understanding of Azure AD users and groups
- Ensure the cluster is created or updated to use Azure AD and Azure RBAC. Assign the IAM role "Azure Kubernetes Service RBAC Cluster Admin" to the group 'aks-admins'. Refer [here](azure-doc-link) for detailed information.
- Allow both 'aks-blog-admins' and 'aks-blog-users' groups to fetch cluster credentials using "az aks get-credentials" in Azure Portal => AKS => Access Control (IAM) by assigning the role "Azure Kubernetes Service Cluster User Role".
- Ensure the existence of a namespace called 'blog' in the cluster. If it doesn't exist, start by obtaining your kubectl credentials using 'az aks get-credentials' using one of the admin users of 'aks-admins', and then use kubectl:

```bash
# Your commands here for kubectl usage
kubectl create namespace blog
```
## Steps for Assigning Azure RBAC Roles to Specific Namespace in AKS

### Step 1: Azure AD Integration

Ensure Azure AD integration is established within the AKS cluster.

### Step 2: Assign IAM Role to aks-blog-admins

Assign the IAM role "Azure Kubernetes Service RBAC Cluster Admin" to the 'aks-blog-admins' group. However, it's crucial to note a limitation: using the Azure Portal's Access Control (IAM) blade from the AKS service will assign this role for all namespaces inside the cluster, not just for the 'blog' namespace. To specifically scope this RBAC Cluster Admin role to a particular namespace:

### Step 3: Use Az CLI for Scoped RBAC Assignment

Utilize the Az CLI to assign the RBAC Cluster Admin role scoped only to the 'blog' namespace:

```bash
# Az CLI command to assign RBAC Cluster Admin role specifically to the 'blog' namespace
az aks update-credentials --resource-group <resource-group-name> --name <aks-cluster-name> --reset-service-principal --scope cluster --output none
