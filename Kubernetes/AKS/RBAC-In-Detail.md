## Reference Document:

``` bash
https://techcommunity.microsoft.com/t5/fasttrack-for-azure/azure-kubernetes-service-rbac-options-in-practice/ba-p/3684275
```
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
az role assignment create --role "Azure Kubernetes Service RBAC Admin" --assignee <<groud-id for aks-blog-admins>> --scope /subscriptions/<<subscription>>/resourcegroups/<<rg>>/providers/Microsoft.ContainerService/managedClusters/<<cluster-name>>/namespaces/blog
```
Note that the --scope property assigns that role only for the blog namespace. One downside for this approach is that you can't see this role assignment in the Portal either. You will need to utilize Az CLI to see the scopes you have assigned for namespaces:

``` bash
az role assignment list --scope /subscriptions/<<subscription>>/resourcegroups/<<rg>>/providers/Microsoft.ContainerService/managedClusters/<<cluster-name>>/namespaces/blog
```
## Managing User Access in the 'blog' Namespace

### Step 2: Empowering aks-blog-admins for User Access Management

Once users from the 'aks-blog-admins' group can successfully log in using 'az aks get-credentials' and perform operations within the 'blog' namespace using tools like kubectl, the challenge lies in how they can manage access for other regular non-admin users within this namespace without relying on Kubernetes YAML rolebindings.

### Step 3: Assigning IAM Role for User Access Administration

Utilize an Owner user for your cluster to assign the IAM role "User Access Administrator" to the 'aks-blog-admins' group. However, similar to Step 2, using the Portal for this assignment will grant the group rights to manage access for the entire cluster, which isn't desired. To specifically assign this role just for the 'blog' namespace:

### Step 4: Az CLI for Scoped Role Assignment

Employ the Az CLI to assign the "User Access Administrator" role scoped exclusively to the 'blog' namespace:

``` bash
az role assignment create --role "User Access Administrator" --assignee <<groud-id for aks-blog-admins>> --scope /subscriptions/<<subscription>>/resourcegroups/<<rg>>/providers/Microsoft.ContainerService/managedClusters/<<cluster-name>>/namespaces/blog
```
 Now, users under aks-blog-admins have permissions to assign Azure RBAC roles to other users for the scope of the blog namespace only. They can execute this Az CLI command to assign "Azure Kubernetes Service RBAC Writer" to group aks-blog-users:

 ``` bash
az role assignment create --role "Azure Kubernetes Service RBAC Writer" --assignee <<groud-id for aks-blog-users>> --scope /subscriptions/<<subscription>>/resourcegroups/<<rg>>/providers/Microsoft.ContainerService/managedClusters/<<cluster-name>>/namespaces/blog
```

And that's it. After that, any Azure user under group aks-blog-users can get their cluster credentials using az aks get-credentials and perform writing operations in the namespace, but they can't give access to others because this group doesn't have User Access Administrator IAM role like the admin group. For description on what each Azure RBAC role allows inside a AKS cluster.


# Conclusion and Recommendations

The choice between the AKS authentication and authorization options revolves around the extent of Azure RBAC utilization for authentication and authorization purposes. There is no singularly correct choice; it depends on specific needs. Consider the following factors to guide your decision-making:

### Local Accounts with Kubernetes RBAC:

- Use only if AKS cluster users cannot be part of Azure AD for certain reasons.
- Managing users within raw Kubernetes becomes complex, especially with larger teams.

### Azure AD Authentication with Kubernetes RBAC:

- Opt for this if utilizing Azure RBAC solely for controlling AKS credential access while managing user permissions via Kubernetes YAML manifests.
- Enhances cluster portability as role bindings are included within the cluster, although they contain Azure-specific group IDs, making tracking access a bit challenging.
- Managing AD group-based access in YAML requires careful documentation and versioning in source control for easier correlation.

### Azure AD Authentication with Azure RBAC:

- Select this option if utilizing Azure RBAC to control user actions directly within the cluster without YAML configurations.
- For assigning permissions to specific namespaces, the Az CLI is required as the Portal lacks this capability currently. The Access Control (IAM) blade assigns roles for the entire cluster.

The decision depends on your specific use case and preferences, considering the management complexity, YAML involvement, ease of access tracking, and tooling limitations within the Azure Portal for precise namespace-level permissions.

### Azure AD Authentication with Kubernetes RBAC:

## Read and Implement Chapter 8 of Kubernetes on Azure Book.
- Get solid understanding of followings:
- ClusterRole
- ClusterRoleBinding
- Role
- RoleBinding
- IAM Roles for getting AKS credentials.

### Reference Document
``` bash
https://stacksimplify.com/azure-aks/kubernetes-rbac-role-and-rolebinding-with-azure-ad/
```
![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/734a0676-b3f3-4aa7-8574-42334d0ed94c)

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/b8ff4e06-e697-42f1-b27f-73bb6742db0a)

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/4602ea5f-eb96-49c0-a31f-dc9aaabd9c2b)


![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/2902fa46-00b7-4a0d-b891-e031ff43a149)


![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/593191c1-63ba-4cb0-b57f-7070d88ffa9f)

  
