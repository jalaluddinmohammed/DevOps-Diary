markdown
Copy code
# Azure AKS Commands

### Create AKS Cluster

az aks create --resource-group handson --name handsonaks --generate-ssh-keys

- This command creates an Azure Kubernetes Service (AKS) cluster named 'handsonaks' in the resource group 'handson' with SSH key generation.

### Delete AKS Cluster
```bash
Copy code
az aks delete --resource-group handson --name handsonaks

Deletes the AKS cluster named 'handsonaks' in the resource group 'handson'.

## Get AKS Cluster Credentials

az aks get-credentials -g handson -n handsonaks

Downloads and sets up the kubeconfig file to access the AKS cluster named 'handsonaks' in the resource group 'handson'.

## Kubernetes (kubectl) Commands

Alias for kubectl

alias k=kubectl

Creates an alias 'k' for the 'kubectl' command.

## Get Nodes

k get nodes -o wide

Displays information about nodes in the Kubernetes cluster.

## Get Pods


k get pods -o wide
Lists all pods in the Kubernetes cluster with additional details.

## Scale Deployment

k scale deployment frontend --replicas=30 

Scales the deployment named 'frontend' to 30 replicas.

## Describe Pod

k describe pod frontend-69747fb87-mtprg
Provides detailed information about a specific pod, identified by its name.

##Rollout History

## kubectl rollout history deployment frontend

Displays the rollout history of the deployment named 'frontend'.

## Get Events for ReplicaSet

kubectl get events | grep ReplicaSet
Filters and displays events related to ReplicaSets in the cluster.

## Rollout Undo Deployment

kubectl rollout undo deployment frontend
Rolls back a deployment named 'frontend' to the previous revision.

## kubectl Patching Commands
Patch Deployment from File

k patch deployment frontend --patch-file frontend-image-patch.yaml
Patches the deployment named 'frontend' using the specified patch file.

## Describe Pod Image

kubectl describe pod wp-mariadb-0 | grep image
Retrieves and displays the container image information for a specific pod.

## Get Secret YAML

kubectl get secret wp-mariadb -o yaml
Displays the YAML representation of the 'wp-mariadb' secret.

## Decode Base64 Secret

echo "VEtMN1l2WnVqZQ==" | base64 -d
Decodes a base64-encoded string.

## Helm Upgrade Command


helm upgrade wp bitnami/wordpress --set mariadb.image.tag=10.6.16-debian-11-r1 --set mariadb.auth.password="TKL7YvZuje" --set mariadb.auth.rootPassword="DTGR4djFnA" --set wordpressPassword="E1O7w7TADb" && kubectl get pods -w

Upgrades or installs the Helm chart for WordPress with custom values and monitors pod changes.

