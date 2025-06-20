# **Lab: Azure Table Storage with PowerShell - Step-by-Step Guide**

## **Objective**
Learn to manage Azure Table Storage using PowerShell:
✅ **Create storage accounts and tables**
✅ **Perform CRUD operations on entities**
✅ **Generate SAS tokens and query data**

---

## **Prerequisites**
- **Azure PowerShell module** (`Install-Module Az`)
- **Active Azure subscription**
- **PowerShell 5.1+ or PowerShell 7+**

---

## **Step 1: Initial Setup**

### **1.1 Login to Azure**
```powershell
Connect-AzAccount
```

### **1.2 Create resource group**
```powershell
New-AzResourceGroup -Name PSStorageLab -Location eastus
```

---

## **Step 2: Create Storage Account**

### **2.1 Create storage account**
```powershell
$accountName = "psstorage" + (Get-Date -Format "yyyyMMddHHmmss")
New-AzStorageAccount `
    -ResourceGroupName PSStorageLab `
    -Name $accountName `
    -Location eastus `
    -SkuName Standard_LRS `
    -Kind StorageV2
```

### **2.2 Get storage context**
```powershell
$storageContext = (Get-AzStorageAccount `
    -ResourceGroupName PSStorageLab `
    -Name $accountName).Context
```

---

## **Step 3: Table Operations**

### **3.1 Create a table**
```powershell
$table = New-AzStorageTable `
    -Name "Customers" `
    -Context $storageContext
```

### **3.2 Insert entities**
```powershell
$entity = @{
    PartitionKey = "Resellers"
    RowKey = "Contoso"
    Email = "contoso@example.com"
    IsActive = $true
}

Add-AzTableRow `
    -Table $table.CloudTable `
    -Entity $entity
```

### **3.3 Query entities**
```powershell
Get-AzTableRow `
    -Table $table.CloudTable
```

### **3.4 Filter entities**
```powershell
Get-AzTableRow `
    -Table $table.CloudTable `
    -CustomFilter "IsActive eq true"
```

---

## **Step 4: Shared Access Signatures (SAS)**

### **4.1 Generate SAS token**
```powershell
$startTime = Get-Date
$endTime = $startTime.AddYears(1)

$sasToken = New-AzStorageTableSASToken `
    -Name "Customers" `
    -Context $storageContext `
    -Permission "r" `
    -StartTime $startTime `
    -ExpiryTime $endTime
```

### **4.2 Query with SAS**
```powershell
$sasContext = New-AzStorageContext `
    -StorageAccountName $accountName `
    -SasToken $sasToken

Get-AzTableRow `
    -Table $table.CloudTable `
    -Context $sasContext
```

---

## **Step 5: Cleanup**

### **5.1 Delete table**
```powershell
Remove-AzStorageTable `
    -Name "Customers" `
    -Context $storageContext `
    -Force
```

### **5.2 Delete resource group**
```powershell
Remove-AzResourceGroup -Name PSStorageLab -Force
```

---

## **Conclusion**
✔ **Managed Table Storage entirely from PowerShell**  
✔ **Performed CRUD operations and filtered queries**  
✔ **Generated SAS tokens for secure access**  

🚀 **Next steps**:  
- Automate with PowerShell scripts  
- Integrate with Azure Functions for automated processing  

Need **advanced query examples** or **error handling techniques**? Let me know! 🔥
