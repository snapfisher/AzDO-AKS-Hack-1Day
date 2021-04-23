# Challenge 7: CD in Azure DevOps Classic

[< Previous Challenge](./06-CI-in-Azure-Devops-Classic.md) - **[Home](../README.md)** - [Next Challenge >](./08-CI-CD-in-Azure-DevOps-Yaml.md)

## Introduction

In the previous challenge, we have set up a build workflow to containerize our code into our Azure Container Repository.  In this challenge we are going to delploy the containers to our cluster.  We are going to continue with the "classic" mode and use a Pipeline Release.

## Set Up the CD Workflow

If we think back to the earlier challenges, the Azure Container Repository is already integrate with our Azure Kubernetes Service cluster.  If we were running from the command line, we would just make a kubectl.exe call to apply a configuration, both for the deployment and the service, for both the API and Web pieces.

Before you start, on the command line remove the running deployment from the AKS cluster.  In theory, we would also remove the service components, but since we are time limited for this hack, let's just leave them in the cluster as is.  The new deployments can use the existing services.

You'll notice when setting up a release, that you can have it read from different sources, such as the build results from your CI build or directly from your repo -- or both!.  Once you are done configuring the release, be sure to set it up for CD, so that it runs every time the CI build completes -- not everytime the baseline is updated, as we would always want to make sure that the new containers are available before we start the deployment step.

Additionally, spend some time taking a look at the Release Definition page.  You will see that your release has an artifacts section, from where the code/artifacts are coming from, and a single stage.  You could add additional stages to run in serial or parallel, only limited by your Azure DevOps licensing.  For this challenge, we are not going to add any additional stages, but let's examine how to add a manual approval before the current deployment.

## Connecting to the Cluster

If you are connecting within Azure DevOps to a kubernetes cluster -- whether it is AKS or another flavor of kubernetes, you need permission in order to access the cluster.  As in the previous challenge, this is done with a service connection.  Once the service account is set up, you will reference it from the Release steps, in a very similar manner as you did with the pipeline steps.

1. If your identity in Azure DevOps and your identity in Azure are the same, then this is the easy(ier) path.  As in the previous challenge, from the service connections page, create a new connection to the cluster.  This will be a "Kubernetes" connection, and you will want to set it's type to "Azure Subscription".
1. If your identity in Azure DevOps is different than your Azure identity, you will need to connect to the cluster through a service account configured on the cluster itself.  So the first thing you will need to do is create the service account.  This will require you to create a service account manifest file.  You also need to bind the account to a rolebinding configured for edit (hint: --clusterrole=edit)
  1. Once that is done, you will need to add specific information into the new service connection dialog in Azure DevOps
    1. Server url: kubectl.exe config view --minify -o "jsonpath={.clusters[0].cluster.server}"
    1. Secret step one: kubectl.exe get serviceAccounts humanaserviceaccount -o=jsonpath={.secrets[*].name}
    1. Secret step two: kubectl.exe get secret {output from step 1} -o json

## Success Criteria

1. Set up a classic release that deploys (at least the) deployments to your AKS cluster.
1. (optional) Have the classic release also deploy the services to your AKS cluster.
1. Have the deployments use the containers that were build in the previous challenge
1. Set up the release to use CD and deploy every time the CI build is successful
1. Have the release pause until someone gives it manual approval to proceed
1. (optional) Have Azure DevOps alert you that a release is ready for approval.  Note: you will NOT be able to do this if you are using an MTC supplied Azure DevOps account, as you will not have access to the email.  You can have the email sent, but you will not receive it.


## Reading:

[Kubectl Task](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/deploy/kubernetes?view=azure-devops)

[Azure DevOps Release Gates and Approvals](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/approvals/?view=azure-devops)

[Configure Service Accounts for Kubernetes Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

[Creating Kubernetes Rolebindings](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-rolebinding)