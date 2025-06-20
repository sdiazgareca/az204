# **Lab: Azure Table Storage with Azure CLI - Step-by-Step Guide**

## **Objective**
Learn to manage Azure Table Storage using only Azure CLI:
✅ **Create storage accounts and tables**
✅ **Perform CRUD operations on entities**
✅ **Generate SAS tokens and query data**

---

## **Prerequisites**
- **Azure CLI installed** (verify with `az --version`)
- **Active Azure subscription**
- **Terminal (Bash, PowerShell or CMD)**

---

## **Step 1: Initial Setup**

### **1.1 Login to Azure**
```bash
az login
```

### **1.2 Create resource group**
```bash
az group create --name CLIStorageLab --location eastus
```

---

## **Step 2: Create Storage Account**

### **2.1 Create storage account**
```bash
az storage account create \
    --name clistorage$(date +%s) \
    --resource-group CLIStorageLab \
    --location eastus \
    --sku Standard_LRS \
    --kind StorageV2
```

### **2.2 Get access keys**
```bash
az storage account keys list \
    --account-name <ACCOUNT_NAME> \
    --resource-group CLIStorageLab \
    --query "[0].value" -o tsv
```

---

## **Step 3: Table Operations**

### **3.1 Create a table**
```bash
az storage table create \
    --name Customers \
    --account-name <ACCOUNT_NAME> \
    --account-key <PRIMARY_KEY>
```

### **3.2 Insert entities**
```bash
az storage entity insert \
    --table-name Customers \
    --account-name <ACCOUNT_NAME> \
    --account-key <PRIMARY_KEY> \
    --entity \
        PartitionKey=Resellers \
        RowKey=Contoso \
        Email=contoso@example.com \
        IsActive=true
```

### **3.3 Query entities**
```bash
az storage entity query \
    --table-name Customers \
    --account-name <ACCOUNT_NAME> \
    --account-key <PRIMARY_KEY>
```

### **3.4 Filter entities**
```bash
az storage entity query \
    --table-name Customers \
    --account-name <ACCOUNT_NAME> \
    --account-key <PRIMARY_KEY> \
    --filter "IsActive eq true"
```

---

## **Step 4: Shared Access Signatures (SAS)**

### **4.1 Generate SAS token**
```bash
az storage table generate-sas \
    --name Customers \
    --account-name <ACCOUNT_NAME> \
    --permissions r \
    --expiry 2024-12-31
```

### **4.2 Query with SAS**
```bash
az storage entity show \
    --table-name Customers \
    --account-name <ACCOUNT_NAME> \
    --sas-token "<SAS_TOKEN>" \
    --partition-key Resellers \
    --row-key Contoso
```

---

## **Step 5: Cleanup**

### **5.1 Delete table**
```bash
az storage table delete \
    --name Customers \
    --account-name <ACCOUNT_NAME> \
    --account-key <PRIMARY_KEY>
```

### **5.2 Delete resource group**
```bash
az group delete --name CLIStorageLab --yes
```

---

## **Conclusion**
✔ **Managed Table Storage entirely from CLI**  
✔ **Learned basic CRUD and filtered queries**  
✔ **Generated SAS tokens for secure access**  

🚀 **Next steps**:  
- Automate with Bash/PowerShell scripts  
- Integrate with Azure Functions for automated processing  

Need **advanced OData expressions** or **automation examples**? Let me know! 🔥
