# BLOQUE 1.1 – Parámetros del sistema que influyen en el rendimiento
## 1. Objetivo
Identificar los principales parámetros técnicos que afectan al rendimiento del
entorno compuesto por:
- Odoo 18 en contenedor Docker
- PostgreSQL transaccional en contenedor Docker
- PostgreSQL para almacén de datos / staging
- Apache Hop ejecutándose en el host Windows
---
## 2. Análisis del sistema anfitrión (Windows)
### 2.1 CPU
**Comando utilizado:**
```powershell
Get-CimInstance Win32_Processor | Select-Object
Name,NumberOfCores,NumberOfLogicalProcessors,MaxClockSpeed
Resultado resumido:
● Procesador: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
● Núcleos: 4
● Procesadores lógicos: 8
● Velocidad máxima: 2419
Interpretación técnica:
La CPU disponible influye en el rendimiento de Docker Desktop, Odoo,
PostgreSQL y Apache Hop. Una alta carga puede ralentizar la ejecución del ERP
y de los procesos ETL.
