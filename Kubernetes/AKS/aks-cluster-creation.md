## What happends behind the scenes when you create a cluster on AKS or EKS or GKE.

When you create a Kubernetes cluster with three master nodes and ten worker nodes on AWS/AKS, here's what's happening behind the scenes:

## 1. Master Nodes:
	• Imagine the master nodes as the brains of your operation. They manage and control everything happening in the cluster.
	• AWS/AKS takes care of creating three virtual servers (EC2/VM instances, in AWS/AKS) to serve as your master nodes. These instances are distributed across availability zones for redundancy and reliability.
## 2. Worker Nodes:
	• Worker nodes are like the hands and legs of your cluster. They do the actual work, running your applications in containers.
	• AWS/AKS sets up ten EC2/VM instances to act as your worker nodes. These instances are connected to the master nodes.
## 3. Networking:
	• AWS/AKS handles the networking to ensure all these nodes can communicate seamlessly. Each node gets its own IP address, and there are security groups and other settings to control who can talk to whom.
## 4. Kubernetes Software:
	• Now, AWS/AKS installs the Kubernetes software on each node. This includes things like kubelet on worker nodes and kube-apiserver on master nodes.
	• This software allows all the nodes to understand each other, manage containers, and keep the cluster running smoothly.
## 5. Node Registration:
	• The worker nodes register themselves with the master nodes. This is like telling the masters, "Hey, I'm here and ready to take on some work!"
## 6. Load Balancing (Optional):
	• If you've set up load balancing, AWS/AKS ensures that requests get distributed evenly among the master nodes. This is to prevent overloading one master while others sit idle.
## 7. Storage (Optional):
	• If you've configured storage, AWS/AKS provides the necessary resources for persistent storage. This allows your data to survive even if a node goes down.
So, in a nutshell, AWS/AKS sets up virtual servers, installs Kubernetes software, takes care of networking, and ensures that all the nodes—master and worker—can talk to each other. The result is a fully functional Kubernetes cluster ready to run your containerized applications!


The details I provided are more aligned with a general Kubernetes setup using tools like kops rather than Amazon EKS (Elastic Kubernetes Service). Let me clarify a bit:

## 8. kops (Kubernetes Operations):
	• kops is a popular open-source tool for setting up, upgrading, and maintaining Kubernetes clusters on various cloud platforms, including AWS/AKS.
	• It provides commands to create and manage the necessary AWS/AKS resources, such as EC2 instances for master and worker nodes, VPCs, and more.
## 9. Amazon EKS:
	• Amazon EKS is a managed Kubernetes service provided by AWS/AKS. With EKS, AWS/AKS takes care of many operational aspects for you, such as patching, scaling, and maintaining the Kubernetes control plane.
	• You don't need to manually provision EC2 instances for the master nodes; AWS/AKS handles that part for you.
For simplicity, the explanation I gave earlier was more in line with a generic Kubernetes setup using tools like kops, where you have more control over the underlying infrastructure. If you were using Amazon EKS, some of the low-level details, like EC2 instance provisioning for the master nodes, would be abstracted away by the managed service.

-- In Amazon EKS (Elastic Kubernetes Service), you still need to provision worker nodes using EC2 instances. While EKS manages the Kubernetes control plane for you, including the master nodes, it does not manage the worker nodes where your containers will run.
Here's a breakdown:
## 10. EKS Control Plane:
	• Amazon EKS takes care of the Kubernetes control plane, which includes the master nodes responsible for managing and controlling the overall state of the cluster.
## 11. Worker Nodes:
	• You are responsible for provisioning and managing the worker nodes. These are the instances where your containerized applications actually run.
	• You can use EC2 instances as worker nodes, and EKS provides an Amazon Machine Image (AMI) optimized for EKS that you can use when launching your worker nodes.
## 12. Auto Scaling Groups:
	• Many users leverage AWS/AKS Auto Scaling Groups in combination with EKS to dynamically adjust the number of worker nodes based on demand.
So, while EKS abstracts away much of the complexity of managing the Kubernetes control plane, you retain control over the worker nodes' infrastructure. This allows you to customize the worker nodes based on your specific requirements and take advantage of AWS/AKS features like Auto Scaling.
