rules - an array of rule objs -> Min 1, max 100 in a policy
name - string - upto 256 chars - case-sensitive. must be unique within a policy
enabled - bool - default true - not a mandatory param
type - an enum value - current valid type is LifeCycle 
definition - An obj that defines the lifecycle rule - each defn is made up of a filter and action set

# blob-lifecycle-rules.json file,
Tier blob to cool tier 30 days after last modification
Tier blob to archive tier 90 days after last modification
Delete blob 2,555 days (seven years) after last modification
Delete blob snapshots 90 days after snapshot creation

# CLI command to create lifecycle mgmt policy 
az storage account management-policy create \
    --account-name <storage-account> \
    --policy @policy.json \
    --resource-group <resource-group>

# class Description
BlobServiceClient -> Represents SA. provides operns to retrieve and configure acc props, and to work with blob containers in the SA
BlobContainerClient -> Reps a sp blob cont, and provides operns to work with the cont and the blobs within
BlobClient -> Reps a sp blob, and provides general operns to work with the blob, inc operns to upload, download, delete and create snapshots
AppendBlobClient -> Reps an append blob, and provides operns sp to append blobs, such as appending log data
BlockBlobClient -> Reps a block blob, and provides operns sp to block blobs, such as staging and then commiting blocks of data

# Packages to work with Blob storage data res'es
Azure.Storage.Blobs: Contains te primary class(client objs) that u can use to operate on the service, containers and blobs
Azure.Storage.Blobs.Specialized: Contains classes that u can use to perform operns sp to a blob type, such as block blobs
Azure.Storage.Blobs.Models: All other utility classes, structures, and enum types

# If your work is narrowly scoped to a single container, you might choose to create a BlobContainerClient object directly without using BlobServiceClient.

# Exercise
az login
# create RG
az group create --location <myLocation> --name az204-blob-rg
# create SA
az storage account create --resource-group az204-blob-rg --name <myStorageAcct> --location <myLocation> --sku Standard_LRS
# Select access keys in Security+N/w'ing --> take access keys and complete conn string of each key
# find conn string under key1, copy this

# Prepare .net proj
# create console proj
dotnet new console -n az204-blob
dotnet build
mkdir data
dotnet add package Azure.Storage.Blobs
dotnet run

To clean up
az group delete --name az204-blob-rg --no-wait

