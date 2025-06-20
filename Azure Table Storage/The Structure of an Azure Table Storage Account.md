### **Summary of "The Structure of an Azure Table Storage Account"**  

Azure Table Storage is a **PaaS NoSQL** solution provisioned within an **Azure Storage Account**, deployable via the **Azure Portal, PowerShell, Azure CLI, or ARM templates**.  

#### **Key Components:**  
1. **Account** – Provides an **HTTPS-enabled RESTful endpoint**.  
2. **Table** – A logical grouping of entities (e.g., customers, orders).  
3. **Entity** – A schema-less "row" containing properties (key-value pairs).  
   - **Mandatory Properties**:  
     - `PartitionKey` (logical grouping for scalability)  
     - `RowKey` (unique ID within a partition)  
     - `Timestamp` (last modification time)  
   - **Data Types**: Supports `string`, `int32`, `int64`, `decimal`, `GUID`, `Boolean`, and `binary`.  
   - **Flexible Schema**: Entities in the same table can have different property types (e.g., `StockCount` as `int32` or `string`).  

#### **Provisioning & Management (Exercise 1)**  
- **Create a Storage Account**:  
  ```bash
  az storage account create --name your-account --resource-group your-group
  ```  
- **Retrieve Access Key**:  
  ```bash
  key=$(az storage account keys list --account-name your-account --query [0].value -o tsv)
  ```  
- **Create a Table & Insert Entities**:  
  ```bash
  az storage table create --name customers --account-name your-account --account-key $key
  az storage entity insert --account-name $account --entity PartitionKey=ReSellers RowKey=Contoso IsActive=true
  ```  
- **Generate a Shared Access Signature (SAS)**:  
  ```bash
  sas=$(az storage table generate-sas --name customers --account-name $account --permissions r --expiry 2200-01-01)
  ```  

#### **Querying via RESTful Interface**  
- **OData Syntax**:  
  - Filter active customers:  
    ```
    https://myaccount.table.core.windows.net/customers()?$filter=IsActive%20eq%20true
    ```  
  - Retrieve by keys (case-sensitive):  
    ```
    https://myaccount.table.core.windows.net/Customers(PartitionKey='ReSellers',RowKey='Contoso')
    ```  
  - Operators: `lt` (`<`), `gt` (`>`), `eq` (`=`), etc.  

#### **C# SDK Integration**  
- **Key Classes**:  
  - `TableServiceClient`: Manages table/entity operations (CRUD).  
  - `TableEntity`: Represents an entity with `PartitionKey`, `RowKey`, and dynamic properties.  
- **Example Workflow**:  
  ```csharp
  var client = new TableClient(connectionString, "customerscode");
  client.CreateIfNotExists();
  ```  

#### **Summary**  
Azure Table Storage is a **cost-effective, schemaless NoSQL** solution with **high-speed key-based queries**. It supports **REST/OData** and **SDKs (e.g., C#)**, enabling seamless integration and scalability (via Cosmos DB Table API).  

---

### **Lab Exercise: Hands-on Azure Table Storage**  
**Objective**: Provision a table, insert entities, and query data via CLI and REST.  

#### **Tasks**:  
1. **Provision Resources**:  
   - Create a storage account and table using Azure CLI.  
   - Insert 3 sample entities with mixed data types (e.g., `int` and `string` for `StockCount`).  

2. **Query Data**:  
   - Generate a SAS token.  
   - Use `curl` or a browser to query:  
     - All entities.  
     - Filtered entities (e.g., `IsActive=true`).  
     - A single entity by `PartitionKey` and `RowKey`.  

3. **C# SDK**:  
   - Modify the provided code
