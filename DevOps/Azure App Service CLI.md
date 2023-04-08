# What is Azure App Service? Explain how App Service works and how it can be used to host web applications. How do you create and deploy a web application to Azure App Service using Azure CLI or Azure Portal? Provide an example of a deployment script.

Azure App Service is a platform-as-a-service (PaaS) offering from Microsoft Azure that enables users to quickly build, deploy, and scale web applications and APIs. It supports a variety of programming languages and frameworks, including .NET, Java, Node.js, Python, and PHP.

When deploying a web application to Azure App Service, there are a few steps involved:

1. Create an App Service plan: An App Service plan is a container for hosting web apps. It defines the computing resources (such as CPU, memory, and storage) that will be allocated to your web app.

2. Create an App Service instance: An App Service instance is a logical container for your web app. It is hosted within an App Service plan and is accessible via a unique URL. You can create an App Service instance using the Azure Portal, Azure CLI, or other tools.

3. Deploy your web app: There are several ways to deploy a web app to Azure App Service, including:
    - Using FTP or SFTP to upload your web app files
    - Using Azure CLI or Azure Portal to deploy directly from a Git repository
    - Using Azure DevOps or other continuous integration/continuous deployment (CI/CD) tools to automate the deployment process

Here's an example of a deployment script that uses Azure CLI to deploy a Node.js web app to Azure App Service:

```bash
# Set the resource group and app name variables
RESOURCE_GROUP=my-resource-group
APP_NAME=my-app-name

# Create a resource group
az group create --name $RESOURCE_GROUP --location eastus

# Create an App Service plan
az appservice plan create --name my-plan --resource-group $RESOURCE_GROUP --sku S1 --is-linux

# Create an App Service instance
az webapp create --name $APP_NAME --plan my-plan --resource-group $RESOURCE_GROUP --runtime "NODE|14-lts"

# Configure deployment settings
az webapp deployment source config --name $APP_NAME --resource-group $RESOURCE_GROUP --branch master --repository-url https://github.com/myusername/myrepo.git

# Restart the app
az webapp restart --name $APP_NAME --resource-group $RESOURCE_GROUP
```

This script creates a resource group, an App Service plan, and an App Service instance, and then configures the deployment settings to pull the web app code from a Git repository. Finally, it restarts the app to apply the changes.