# Microsoft .NET SDK v3 for Azure Cosmos DB
# Create cosmos client
CosmosClient client = new CosmosClient(endpoint, key);
# Create a database
// An object containing relevant information about the response
DatabaseResponse databaseResponse = await client.CreateDatabaseIfNotExistsAsync(databaseId, 10000);
# Read a database by ID
DatabaseResponse readResponse = await database.ReadAsync();
# Delete a database
await database.DeleteAsync();
# Create a container
// Set throughput to the minimum value of 400 RU/s
ContainerResponse simpleContainer = await database.CreateContainerIfNotExistsAsync(
    id: containerId,
    partitionKeyPath: partitionKey,
    throughput: 400);
# Get a container by ID
Container container = database.GetContainer(containerId);
ContainerProperties containerProperties = await container.ReadContainerAsync();

# Delete a container
await database.GetContainer(containerId).DeleteContainerAsync();

# Item examples
# Create an item
ItemResponse<SalesOrder> response = await container.CreateItemAsync(salesOrder, new PartitionKey(salesOrder.AccountNumber));

# Read an item
string id = "[id]";
string accountNumber = "[partition-key]";
ItemResponse<SalesOrder> response = await container.ReadItemAsync(id, new PartitionKey(accountNumber));

# Query an item
QueryDefinition query = new QueryDefinition(
    "select * from sales s where s.AccountNumber = @AccountInput ")
    .WithParameter("@AccountInput", "Account1");

FeedIterator<SalesOrder> resultSet = container.GetItemQueryIterator<SalesOrder>(
    query,
    requestOptions: new QueryRequestOptions()
    {
        PartitionKey = new PartitionKey("Account1"),
        MaxItemCount = 1
    });

https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage

# Create resources by using the Microsoft .NET SDK v3

az login
az group create --location <myLocation> --name az204-cosmos-rg
# Create the Azure Cosmos DB account
az cosmosdb create --name <myCosmosDBacct> --resource-group az204-cosmos-rg
# Record document endpt - https://yuvitestcosmosdbacct.documents.azure.com:443/
# Retrieve the primary key for the account - take primaryMasterKey
az cosmosdb keys list --name <myCosmosDBacct> --resource-group az204-cosmos-rg

# create .net proj
dotnet new console
# add package
dotnet add package Microsoft.Azure.Cosmos

# triggers
# pretriggers ->  can't have any input parameters.
