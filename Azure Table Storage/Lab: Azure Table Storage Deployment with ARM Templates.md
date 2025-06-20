# **Lab: Azure Table Storage Deployment with ARM Templates**

## **Objective**
Learn to deploy Azure Table Storage using ARM templates:
✅ **Create storage accounts and tables via Infrastructure-as-Code**
✅ **Understand ARM template structure for storage resources**
✅ **Automate deployment with parameters**

---

## **Prerequisites**
- **Azure subscription**
- **Azure CLI or PowerShell installed**
- **VS Code (with ARM Tools extension recommended)**

---

## **Step 1: ARM Template Basics**

### **1.1 Template Structure**
Create `tableStorage.json`:
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the storage account"
      }
    },
    "tableName": {
      "type": "string",
      "defaultValue": "Customers",
      "metadata": {
        "description": "Name of the table to create"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-01-01",
      "name": "[parameters('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {}
    },
    {
      "type": "Microsoft.Storage/storageAccounts/tableServices/tables",
      "apiVersion": "2023-01-01",
      "name": "[format('{0}/default/{1}', parameters('storageAccountName'), parameters('tableName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]"
      ]
    }
  ]
}
```

---

## **Step 2: Deploy with Azure CLI**

### **2.1 Create Resource Group**
```bash
az group create --name ARMTemplateLab --location eastus
```

### **2.2 Deploy Template**
```bash
az deployment group create \
  --resource-group ARMTemplateLab \
  --template-file tableStorage.json \
  --parameters storageAccountName=armtable$(date +%s)
```

### **2.3 Verify Deployment**
```bash
az storage table list \
  --account-name <STORAGE_ACCOUNT_NAME> \
  --query "[].name" -o tsv
```

---

## **Step 3: Advanced Template (Optional)**

### **3.1 Add SAS Token Generation**
Extend your template with an outputs section:
```json
"outputs": {
  "tableSAS": {
    "type": "string",
    "value": "[listServiceSas(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2023-01-01', {'canonicalizedResource': concat('/blob/', parameters('storageAccountName')}, {'signedResource': 't', 'signedPermission': 'r', 'signedExpiry': '2024-12-31'}).serviceSasToken]"
  }
}
```

---

## **Step 4: Cleanup**
```bash
az group delete --name ARMTemplateLab --yes
```

---

## **Key Concepts Learned**
✔ **ARM template structure** for storage resources  
✔ **Dependency management** with `dependsOn`  
✔ **Deployment automation** via CLI/PowerShell  

🚀 **Next Steps**:  
- Add **RBAC assignments** in the template  
- Implement **linked templates** for complex deployments  

Need the **PowerShell version** or **Bicep equivalent**? Ask below! 👇
