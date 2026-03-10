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
Get-CimInstance Win32_Processor | Select-Object Name,NumberOfCores,NumberOfLogicalProcessors,MaxClockSpeed
```
Resultado resumido:
- Procesador: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
- Núcleos: 4
- Procesadores lógicos: 8
- Velocidad máxima: 2419

Interpretación técnica:
- La CPU disponible influye en el rendimiento de Docker Desktop, Odoo,
PostgreSQL y Apache Hop. Una alta carga puede ralentizar la ejecución del ERP
y de los procesos ETL.

### 2.2 Memoria
**Comando utilizado:**
```powershell
Get-CimInstance Win32_OperatingSystem | Select-Object TotalVisibleMemorySize,FreePhysicalMemory
```

Resultado resumido:

- Memoria total: 16507808
- Memoria libre: 6350556

Interpretación técnica:
- La memoria disponible condiciona el comportamiento de Docker Desktop, los
contenedores y Apache Hop. Si la memoria libre es baja, puede producirse
degradación del rendimiento general.

### 2.3 Espacio en disco
**Comando utilizado:**
```powershell
Get-PSDrive -PSProvider FileSystem
```
Resultado resumido:
- Unidad principal: C:\  
- Espacio usado:  191.05GB
- Espacio libre:  46.78GB

Interpretación técnica:
- El espacio en disco afecta al almacenamiento de datos, logs, imágenes Docker,
volúmenes y ficheros temporales generados por PostgreSQL y Apache Hop.

### 2.4 Procesos activos
**Comandos utilizados:**
```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 10
```
Resultado resumido:
- Procesos con mayor CPU:
- Procesos con mayor uso de memoria:

Interpretación técnica:
- El análisis de procesos permite detectar si Docker Desktop, Java/Apache Hop u
otros procesos del sistema están consumiendo demasiados recursos.

## 3. Análisis de contenedores Docker
### 3.1 Contenedores activos
**Comando utilizado:**
```powershell
docker ps
```
Resultado resumido:
- Contenedor Odoo: odoo.18
- Contenedor PostgreSQL: postgres.db
- Estado: Up

### 3.2 Consumo de recursos de contenedores
**Comando utilizado:**
```powershell
docker stats --no-stream
```

Resultado resumido:
- CPU usada por Odoo: 398MiB
- RAM usada por Odoo: 0.01%
- CPU usada por PostgreSQL:
- RAM usada por PostgreSQL:

Interpretación técnica:
- El consumo de recursos de los contenedores permite valorar si existe
saturación en el ERP o en la base de datos.