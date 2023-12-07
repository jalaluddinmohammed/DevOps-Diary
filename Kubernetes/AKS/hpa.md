### To perform this lab first deploy all kubernetes manifest using guestbook-all-in-one.yaml

## Manual Scaling

Kubernetes gives us the ability to scale each component of an application dynamically. In this section, we will show you how to scale the front end of the guestbook application. Right now, the front-end deployment is deployed with three replicas.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/455f9710-3820-4ae2-b488-91e801a293b9)

## To scale the front-end deployment, you can execute the following command:

kubectl scale deployment/frontend --replicas=6

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/755f2699-e52f-4688-b688-6f9e7b98e6e8)

This will cause Kubernetes to add additional pods to the deployment. You can set the number of replicas you want, and Kubernetes takes care of the rest. You can even scale it down to zero (one of the tricks used to reload the configuration when the application doesn't support the dynamic reload of configuration). To verify that the overall scaling worked correctly, you can use the following command:

As you can see, the front-end service scaled to six pods. Kubernetes also spread these pods across multiple nodes in the cluster. You can see the nodes that this is running on with the following command

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/13b089da-0dec-4d47-ac1f-19882c4edd23)


### Using the HPA
Scaling manually is useful when you're working on your cluster. 
For example, if you know your load is going to increase, you can manually scale out your application.
In most cases, however, you will want some sort of autoscaling to happen on your application. In Kubernetes, you can configure autoscaling of your deployment using an object called the Horizontal Pod Autoscaler (HPA).
HPA monitors Kubernetes metrics at regular intervals and, based on the rules you define, it automatically scales your deployment. For example, you can configure the HPA to add additional pods to your deployment once the CPU utilization of your application is above 50%.

1. To start the configuration, let's first manually scale down our deployment to
one instance:

kubectl scale deployment/frontend --replicas=1

## Save this as hpa.yaml

## Horizontal Pod Autoscaler Configuration

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: frontend-scaler
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: frontend 
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

## After you execute hpa.yaml you will find below.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/1bee2635-e08f-4de9-8a0f-e7e3a6965008)

## Now the load is normal, hence HPA is not doing auto scaling.

![image](https://github.com/jalaluddinmohammed/DevOps-Diary/assets/145260536/a9d0e8b9-966b-45a3-baf1-e8fe2493edaf)

## Now lets genereate some load.



