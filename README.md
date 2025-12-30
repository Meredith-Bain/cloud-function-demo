# cloud-function-demo

Demonstrating CI/CD structure for cloud functions using Cloud Build in GCP

As an alternative to GitHub Actions, you can use Cloud Build to automatically deploy services in GCP. This repo demonstrates how to set up a Cloud Run Service (formerly Cloud Function)

## Setup

### Connect your GitHub Repository to Cloud Build
In order to use a GitHub commit to trigger a cloud build, you will need to connect your repo to Cloud Build using the following documentation: https://docs.cloud.google.com/build/docs/automating-builds/github/connect-repo-github?generation=1st-gen

### Set Up a Build Trigger
Use the following documentation to set up a build trigger. This is the service that listens for commits to your GitHub repo, and re-builds the Cloud Run Service if there are any changes. 

https://docs.cloud.google.com/build/docs/automating-builds/create-manage-triggers#build_trigger

### Edit the Run Service code in main.py
In this repo, edit the code in __main.py__ to fit the parameters needed for your application.

### Edit the configuration in cloudbuild.yaml
The file __cloudbuild.yaml__ contains the instructions for your Cloud Build Trigger, including what to name the Run Service, where the code for the service is located, how the service should be triggered, and what runtime should be used to set up the code. The instructions in the Cloudbuild file are the same as what you might type into the command line if you are using the gcloud CLI. 

Change the line _-hello\_world_ to change the name of the Run Service. 

Runtime, trigger, memory capacity, and other options for the __cloudbuild.yaml__ file can be found at https://docs.cloud.google.com/sdk/gcloud/reference/run/deploy

## Running multiple Cloud Run Services from one repository
It is often desirable to have a suite of services or jobs in CI/CD that are contained in a single repository. You can achieve this by making the following changes:
1. Put the main.py and cloudbuild.yaml files for a single service in their own directory in the repo (this repo must still be connected to Cloud Build via the first setup instruction above)
2. Modify your Build Trigger to only look for changes in the directory containing the service-specific directory (in the Build Trigger setup widget, in the _included files_ prompt, type in __your directory_name/\**__. You will also have to specify the cloudbuild.yaml file by typing in __your directory_name/cloudbuild.yaml__ where it asks for the address of your YAML file). 