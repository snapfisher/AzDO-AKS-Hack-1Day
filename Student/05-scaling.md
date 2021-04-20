# Challenge 5: Scaling

[< Previous Challenge](./04-k8sdeployment.md) - **[Home](../README.md)** - [Next Challenge >](./06-CI-in-Azure-Devops-Classic.md)

## Introduction

Now that you have your application running there are a few things to consider. How do you make it responsive? How do you make it resilient? How do you control costs while mananging load? In this challenge, we'll find that out.

## Description

In this challenge we will cover scale and resiliency from multiple aspects. We'll make sure enough replicas of our container are running to handle load. We'll make sure that there are enough resources in our cluster to handle all the containers we want to run and we'll figure out how Kubernetes repairs itself.

- Scale the nodes in the AKS cluster from 3 to 1.  Make sure you watch the pods after you perform the scale operation.  You can use an Azure CLI command like the following to do this: 

**`az aks nodepool scale --resource-group wth-rg01-poc --cluster-name wth-aks01-poc --name nodepool1 --node-count 1`**

- Next, scale the **Web** app to 2 instances
	- This should be done by modifying the YAML file for the Web app and re-deploying it 
- Now, scale the **API** app to 4 instances using the same technique as above.  
- AKS supports autoscaling through the cluster autoscaler and the horizontal pod autoscaler.  We are probably too time limited to undertake this as a challenge, but if you have extra time you could update your cluster to use the cluster autoscaler.  Then, change your deployment to require many more web pods and watch what happens.

## Success Criteria

1. You can scale your cluster up and down
1. (Optionally) Have your cluster use the autoscaler

## Resource Links

https://docs.microsoft.com/en-us/cli/azure/aks/nodepool?view=azure-cli-latest

https://docs.microsoft.com/en-us/azure/aks/cluster-autoscaler