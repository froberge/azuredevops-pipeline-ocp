[![Build Status](https://dev.azure.com/redhat-demo/demo-stm/_apis/build/status/demo-stm?branchName=main)](https://dev.azure.com/redhat-demo/demo-stm/_build/latest?definitionId=1&branchName=main)

# Introduction 
Simple MVC .NET Core application that was fork from [Red Hat Developer](https://github.com/redhat-developer/s2i-dotnetcore-ex). 

This code is use to demmonstrate the creation a CI/CD Azure Devops pipeline with a deployment to OpenShift. 
Refect to the project [README documentation](README.adoc) for more info on the actual .NET core project.

# Getting Started
Here are the requirement to get this project started.
1.	Make sure your your azure devops organization as parallelism grant.
2.	Have access to [quay](quay.io).
3.	Have an OpenShift installation working and ready.
4.  Create and Openshift project and a Service Account.
5.	Install the [Openshift extension](#Install-OpenShift-extension).


# Create a service account
1. Go to the project you need the service account and run this command.
```
oc create sa [service-account-name]
```

2. Add the role to the service account. In this case we will add the admin role.
```
oc adm policy add-role-to-user admin -z [service-account-name] -n [project]
```

3. Validate role
```
 oc policy who-can [verb] [resource] -n [project]
 ```
 verbs:get, list, create, update, delete, deletecollection, watch

 4. Find the token
 ```
 oc describe sa [service-account-name] -n [project]
 ```

5. Retrieve the token
```
 oc describe secret [token-name] -n [project]
```


# Install OpenShift extension
Guide line how to install the openshift extension
1.  Go to your orgazization page.
2.  Browse the marketplace and find Openshift extension.
3.  Click on Get it free.
4.  Select the organization for which you want to install the extension and click install.
5.  Connect your OpenShift cluster, by creating a service connection.
    * Go to Project Setting.
    * Select Service Connections.
    * Create a New service connection, Select connection type: OpenShift.
    * Connect using the Token authentication method.
    * Use the credential from the service account created to connect to the Openshift project.

# Connect to Quay or Container Registry
Guide to how to connect to an external container registry.

* Go to Project Setting.
* Select Service Connections.
* Create a New service connection, Select connection Type: Docker Registry
* Enter the information for quay.
    * https://quay.io
    * The service account credential for the repository.

# Build and Test

## Option 1  -Run pipeline
The application can be build and test using the azure pipeline in the repository considering that the previous setup was made.

## Option 2 - Manual CI/CD mode
You can build, deploy and run the image  using the following steps.

1. go to the app folder
2. run the following docker command to build the image.
```
docker build -t [image_name] -f ../docker/Dockerfile.manual-build .
```
3. run the following command to run the image locally. the image will then be available at localhost:8080.
```
docker run -d -p 8080:8080 --name [container_name] [image_name]
```
*optional push image to a container registry and onto the desirer container platform* 

1. Tag the image
2. Push to image to the container registry
3. Deploy the image rom the registry to the desired platform
