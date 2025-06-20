# **Lab: Azure Table Storage with Python – Step-by-Step Guide**

## **Objetivo**
Aprender a:
✅ **Crear una cuenta de Azure Storage y una tabla**
✅ **Realizar operaciones CRUD con el SDK de Python**
✅ **Consultar datos con filtros OData**

---

## **Prerrequisitos**
- **Cuenta de Azure** (Nivel gratuito funciona)
- **Python 3.8+**
- **Azure CLI** (o PowerShell)
- **Visual Studio Code** (o cualquier editor)

---

## **Paso 1: Configurar el entorno**

### **1.1 Instalar paquetes necesarios**
```bash
pip install azure-data-tables azure-identity
```

### **1.2 Configurar variables de entorno**
Crea un archivo `.env`:
```ini
CONNECTION_STRING="<TU_CADENA_DE_CONEXIÓN>"
TABLE_NAME="Customers"
```
📌 Obtén la cadena de conexión con:
```bash
az storage account show-connection-string \
    --name <NOMBRE_CUENTA> \
    --resource-group <GRUPO_RECURSOS>
```

---

## **Paso 2: Conexión y creación de tabla**

### **2.1 Conectarse a Azure Table Storage**
```python
from azure.data.tables import TableServiceClient
from dotenv import load_dotenv
import os

load_dotenv()

service = TableServiceClient.from_connection_string(os.getenv("CONNECTION_STRING"))
table = service.get_table_client(os.getenv("TABLE_NAME"))
```

### **2.2 Crear tabla (si no existe)**
```python
try:
    table.create_table()
    print("Tabla creada!")
except Exception as e:
    print(f"Error: {e}")
```

---

## **Paso 3: Operaciones CRUD**

### **3.1 Insertar entidades**
```python
from datetime import datetime

entities = [
    {"PartitionKey": "Resellers", "RowKey": "Contoso", "Email": "contoso@example.com", "IsActive": True, "LastUpdated": datetime.now()},
    {"PartitionKey": "Sellers", "RowKey": "Fabrikam", "Email": "fabrikam@example.com", "IsActive": False, "LastUpdated": datetime.now()}
]

for entity in entities:
    table.create_entity(entity)
    print(f"Entidad insertada: {entity['RowKey']}")
```

### **3.2 Consultar entidades**
```python
# Obtener todas las entidades
print("\nTodas las entidades:")
entities = table.list_entities()
for entity in entities:
    print(entity)

# Filtrar por IsActive=True
print("\nClientes activos:")
active_customers = table.query_entities("IsActive eq true")
for customer in active_customers:
    print(customer)
```

### **3.3 Actualizar una entidad**
```python
entity = table.get_entity(partition_key="Resellers", row_key="Contoso")
entity["Email"] = "nuevo@contoso.com"
table.update_entity(entity)
print("\nEntidad actualizada!")
```

### **3.4 Eliminar una entidad**
```python
table.delete_entity(partition_key="Sellers", row_key="Fabrikam")
print("Entidad eliminada!")
```

---

## **Paso 4: Consultas avanzadas (OData)**

### **4.1 Usar operadores lógicos**
```python
# Clientes inactivos actualizados hoy
query = "IsActive eq false and LastUpdated ge datetime'2023-01-01T00:00:00Z'"
results = table.query_entities(query)
print("\nClientes inactivos recientes:")
for item in results:
    print(item)
```

### **4.2 Paginación**
```python
from azure.core.paging import ItemPaged

paged_results: ItemPaged = table.list_entities(results_per_page=2)
print("\nPrimera página:")
for entity in paged_results.by_page():
    for item in entity:
        print(item)
```

---

## **Paso 5: Limpieza**

### **5.1 Eliminar tabla (Python)**
```python
table.delete_table()
print("Tabla eliminada!")
```

### **5.2 Eliminar recursos (CLI)**
```bash
az group delete --name <GRUPO_RECURSOS> --yes
```

---

## **Conclusión**
✔ **Creamos y gestionamos Azure Table Storage desde Python**  
✔ **Realizamos operaciones CRUD y consultas avanzadas**  
✔ **Aplicamos filtros OData y paginación**  

🚀 **Próximos pasos**:  
- Implementar **transacciones en lote**  
- Migrar a **Cosmos DB Table API** para escalabilidad global  

¿Necesitas un **ejemplo con Pandas** o una **guía de rendimiento**? ¡Avísame! 🎯
