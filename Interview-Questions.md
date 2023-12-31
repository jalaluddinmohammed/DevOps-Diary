## Interview Questions & Answers

## What is the difference between Docker and Kubernetes ?

Docker is a container platform and Kubernetes is a container orchestration environment that offers capabilities like Auto healing, Auto Scaling, Clustering, and Enterprise level support like Load balancing.
What are the main components of Kubernetes architecture? On a broad level, you can divide the Kubernetes components into two parts.

1. Control Plane (API SERVER, SCHEDULER, Controller Manager, C-CM, ETCD)
2. Data Plane (Kubelet, Kube-proxy, Container Runtime)
   
## What are the main differences between the Docker Swarm and Kubernetes?

Kubernetes is better suited for large organizations as it offers more scalability, networking capabilities like policies, and huge third-party ecosystem support.

It's essential to note the fundamental differences. Kubernetes stands out as a widely favoured choice, particularly in the context of enterprise-scale applications and organizations, offering robust scaling options and advanced networking capabilities. On the other hand, Docker Swarm, while notably straightforward to install and use, is more suited for smaller-scale or uncomplicated applications. 

The simplicity of Docker Swarm comes with limitations, especially in advanced networking and third-party support when compared to the extensive backing that Kubernetes receives from communities like CNCF. In essence, the choice between Kubernetes and Docker Swarm boils down to the scale and complexity of the project. 

For mid to large-scale solutions, Kubernetes is the go-to, given its extensive community support and customizable capabilities. 

However, for those unconcerned about scale intricacies, Docker Swarm's simplicity in installation and usage might be a viable option. 

Considering the current market trends, Kubernetes holds a dominant position, with widespread adoption and ample job opportunities in  of DevOps.


## What is the difference between a Docker container and a Kubernetes pod?

A pod in Kubernetes is a runtime specification of a container in docker. A pod provides a more declarative way of defining using YAML and you can run more than one container in a pod, and they can talk to each other and they can share same resources inside a pod.

## What is a namespace in Kubernetes ?

In Kubernetes namespace is a logical isolation of resources, network policies, rbac, and everything. For example, there are two projects using the same k8s cluster. One project can use namespace1 and another project can use namespace2 without any overlap and authentication problems.

For each project you can't create k8s cluster right?

In an amazon, let's say 20 microservices are running then all of them will be deployed as a different name space in the k8s cluster.

## What is the role of the kube proxy?

Kube-proxy works by maintaining a set of network rules on each node in the cluster, which are updated dynamically as services are added or removed. When a client sends a request to a service, the request is intercepted by kube-proxy on the node where it was received. Kube-proxy then looks up the destination endpoint for the service and routes the request accordingly. 

Kube-proxy is an essential component of a Kubernetes cluster, as it ensures that services can communicate with each other.


## What are the different types of services within Kubernetes?

## 3 Major responsibilities of Service in k8s.
	1. Service discovery
	2. Load balancing
	3. Exposing the application to external world. ( Different Service modes)

There are three different types of services that a user can create. 

1. Cluster IP Mode
2. Node Port Mode
3. Load Balancer Mode


![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/60e18b5f-53ca-487d-a3da-38a53e8669c0)



## What is the role of Kubelet ?

Kubelet manages the containers that are scheduled to run on that node. It ensures that the containers are running and healthy and that the resources they need are available. Kubelet communicates with the Kubernetes API server to get information about the containers that should be running on the node, and then starts and stops the containers as needed to maintain the desired state. It also monitors the containers to ensure that they are running correctly and restarts them if necessary.


## When somebody asks us the question, "What are the day-to-day activities as a DevOps engineer or what are the day-to-day activities on Kubernetes?"

We are not able to answer, so don't worry about it. Actually, you know, it's a very easy question or it's a very simple question to answer. If you are starting with a good answer like this, it will be our first question the interviewer can ask you, "What are your day-to-day activities on DevOps or what are the day-to-day activities on Kubernetes?" So, because this is your first question or most probably in the first one or two questions.

If you answer this question in a very good way, then it boosts your confidence, and this question is a very simple one. You don't have to complicate the question or complicate your answer. Simply say to them that as part of the DevOps engineer role, we manage Kubernetes clusters for our organization, and we also ensure that the applications are deployed onto the Kubernetes cluster and there are no issues with the application. We have set up monitoring on our Kubernetes cluster. We ensure that whenever there are bugs on the Kubernetes cluster, for example, the developers are not able to troubleshoot some issue with respect to pods, developers are not able to troubleshoot with respect to services. They are not able to route the traffic inside the Kubernetes cluster. So, in such cases, as subject matter experts on the Kubernetes clusters, we come into the picture and we solve their problems.

But apart from that, we also do a lot of maintenance activities. For example, we have Kubernetes clusters with three master nodes and ten worker nodes. So we have to do some continuous maintenance activities on these worker nodes, probably upgrading the versions of these worker nodes or installing some default mandatory packages, ensuring that these worker nodes are not exposed to security vulnerabilities. So, all of these things are our day-to-day activities on Kubernetes. Apart from that, we also serve as subject matter experts on Kubernetes. So, if anyone in the organization has any issues with Kubernetes, they create Jira items for us or they create tickets for us, and we will help them in solving or making them understand the concept of Kubernetes.

So, this is how you can explain. It is a very simple answer. It's a very straightforward answer. You don't have to get scared about this question.

## 1. Pod Lifecycles

## CrashLoopBackOff:

Cause: Usually indicates a problem within the application or configuration.
Troubleshooting: Check pod logs (kubectl logs <pod-name>) and describe pod (kubectl describe pod <pod-name>).

## ImagePullBackOff:

Cause: The specified container image couldn't be pulled.
Troubleshooting: Verify the image name and credentials. Check network connectivity and registry access.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/ddb90e4a-bb34-4996-ba32-d443e8a8b00f)

You might see either a status called ErrImagePull or ImagePullBackOff. Both errors refer to the fact that Kubernetes cannot pull the image from the registry.
The ErrImagePull error describes just this; ImagePullBackOff describes that Kubernetes will back off (wait) before retrying to download the image. This back-off has an exponential delay, going from 10 to 20 to 40 seconds and beyond, up to 5 minutes.
 
- Run the following command to get the full error details:
## kubectl describe pods pod-name

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/da561c0d-291f-4275-bff8-97c94a9c5698)


Image pull errors can occur when images aren't available or when you don't have access to the container registry.

## Because Kubernetes did a rolling update, the front end was continuously available with zero downtime. Kubernetes recognized a problem with the new specification and stopped rolling out additional changes automatically.



## How to login to PODS?

## kubectl exec -it <pod-name> -- bash

- This command lets you run commands on the command line of that pod. With the -it option, it attaches an interactive terminal to the pod and gives you a shell that you can run commands on. The following command launches a Bash terminal on the pod:


  ![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/5b68c548-6301-48a9-ad49-09488640818b)

# Kubernetes Commands Cheat Sheet

## `kubectl exec`
 
 - Execute a command in a running container within a pod0
- Used to execute commands inside a running container within a pod.
- It provides an interactive shell to the container, allowing you to run commands, debug, or perform troubleshooting.
- It supports both interactive and non-interactive modes.
- You can execute commands as if you were working directly inside the container.

- kubectl exec -it <pod-name> -c <container-name> -- /bin/bash

Explanation:

-it: Opens an interactive terminal.

<pod-name>: Replace with the name of the pod.

-c <container-name>: Specify the container name if there are multiple containers in the pod.

-- /bin/bash: Replace with the desired command to execute inside the container.

## kubectl logs
 Fetch and display logs of a container running within a pod

- kubectl logs <pod-name> -c <container-name>

Explanation:
<pod-name>: Replace with the name of the pod.
-c <container-name>: Specify the container name if there are multiple containers in the pod.

- Used to fetch and display the logs of a container running within a pod.
- Primarily focuses on providing access to the standard output (stdout) and standard error (stderr) logs of a specific container in a pod.
- Useful for real-time monitoring and debugging of application logs.


## kubectl describe
# Display detailed information about a Kubernetes resource

## kubectl describe pod <pod-name>

Explanation:

<pod-name>: Replace with the name of the pod or the specific resource you want to describe.

- Used to display detailed information about Kubernetes resources, including pods, nodes, services, etc.
- Provides metadata and configuration details for a given resource.
- The information includes events, labels, annotations, resource utilization, and other relevant details.
- It's useful for understanding the current state and configuration of a resource.

## The following points summarize what was covered in this section on how to identify an error and how to fix it:
- Errors can come in many shapes and forms.
- Most of the errors encountered by the deployment team are configuration
issues.
- Use logs to identify the root cause.
- Using kubectl exec on a container is a useful debugging strategy.
- Note that broadly allowing kubectl exec is a serious security risk, as it lets the Kubernetes operator execute commands directly in the pods they have access to. Make sure that only a subset of operators has the ability to use the kubectl exec command. You can use role-based access control to manage this access restriction, as you'll learn in Chapter 8, Role-based access controlin AKS.
- Anything printed to stdout and stderr shows up in the logs (independent of the application/language/logging framework).





## 2. Service Discovery
## ClusterIP:

Usage: Internal service discovery within the cluster.
Example: http://<service-name>.<namespace>.svc.cluster.local.

## NodePort:
Usage: Exposes the service on each node's IP at a static port.
Example: Accessible externally using NodeIP:NodePort.

## LoadBalancer:
Usage: Provides an externally accessible IP with external traffic balanced to service endpoints.
Example: Useful in cloud environments with load balancer support.

## 3. Scaling
## Horizontal Pod Autoscaling (HPA):
Trigger: Based on observed CPU utilization or custom metrics.
Configuration: Define scaling thresholds and behavior in the HPA object.

## 4. Rolling Updates
Rolling Update:
Strategy: Gradual replacement of old pods with new ones.
Configuration: Defined in the deployment spec using strategies like "RollingUpdate."

## 5. Persistent Storage
## Persistent Storage in Pods:

Configuration: Specify storage requirements in the pod spec using Persistent Volume Claims (PVCs).
Types: Local storage, Network-Attached Storage (NAS), and cloud-specific solutions like AWS EBS or Azure Disk.

## 6. Security
## RBAC (Role-Based Access Control):

Roles: Define sets of permissions.
RoleBinding: Associates roles with specific users or groups.
ClusterRoles: Roles that apply globally.

## 7. Networking
## Services:

ClusterIP: Default for internal service discovery.
NodePort: Exposes service externally on node IP.
LoadBalancer: Exposes service with an external load balancer.

## Ingress:
Routing: Routes external HTTP/S traffic to services based on rules.
Example Rule: Direct /api to a specific service and /web to another.

## Network Policies:
Control: Define how pods communicate with each other.
Example Rule: Restrict communication between specific pods.

## 8. Monitoring and Logging
## Monitoring:
## Prometheus: Popular monitoring tool for Kubernetes and Visual representation using Grafana.
Metrics: Monitor cluster health, pod resource usage, and custom metrics.
## Logging:
Fluentd, Elasticsearch: Collect and analyze logs from containers.
Log Aggregation: Centralize logs for easier analysis.



