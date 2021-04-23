# Challenge 8: CI-CD in AzureDevOps Yaml

[< Previous Challenge](./07-CD-in-Azure-DevOps-Classic.md) - **[Home](../README.md)** - [Next Challenge >](./09-privateaks.md)

## Introduction

In the previous two challenges, we created a build pipeline and a classic release.  Both of these require the user/develoepr to log into Azure DevOps in order to make changes to the build/deployment workflow.  However, a second method is also available in Azure DevOps.  This is to use a yaml pipeline, more specifically, a multi-stage yaml pipeline to handle both building and deploying.

Yaml is a describtive syntax, similar to Json.  A yaml pipeline is a workflow file that is stored within your code repository.  Azure DevOps can then implement the workflow defined within the yaml file.  The benefit of this is that developers can alter the build without having to log into Azure DevOps.  The pipeline definition becomes part of your code base.

## Creating the Pipeline


```
trigger:
- main

resources:
- repo: self

variables:

  # Container registry service connection name
  dockerRegistryServiceConnection: 'connection name'
  # Other Variables go here.....

  # Agent VM image name
  vmImageName: 'ubuntu-latest'


stages:
- stage: Build
  displayName: Build stage
  jobs:
  - job: Build
    displayName: Build
    pool:
      vmImage: $(vmImageName)
    steps:

    # Steps go here

    # If you use a publish step, you can pick up the file in later stages, for example the
    # following publish step can be retrieved in the deploy stage with
    # $(Pipeline.Workspace)/dropapi/content-api-deploy-solution-sa.yml
    - publish: $(System.DefaultWorkingDirectory)/Student/Resources/Challenge 0/content-api/content-api-deploy-solution-sa.yml
      artifact: dropapi

    # More steps as the API and Web will need to be worked out separately
    - publish: $(System.DefaultWorkingDirectory)/Student/Resources/Challenge 0/content-web/content-web-deploy-solution-sa.yml
      artifact: dropweb

- stage: Deploy
  displayName: Deploy stage
  dependsOn: Build

  jobs:
  - deployment: Deploy
    displayName: Deploy
    pool:
      vmImage: $(vmImageName)
    environment: 'EnvironmentName'
    strategy:
      runOnce:
        deploy:
          steps:

            # Steps go here

```

## Success Criteria

1. x


## Reading:

[Azure DevOps Yaml Schema Reference](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema)

[Jobs in Azure DevOps Yaml Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml)