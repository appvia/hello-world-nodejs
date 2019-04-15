# hello-world-nodejs

This is an example hello world application that covers off an iterative exemplar of:
1. How to do CI and test with CircleCI
2. How to build and push a docker image
3. How to deploy the latest build to Kubernetes

### What we Use

In terms of speeding up the provisioning of resources we use the [Appvia Hub](https://github.com/appvia/appvia-hub). 
This creates:
- The Code repository for your app
- The Docker repository for your application with a robot token
- A kubernetes Namespace with RBAC and CI robot token

We can then use the above to iterate and deploy our application. 

In the absence of the above, you will need to manually provision all three for this to work accordingly and keep a record of the robot tokens you are provisioning for CI to use.

### What this Contains

This repository contains:

- A node application which has a unit test
- A circleci directory with configuration
- A kubernetes deployment directory with the deployment files using a loadbalancer service

### CircleCI

If you are not familiar with CircleCI, please familiarise yourself [with the docs](https://circleci.com/docs/). For nodejs, there are examples around [javascript on circleci](https://circleci.com/docs/2.0/language-javascript/) which may offer some guidance to what we have produced.

Essentially the circleci job is going to get triggered and specific branch pushes and selectively will build and deploy a new docker image when there is a merge to master. The credentials are used as [environment variables](https://circleci.com/docs/2.0/env-vars/) and will be injected into the jobs accordingly. 

### Kubernetes Deployment

In a similar situation to CircleCI Quay docker image builds and pushes, we are using the environment variables of the robot service account, to deploy continuously into the defined namespace when an image is merged to master. We have a dependency chain in the workflow, so that we:
- Test code before we build a docker image
- Build a docker image and push it to quay when someone merges to master
- Trigger a kubernetes deployment into the relevant namespace of the newly built artefact

To be able to deploy to Kubernetes, we need to define a set of variables and also control the event to only happen on a merge master, so that it is a controlled process by the team as to what is deployed. 

