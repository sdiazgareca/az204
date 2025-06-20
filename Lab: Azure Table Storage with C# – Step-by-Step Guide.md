# **Lab: Azure Table Storage with C# – Step-by-Step Guide**  

## **Objective**  
Learn how to:  
✅ **Create an Azure Storage Account & Table**  
✅ **Perform CRUD operations using C# SDK**  
✅ **Query data with LINQ and REST**  

---

## **Prerequisites**  
- **Azure Account** (Free tier works)  
- **.NET SDK** (6.0+)  
- **Azure CLI** (or PowerShell)  
- **Visual Studio / VS Code**  

---

## **Step 1: Set Up Azure Storage Account**  

### **1.1 Create a Resource Group**  
```bash
az group create --name AzureTablesLab --location eastus
```

### **1.2 Create a Storage Account**  
```bash
az storage account create \
    --name aztableslab<YOUR_UNIQUE_ID> \  # e.g., aztableslab123
    --resource-group AzureTablesLab \
    --sku Standard_LRS \
    --kind StorageV2
```

### **1.3 Get Connection String**  
```bash
az storage account show-connection-string \
    --name aztableslab<YOUR_UNIQUE_ID> \
    --resource-group AzureTablesLab
```
📌 **Save this for C# code!**  

---

## **Step 2: Create a .NET Console App**  

### **2.1 Initialize Project**  
```bash
dotnet new console -n AzureTableDemo
cd AzureTableDemo
dotnet add package Azure.Data.Tables
```

### **2.2 Update `Program.cs`**  
```csharp
using Azure.Data.Tables;
using Azure;

var connectionString = "<YOUR_CONNECTION_STRING>";
var tableName = "Customers";

// Initialize Table Client
var tableClient = new TableClient(connectionString, tableName);
await tableClient.CreateIfNotExistsAsync();

// Insert an entity
var customer = new TableEntity("Resellers", "Contoso")
{
    { "Email", "contoso@example.com" },
    { "IsActive", true }
};
await tableClient.AddEntityAsync(customer);
Console.WriteLine("Entity inserted!");
```

---

## **Step 3: CRUD Operations**  

### **3.1 Insert Entities**  
```csharp
var customers = new List<TableEntity>
{
    new("Resellers", "Fabrikam") { { "Email", "fabrikam@example.com" }, { "IsActive", false } },
    new("Sellers", "Adventure") { { "Email", "adventure@example.com" }, { "IsActive", true } }
};

foreach (var cust in customers)
    await tableClient.AddEntityAsync(cust);
```

### **3.2 Query Entities (LINQ)**  
```csharp
// Get all active customers
var queryResults = tableClient.Query<TableEntity>(e => e.GetBoolean("IsActive") == true);
foreach (var entity in queryResults)
    Console.WriteLine($"Active Customer: {entity.RowKey}, Email: {entity["Email"]}");
```

### **3.3 Update an Entity**  
```csharp
var entityToUpdate = (await tableClient.GetEntityAsync<TableEntity>("Resellers", "Contoso")).Value;
entityToUpdate["Email"] = "updated@contoso.com";
await tableClient.UpdateEntityAsync(entityToUpdate, ETag.All);
```

### **3.4 Delete an Entity**  
```csharp
await tableClient.DeleteEntityAsync("Resellers", "Contoso");
```

---

## **Step 4: Advanced Queries (OData/REST)**  

### **4.1 Query via REST (Using SAS Token)**  
```bash
# Generate SAS Token (CLI)
az storage table generate-sas \
    --name Customers \
    --account-name aztableslab<YOUR_ID> \
    --permissions r \
    --expiry 2025-01-01
```
📌 Use the SAS in a REST URL:  
```
https://aztableslab<YOUR_ID>.table.core.windows.net/Customers()?$filter=IsActive%20eq%20true&$format=json&<SAS_TOKEN>
```

### **4.2 Query with C# (OData Syntax)**  
```csharp
var query = tableClient.Query<TableEntity>(filter: "IsActive eq true");
foreach (var entity in query)
    Console.WriteLine($"{entity.RowKey} is active.");
```

---

## **Step 5: Cleanup**  

### **5.1 Delete Table (C#)**  
```csharp
await tableClient.DeleteAsync();
```

### **5.2 Delete Resources (CLI)**  
```bash
az group delete --name AzureTablesLab --yes
```

---

## **Conclusion**  
✔ **Created & Managed Azure Table Storage**  
✔ **Performed CRUD operations in C#**  
✔ **Queried data with LINQ & OData**  

🚀 **Next Steps**:  
- Try **batch operations** (`SubmitTransactionAsync`)  
- Explore **Cosmos DB Table API** for global scaling  

Would you like a **diagram** of the architecture or a **troubleshooting guide**? Let me know! 🎯
