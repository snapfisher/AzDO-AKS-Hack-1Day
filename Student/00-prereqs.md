# Challenge 0: Pre-requisites - Ready, Set, GO! 

**[Home](../README.md)** - [Next Challenge >](./01-containers.md)

## Introduction

A smart cloud solution architect always has the right tools in their toolbox. 

## Description

In this challenge we'll be setting up all the tools we will need to complete our challenges.  This challenge is prework intended to be completed before you arrive for the hack.

### Personal Workstation
- Install the recommended toolset:
    - [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
    - [git](https://git-scm.com/downloads)
    - [Visual Studio Code](https://code.visualstudio.com/Download)
    - Visual Studio Code recommended extenstions:
      - Docker Extension
      - Kubernetes Extension
      - Remote SSH Extension

All of the challenges can be completed on a Linux workstation within the Bash Shell.  You can also do most of the challenges from a Windows desktop, however, some of the early challenges (at least challenge 1) will require you to containerize applications using docker and must be completed from a Linux workstation, since the containers used for this hack are Linux containers.  If you do not have a Linux workstation (which we assume), you have two choices:
- Install the Windows Subsystem for Linux, Version 2 in your Windows Laptop, and then use that for at least the Linux portions
- Use a Linux virtual machine in Azure for the Linux Portions.

These instuctions assume that you are using your (lcoal) laptop as the desktop machine, however, you can also create a new VM in Azure and use that for your acutal development desktop.  This means that all you will use your laptop for is to log into the Azure VM.

### Deploying Lunix and Docker on Windows with WSL2
If you choose to use the first option, instructions on installing WSL2 can be found here: [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
You will also need to install docker in the WSL2 subsystem: [Docker Installation](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)

You will then need to copy the code that you you will need into the Lunux subsystem.  There are two folders that you need: "Student/Resources/Chapter 0/content-api" and "Student/Resources/Chapter 0/content-web"

### Deploying and Access a Linux VM Build Server
If you choose to use the second option, we have already created a virtual machine template for your use, which also contains most of the code which will need to be containerized.  Once the machine is built you will need to copy the Dockerfiles from "Student/Resources/Chapter 0/content-api" and "Student/Resources/Chapter 0/content-web" onto the machine, although the rest of the code will already be there.

If you choose to use a VM in Azure, deploy the build machine VM with Linux + Docker using the provided ARM Template and parameters file in the "Student/Resources/Chapter 0/build-machine" folder of this repo.  If you clone this repo to your local machine and then install the Azure CLI, you can do this easily with the following script:
```
RG="akshack-RG"  #Change as appropriate
LOCATION="EastUS"  # Change as appropriate
az group create --name $RG --location $LOCATION
az deployment group create --name buildvmdeployment -g $RG \
    -f docker-build-machine-vm.json \
	-p docker-build-machine-vm.parameters.json
```
You will then need to ssh into the machine (which is using port 2266):

`ssh -p 2266 wthadmin@12.12.12.12 # replace 12.12.12.12 with the public IP of the vm`

Once you log into the linux machine, the only maintence you should need to do is to update the Azure CLI, use 'az upgrade' [CLI Upgrade](https://docs.microsoft.com/en-us/cli/azure/update-azure-cli)

### Azure DevOps

If we were going to mimic how you most probably would integrate Azure DevOps with AKS in a production environment, you would log into both Azure and Azure DevOps with the same identity.  However, we realize that there may be limitations to the environments you are using for this Hack.  Related to this, we normally would have all attendees create their own Azure DevOps environments.  Unfortunately, there has been a marked increase of people using the free version of Azure DevOps for CryptoCurrency mining.  To combat this, Microsoft was forced to limit the automatic granting of free Azure DevOps pipelines to new Azure DevOps organizations.  You can still access the free tier of pipelines for private projects, but you may have to call customer support to have it enabled.  This is unfortunate for a one-day hack.

However, we have existing Azure DevOps organizations which you can use for the hack.  We will distribute this information on the day of the hack.  You are still welcome to use your own Azure DevOps organizations, in which case you can keep your work when the hack is complete.  You can set up a new Azure DevOps organization here: [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/?nav=min), but for use in this hack, you need to ensure that Pipelines are enabled.  If you use our Azure DevOps organizations, we will need to disable your access at the end of the hack session.

## Success Criteria

1. You have a development desktop at your disposal (Windows, Mac, Linux)
1. You have a bash shell and docker (Linux) at your disposal (WSL, Mac, Linux)
1. Running `az --version` shows the version of your Azure CLI, and it is up to date
1. Running `docker ps` shows that docker is running 
1. Visual Studio Code is installed.
1. (Optional) Have a working Azure DevOps organization with Pipelines enabled.  If you do not, we will supply you with one to use for the day.
