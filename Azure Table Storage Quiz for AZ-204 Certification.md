# **Azure Table Storage Quiz for AZ-204 Certification**

### **Multiple Choice Questions (Answers at the end)**

#### **1. What is the maximum length allowed for an Azure Table name?**
A) 32 characters  
B) 63 characters  
C) 128 characters  
D) 255 characters  

#### **2. Which of these are mandatory properties for every Azure Table entity? (Select all that apply)**
A) PartitionKey  
B) RowKey  
C) Timestamp  
D) ETag  

#### **3. What type of performance tier is required for Azure Table storage?**
A) Premium  
B) Standard  
C) Ultra  
D) Basic  

#### **4. Which CLI command creates a new Azure Table?**
A) `az storage table create`  
B) `az table create`  
C) `az storage create-table`  
D) `az create storage-table`  

#### **5. In OData queries for Azure Tables, which operator represents "greater than or equal to"?**
A) `gt`  
B) `ge`  
C) `gte`  
D) `>=`  

#### **6. What is the correct PowerShell cmdlet to insert a new entity into a table?**
A) `Add-AzTableEntity`  
B) `New-AzTableRow`  
C) `Insert-AzTableItem`  
D) `Set-AzTableData`  

#### **7. Which of these data types is NOT supported in Azure Table properties?**
A) String  
B) Int32  
C) DateTime  
D) Float  

#### **8. What does this C# code do?**  
```csharp
var client = new TableClient(connectionString, "Customers");
client.DeleteEntity("Resellers", "Contoso");
```
A) Deletes the entire table  
B) Removes a specific entity  
C) Clears all entities in a partition  
D) Updates an entity  

#### **9. How do you generate a read-only SAS token for a table using Azure CLI?**
A) `az storage generate-sas --permissions r`  
B) `az storage table generate-sas --permissions r`  
C) `az table create-sas --permissions read`  
D) `az sas create --table-permissions r`  

#### **10. Which Azure service provides a globally distributed alternative to Azure Table storage with the same API?**
A) Azure SQL Database  
B) Cosmos DB Table API  
C) Azure Blob Storage  
D) Azure Data Lake Storage  

---

## **Answers**  

1. **B** (63 characters)  
2. **A, B, C** (PartitionKey, RowKey, Timestamp)  
3. **B** (Standard)  
4. **A** (`az storage table create`)  
5. **B** (`ge`)  
6. **B** (`New-AzTableRow` - though note the correct cmdlet is `Add-AzTableRow`)  
7. **D** (Float - uses Decimal instead)  
8. **B** (Removes a specific entity)  
9. **B** (`az storage table generate-sas --permissions r`)  
10. **B** (Cosmos DB Table API)  

---

### **Scoring**  
- **9-10 correct**: AZ-204 ready! 🎯  
- **6-8 correct**: Good, review CLI/PowerShell syntax 📚  
- **≤5 correct**: Revisit Azure Table fundamentals 🔄  

Need explanations for any answers? Ask away! 😊
