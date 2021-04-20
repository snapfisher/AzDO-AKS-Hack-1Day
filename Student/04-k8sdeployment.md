# Challenge 4: Your First Deployment

[< Previous Challenge](./03-k8sintro.md) - **[Home](../README.md)** - [Next Challenge >](./05-scaling.md)

## Introduction

Now the rubber meets the road.... we will be deploying the application to our newly minted cluster and making sure it works as intended. You'll learn about pods, deployments and services, oh my!

## Description

In this challenge we need to get our application up and running in Kubernetes. We will learn about Kubernetes configuration YAML files used to create the various Kubernetes resources that will be needed to run our app. We will give our containers resource limits and open the app up to the outside world so we can test it.


### Deploy the **API app** from the command line using kubectl and YAML files:

- **NOTE:** Sample YAML files to get you started can be found in the Student Resources folder. (They are really, really close to working, only minor changes are needed)
- Configuration details:
  - Number of pods: 1
  - Service: Internal
  - Port and Target Port: 3001
  - CPU: 0.5
  - Memory: 128MB
- Hint:  Make sure you correctly set [resource requests](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#requests-and-limits)
- We have not exposed the API app to the external world. Therefore, to test it you need to:
	- Figure out how to get a bash shell on the API app pod just deployed.  _[Hint 1: link to docs](https://kubernetes.io/docs/tasks/debug-application-cluster/get-shell-running-container/)_   _Hint 2: VS Code can make this point-n-click_
	- From the terminal, curl the url of the `/speakers` end point.
	- You should get a huge json document in response.
  
### Deploy the Web app from the command line using kubectl and YAML files
- **NOTE:** Sample YAML files to get you started can be found in the Student Resources folder. (They are really, really close to working, only minor changes are needed)
- **NOTE:** The Web app expects to have an environment variable pointing to the URL of the API app named:
	- **CONTENT_API_URL** (and is already in the supplied Dockerfile)
- Create a deployment yaml file for the Web app using the specs from the API app, except for:
	- Port and Target Port: 3000
- Create a service yaml file to go with the deployment
	- **Hint:** Not all "types" of Services are exposed to the outside world
- **NOTE:** Applying your YAML files with kubectl can be done over and over as you update the YAML file. Only the delta will be changed.
- **NOTE:** The Kubernetes documentation site is your friend. The full YAML specs can be found there: <https://kubernetes.io/docs>
- Find out the External IP that was assigned to your service. You can use kubectl for this or the Azure portal.
- Test the application by browsing to the Web app's external IP and port and seeing the front page come up.
	- Ensure that you see a list of both speakers and sessions on their respective pages.
	- If you don't see the lists, then the web app is not able to communicate with the API app.

## Success Criteria

1. You have the **content-api** container image deployed and can get data from the `/speakers` endpoint.
1. You have the **content-web** container deployed and can access its page from the open internet.
1. The `/speakers` and `/sessions` pages display speakers and sessions respectively, not just blank pages.

## Additional Reading
- https://goglides.io/clusterip-nodeport-and-loadbalancer-service-types-in-kubernetes/98/
- https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
