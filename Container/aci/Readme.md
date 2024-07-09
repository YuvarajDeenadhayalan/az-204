Deploy a container instance by using the Azure CLI

# Run containerized tasks with restart policies
az container create --resource-group myResourceGroup --name mycontainer --image mycontainerimage --restart-policy OnFailure

# Set environment variables in container instances
az container create --resource-group myResourceGroup --name mycontainer2 --image mcr.microsoft.com/azuredocs/aci-wordcount:latest  --restart-policy OnFailure --environment-variables 'NumWords'='5' 'MinLength'='8'

# Create container using yaml file
az container create --resource-group myResourceGroup --file secure-env.yaml

# Mount an Azure file share in Azure Container Instances. The --dns-name-label value must be unique within the Azure region where you create the container instance. Update the value in the preceding command if you receive a DNS name label error message when you execute the command.
az container create --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles --image mcr.microsoft.com/azuredocs/aci-hellofiles --dns-name-label aci-demo --ports 80 --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME --azure-file-volume-account-key $STORAGE_KEY --azure-file-volume-share-name $ACI_PERS_SHARE_NAME --azure-file-volume-mount-path /aci/logs/

# To use a template or YAML file, provide the share details and define the volumes by populating the volumes array in the properties section of the template.

# For example, if you created two Azure Files shares named share1 and share2 in storage account myStorageAccount, the volumes array in a Resource Manager template would appear similar to the mount-multiple-volumes-arm.json first array

# Next, for each container in the container group in which you'd like to mount the volumes, populate the volumeMounts array in the properties section of the container definition. For example, the 2nd array in mount-multiple-volumes-arm.json mounts the two volumes, myvolume1 and myvolume2, previously defined:
