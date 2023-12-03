# This doc explains the K8S architecture and its components.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/35fc4d53-3da4-4b71-84d0-71cea59cd7b1)




# Kubernetes Architecture Overview

Kubernetes follows a distributed system architecture with several key components. Let's break down the primary components and their roles:

## 1. Master Node ( Control Plane)

- **API Server:** Exposes the Kubernetes API for processing RESTful API requests.
- **Controller Manager:** Manages various controllers that regulate the state of the cluster, such as replication controllers, endpoints controllers, and more.
- **Scheduler:** Assigns workloads to nodes based on resource availability.
- **Cloud Controller Manager:** This component is used by the Cloud Providers such as AWS, Azure and GCP to link your cluster into your cloud providers API, example: when you need a LB to be provisioned on cloud then this component will talk to your cloud provider and get the LB provisioned.

## 2. Node (Worker Node)

- **Kubelet:** Ensures containers in a pod are running, communicates with the master node.  It takes care of starting, stopping, and maintaining container health.
- **Container Runtime:** Software for running containers (e.g., Docker or containerd.).
- **Kube Proxy:**  Maintains network rules on nodes, enabling communication between different pods and services.

## 3. Pods

- Smallest deployable units in Kubernetes.
- Containers within a pod share the same network namespace. communicate with each other using localhost.

## 4. Replication Controller / Replica Set

- Ensures a specified number of pod replicas are running.
- Maintains the desired number of pod instances.

## 5. Service

- Defines a logical set of pods with a policy for accessing them.
- Provides a consistent way to expose and access applications.

## 6. Volume

- Provides persistent storage to pods.
- Used for sharing data between pods or persisting application data.

## 7. Namespace

- Divides cluster resources between users, teams, or projects.
- Organizes and manages resources within a cluster.

## 8. ConfigMap and Secret

- **ConfigMap:** Provides access to configuration file from the container image.
- **Secret:** Stores sensitive information like passwords or tokens.

## 9. StatefulSets

- Manages deployment and scaling of pods with unique, stable network identities.
- Suitable for stateful applications requiring stable network identities.

## 10. Custom Resource Definitions (CRDs)

- Extends the Kubernetes API to include custom resources.
- Allows users to define and use custom resources within applications.
- Argo CD, Gatekeeper etc.

---

**Example: Deploying a Nginx Pod**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx-container
    image: nginx

To apply this configuration, use the following command:
```bash
kubectl apply -f nginx-pod.yaml

## This creates a pod running the Nginx web server. The master node schedules the pod to a worker node, and the Kubelet ensures the Nginx container is running.
