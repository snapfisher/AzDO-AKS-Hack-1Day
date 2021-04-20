# Challenge 6: CI in Azure Devops Classic

[< Previous Challenge](./05-scaling.md) - **[Home](../README.md)** - [Next Challenge >](./07-CD-in-Azure-DevOps-Classic.md)

## Introduction

Azure DevOps Pipelines are a full featured build and deployment engine.  In these next few challenges, we'll learn the basics of setting up CI/CD within Azure DevOps to build to an ACR and Deploy to an AKS cluster.  We'll be using Azure DevOps service, which is a lcoud based system.  Microsoft also has a product called Azure DevOps Server, which can be deployed on-prem, and is effectively the same system, but 6 months behind.

In Azure DevOps, there are actually two separate user interfaces for CI/CD.  The first is called "classic" pipelines, and this provides a full GUI for specifying the steps in your CI/CD workflows.  The second are called "Yaml" pipelines, as they are written in yaml, which is a descriptive format -- like json, but using spaces vs. curly braces.  We'll take a look at both methodologies over the next few challenges.  For this challence, we'll set up a classic CI workflow that takes the code and builds it into our ACR.  We're not actually going to have to install docker anywhere.  The ACR has a feature called ACR Tasks which will allow the ACR to actually build the container for us.  We'll use this feature here.

## Azure DevOps Setup -- Project

We need to create an Azure DevOps Organization.  If you have an organization with Pipelines enabled, you can use that.  As mentioned in the prereq stage, you can create a free Azure DevOps organization, and it might have Pipelines enabled, but if not you will have to call Microsoft support to enable pipelines.  This is fine, but not something we want to spend time on today.  If you do not have an organization, your instructor can give you one to use for the day, along with a username/password.

The advantage of using your own organization for this hack is that you will be able to keep the work when the hack is complete.  You can create a free Azure DevOps organization here: [Azure Devops](https://dev.azure.com)

In the organization, you will need to create a new project.  This can be a private or public project.  If you are working in a team and with an Azure DevOps organization supplied by the instructor, up to 5 people can be added to each project.  This is the free organization limit (although many more stakeholders could be added, but this is outside the bounds of our hack).  If you are using a corporate/paid Azure DevOps subscription, the number of people who can be added to the project, may be much greater.  

## Azure DevOps Setup -- Code

For the next few challenges, we have two options, as we'll need the code and Dockerfiles from Challenge 0 brought into Azure DevOps.  You'll also want the final .yml deployment files that you finished with in Challenge 4.

1. We could import this repo directly into Azure DevOps (and then manually add the .yml files from Challenge 4)  Azure DevOps repositories provides a full git repo for your needs, and this will make a copy of the code for your use.  Since this repo is public, you will not need to authenticate.  
1. However, a better solution is to not copy the code, but just store the code in GitHub and link the two together.  This will allow Azure DevOps Pipelines to just pull the code out of the GitHub repo, when needed, and a single point of truth remains.  This is also advantageous since GitHub repos contain features like Dependabot and Advanced Security, which gives GitHub repos an extremely high value when used in conjunction with Azure DevOps Pipelines.

For the first option, you will find an import button if you click on the Repos->Files button from the left hand menu in your Azure DevOps project.  You will want to use the clone URL from this repo's GitHub page.

For the second option, you will need a GitHub account.  You can create one, or use one that you already have.  You can create it here: [GitHub](https://github.com).  You will want to fork this repo into your own namespace, as you will need to add the Dockerfiles and completed yml files from challenge zero.  

Once this repo is forked, in Azure DevOps click on Project Settings on the lower left menu.  Here you will see the configuration screen.  Click on Service Connections and create a new connection to your GitHub Environment.

## Set Up the CI Workflow

In addition to the above, we will need to give Azure DevOps a service connection to your Azure Environment.  From the project menu, click on Project Settings in the lower left corner and then click on the Service Connections button.  You want to create an Azure Resource Manager connection.  Actual permission is granted through a service principal from your Azure environment.  If you are using the same identity in Azure DevOps as you are in Azure, then Azure DevOps can set this up for you automatically.  If you are not, you need to first create the service principal in your Azure tenant and then use that to create a manual connection.  Either way, once you do this, be sure to leave the "Grant access permission to all pipelines" and then you will be set for all the work you do in this hack.  The easiest way to create a service perincipal is to use the Azure CLI.  (Hint: az ad sp create-for-rbac)

Note that if you are using the same identity in Azure DevOps and Azure, you actually could skip the service connection step, as Azure will allow you to connect using your subscription directly.  However, doing this can be problematic in a real scenario, as all users would be using your identity for deployment.  This can also be a problem if the workflow needs to be changed and you are not around to do the work, personally.  So, let's create that service principal!

Once you have the service principal set up, go and create your first pipeline.  You will want to make sure that you choose a "classic" pipeline, which will be a choice at the bottom of the list.

You will want to create your pipeline to pull from your repo, whether it is the Azure DevOps repo or the GitHub repo.  The easiest way to specify a build against Azure ACR Tasks is to use the Azure CLI from the pipeline workflow (hint: az acr build).  The CLI task allows you to create a CLI script directly within the user interface.  Although workflow design is as much art as ascience, you could be able to finish this challenge using a workflow with only a single step.

Once it is working, enable CI for the workflow, so everytime the code is updated, the workflow should run.

## Success Criteria

1. You have a classic pipeline that fires automatically whenever code is checked into the repo.
1. The pipeline uses Azure ACR Tasks, and the code/Dockerfile from challenge 1 to build containers into your ACR.

## Reading:

[Azure CLI ad commands](https://docs.microsoft.com/en-us/cli/azure/ad?view=azure-cli-latest)

[Azure CLI acr commands](https://docs.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest)

[Azure Pipelines Documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)