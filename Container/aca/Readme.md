# In Azure Cloud shell, Install the Azure Container Apps extension for the CLI.
az extension add --name containerapp --upgrade

# Register the Microsoft.App namespace. Azure Container Apps resources have migrated from the Microsoft.Web namespace to the Microsoft.App namespace.
az provider register --namespace Microsoft.App

# Register the Microsoft.OperationalInsights provider for the Azure Monitor Log Analytics workspace if you haven't used it before
az provider register --namespace Microsoft.OperationalInsights

# Set environment variables used later in this exercise. Replace <location> with a region near you.
myRG=az204-appcont-rg myLocation=<location> myAppContEnv=az204-env-$RANDOM

# Create the resource group for your container app.
az group create \
    --name $myRG \
    --location $myLocation

# Create an environment by using the az containerapp env create command.
az containerapp env create \
    --name $myAppContEnv \
    --resource-group $myRG \
    --location $myLocation

# Deploy a sample app container image by using the containerapp create command.
az containerapp create \
    --name my-container-app \
    --resource-group $myRG \
    --environment $myAppContEnv \
    --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest \
    --target-port 80 \
    --ingress 'external' \
    --query properties.configuration.ingress.fqdn

# Clean up resources
az group delete --name $myRG

# The code in aca-configuration.json is an example of the containers array in the properties.template section of a container app resource template. The excerpt shows some of the available configuration options when setting up a container when using Azure Resource Manager (ARM) templates. Changes to the template ARM configuration section trigger a new container app revision.