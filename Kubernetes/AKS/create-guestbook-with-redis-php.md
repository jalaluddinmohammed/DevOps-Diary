## Create a guestbook with Redis and PHP, this image represents for GKE, but Its same for AKS as well

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/f5a7986c-ad59-4a1c-8c64-397d7a6fba49)

# 2 Tier Architecture
This tutorial demonstrates how to build a multi-tier web application using Google Kubernetes Engine (GKE). The tutorial application is a guestbook that lets visitors enter text in a log and see the last few logged entries.
The tutorial shows how to set up the guestbook web service on an external IP with a load balancer, and how to run a Redis cluster with a single master (leader) and multiple replicas (followers).

The example highlights a number of important AKS concepts:

Declarative configuration using YAML manifest files
Deployments, which are Kubernetes resources that determine the configuration for a set of replicated Pods
Services to create internal and external load balancers for a set of Pods

## Objectives
To deploy and run the guestbook application on AKS:
Set up the Redis leader
Set up two Redis followers
Set up the guestbook web frontend
Visit the guestbook website

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/81695453-9c0c-421b-ab91-2f403eb82dbf)

-  The guestbook application needs to communicate with the Redis leader to write its data. 
-  The guestbook application needs to communicate with the Redis followers to read data.
-  The guestbook app uses a PHP frontend. It is configured to communicate with either the Redis follower or leader Services, depending on whether the request is a read or a write.
-  The frontend exposes a JSON interface, and serves a jQuery-Ajax-based UX.
-  Redis Follower is also served here as a backup for Redis Leader, if Redis leader goes down, then Follower can be promoted as a Redis Leader, making fault tolerance.

## guest-book-all-in-one.yaml
Look for this all in one file in AKS directory

## Reference

https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook?_gl=1*197v0dh*_ga*MTIzMjQ0NTUxNC4xNzAxNjcwMjc3*_ga_WH2QY8WWF5*MTcwMTY3MDI3OC4xLjEuMTcwMTY3MDMwNS4wLjAuMA..&_ga=2.260529805.-1232445514.1701670277














