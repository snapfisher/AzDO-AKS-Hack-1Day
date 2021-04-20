# Intro To Azure DevOps Pipelines & AKS Hack -- 1 Day Version

## Introduction
This intro level hack will help you get hands-on experience with Docker, Kubernetes and the Azure Kubernetes Service (AKS) on Microsoft Azure, as well as creating Azure DevOps Pipeline for use in deploying containerized code to AKS. Kubernetes has quickly gone from being the shiny new kid on the block to the defacto way to deploy and orchestrate containerized applications.

Being a one day event, this hack will focus on the use of AKS and the creation of Azure DevOps pipelines.  There is a tremendous amount of additional information which you will need to become proficient with each tool.  Specifically, we are not going to dwell much on how to containerize application.  Nor are we going to spend time on the other pieces of functionality within Azure DevOps, including Azure Boards, the creation and management of PRs, and many, many more topics.  

## Learning Objectives
In this hack you will solve a common challenge for companies migrating to the cloud. You will take a simple multi-tiered web app, containerize it, and deploy it to an AKS cluster. Once the application is in AKS, you will learn how scale it and then use Azure DevOps Pipelines to deploy the code.  We will also take a look at Private AKS Clusters, where all of the cluster is protected from the internet.

## Challenges
- Challenge 0: **[Pre-requisites - Ready, Set, GO!](Student/00-prereqs.md)**
   - Prepare your workstation to work with Azure, Docker containers, and AKS
   - Challenge 1: **[Got Containers?](Student/01-containers.md)**
   - Package the "FabMedical" app into a Docker container and run it locally.
- Challenge 2: **[The Azure Container Registry](Student/02-acr.md)**
   - Deploy an Azure Container Registry, secure it and publish your container.
- Challenge 3: **[Introduction To Kubernetes](Student/03-k8sintro.md)**
   - Install the Kubernetes CLI tool, deploy an AKS cluster in Azure, and verify it is running.
- Challenge 4: **[Your First Deployment](Student/04-k8sdeployment.md)**
   - Pods, Services, Deployments: Getting your YAML on! Deploy the "FabMedical" app to your AKS cluster. 
- Challenge 5: **[Scaling and High Availability](Student/05-scaling.md)**
   - Flex Kubernetes' muscles by scaling pods, and then nodes. Observe how Kubernetes responds to resource limits.
- Challenge 6: **[CI in Azure Devops Classic](Student/06-CI-in-Azure-Devops-Classic.md)**
   - Create a CI workflow to build containers into the ACR using ACR Tasks
- Challenge 7: **[CD in Azure DevOps Classic](Student/07-CD-in-Azure-DevOps-Classic.md)**
   - Create a CD workflow to deploy from the ACR into the AKS Cluster
- Challenge 8: **[CI-CD in AzureDevOps Yaml](Student/08-CI-CD-in-Azure-DevOps-Yaml.md)**
   - Create a Yaml based workflow to handle both CI and CD
- Challenge 9: **[Private AKS Clusters](Student/09-privateaks.md)**
   - Create an AKS cluster deployment which has no public endpoints
   
## Prerequisites

- Access to an Azure subscription with Owner access
   - If you don't have one, [Sign Up for Azure HERE](https://azure.microsoft.com/en-us/free/)
- See the Challenge 0 documentation for the rest of the prerequisites.  Challenge 0 should be done as prework before you arrive at the hack

## Repository Contents
- `../Student/Resources`
   - FabMedial app code and sample templates to aid with challenges

## Contributors
Initially based on topics from [What the Hack](https://github.com/microsoft/WhatTheHack).  Modified/updated by Larry Claman and Paul Fisher.
