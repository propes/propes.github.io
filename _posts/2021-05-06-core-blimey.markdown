---
layout: post
title:  "Journeys into automated deployments 🚀"
date:   2021-05-06 12:21:00 +0100
tag: automated deployment
---
Recently I was involved in building a microservices api which was set up with a fully automated CI/CD pipeline that deployed to an AKS cluster. This was a new and exciting experience for me as it was the first time I'd been involved directly in devops and it motivated me to write this post...

Something I've been wanting to learn for a while now is how to set up a fully automated CI/CD pipeline that deploys to a cloud service. To make things simple I decided to start with Azure because I have some familiarity with it and initially I'm going to start with deployments to Azure App Service because it's simple and probably the most logical place to start. In future posts I might explore deploying to other platforms such as AKS.

This post details how to set up an automated pipeline starting from the beginning and building up. Hopefully it will give you an understanding of how the pieces fit together and an insight into how I go about learning something like this.

For this exercise I'm just using the default web api you get from creating a new web api using the dotnet cli, i.e. `dotnet new webapi -n azure-pipelines-test`.

Running this api locally displays some dummy weather forecast data in json.

![Weather forecast screenshot](/assets/images/weather-forecast.png)

It's simple but it will serve the purpose of giving us something to deploy to Azure App Service.

First we need to set up a basic CI pipeline. Something that can build the code and run tests. Once we've got that in place we can worry about deploying it to an environment. For this I've started with the template for asp.net core provided by Microsoft, which can be found [here](https://github.com/microsoft/azure-pipelines-yaml/blob/master/templates/asp.net-core.yml).

In this case I've set the pipeline to be triggered by a main or master branch.

```yml
# azure-pipelines.yml
trigger:
- main
- master

variables:
  buildConfiguration: 'Release'

steps:
- script: dotnet build --configuration $(buildConfiguration)
  displayName: 'dotnet build $(buildConfiguration)'
```


Limitations
- Doesn't include different environments or stages in the pipeline.
