# PROYECTO: Sistema de Control de Inventario con Arquitectura Medallion

## Descripción
Sistema de análisis de inventario implementado en Databricks que procesa 22,071 registros desde Excel y genera dashboards interactivos para la gestión de inventario empresarial.

## Arquitectura Medallion

### Bronze Layer
- **Tabla**: workshop_aleja.default.inventario_bronze
- **Registros**: 22,071
- **Columnas**: 34
- **Función**: Carga raw del archivo Excel sin transformaciones

### Silver Layer
- **Tabla**: workshop_aleja.default.inventario_silver
- **Registros**: 22,071
- **Columnas**: 38 (incluye columnas calculadas)
- **Transformaciones**:
  - Limpieza de datos
  - Normalización de valores
  - Cálculos de provisiones
  - Exclusión de registros con errores

### Gold Layer - Tablas Analíticas

#### 1. inventario_gold_unidad_negocio
Métricas agregadas por unidad de negocio
- **5 Unidades**: TL ($73.3B), DT ($72.1B), PR ($52.2B), TR ($13.1B), NB ($807M)
- **Columnas**: unidad_de_negocio, total_productos, valor_total_inventario, provision_total, valor_promedio_producto

#### 2. inventario_gold_categoria_provision
Distribución por categoría de provisión
- **7 Categorías**: Regular (58.45%), B (23.11%), D (7.82%), C (6.95%), A (3.18%), Muestra (0.49%)
- **Valor Total**: $211.37B COP

#### 3. inventario_gold_temporal
Series temporales para análisis de tendencias
- **Periodo**: Marzo-Abril 2026
- **Granularidad**: Por fecha y unidad de negocio

#### 4. inventario_gold_producto
Análisis detallado a nivel de producto/item

## Dashboard Databricks

### Widgets Implementados
1. **Valor Total Inventario** (Counter): $211.37B COP
2. **Total Productos** (Counter): 22,071 items
3. **Distribución por Unidad de Negocio** (Bar Chart)
4. **Categorías de Provisión** (Pie Chart)
5. **Tabla Detalle** (Table): Métricas por unidad
6. **Tendencia Temporal** (Line Chart): Evolución por fecha

## Datos Clave del Inventario

### Por Unidad de Negocio
- **TL**: 546 productos, $73.3B
- **DT**: 237 productos, $72.1B
- **PR**: 72 productos, $52.2B
- **TR**: 319 productos, $13.1B
- **NB**: 22 productos, $807M

### Por Categoría de Provisión
- **Regular**: $123.5B (58.45%)
- **B**: $48.8B (23.11%)
- **D**: $16.5B (7.82%)
- **C**: $14.7B (6.95%)
- **A**: $6.7B (3.18%)
- **Muestra**: $1.0B (0.49%)

## Requisitos Técnicos

### Databricks
- Workspace con Unity Catalog habilitado
- SQL Warehouse activo
- Permisos en catalog: workshop_aleja
- Serverless compute para notebooks

### Archivos de Origen
- **Ubicación**: /Volumes/workshop_aleja/default/trabajo_final/Inventario_databricks.xlsx
- **Formato**: Excel (.xlsx)
- **Registros**: 22,071

## Estructura del Proyecto

```
control-inventario/
├── README.md (este archivo)
├── notebooks/
│   └── Pipeline Medallion - Control Inventario.ipynb
├── data/
│   └── Inventario_databricks.xlsx
└── dashboards/
    └── Control de Inventario - Medallion.lvdash.json
```

## Instalación y Uso

### 1. Configuración del Catalog
```sql
USE CATALOG workshop_aleja;
USE SCHEMA default;
```

### 2. Ejecución del Pipeline
- Abrir notebook: Pipeline Medallion - Control Inventario
- Ejecutar todas las celdas secuencialmente
- Verificar creación de 6 tablas (1 bronze + 1 silver + 4 gold)

### 3. Dashboard
- Acceder al dashboard desde Databricks UI
- Los datasets se actualizan automáticamente desde las tablas gold

## Integración con Power BI (Opcional)

### Conexión Manual
1. Obtener credenciales del SQL Warehouse:
   - Server hostname
   - HTTP path
2. Crear Personal Access Token (Settings → Developer → Tokens)
3. Importar tablas gold desde Power BI Desktop
4. Construir visualizaciones personalizadas

## Mantenimiento

### Actualización de Datos
1. Reemplazar archivo Excel en el volumen
2. Re-ejecutar notebook del pipeline
3. El dashboard se refresca automáticamente

### Monitoreo
- Verificar conteo de registros en cada capa
- Validar que no existan valores ERROR:#VALUE! en silver/gold
- Revisar totales agregados contra el archivo fuente

## Autor
Alejandra (alejita2180@gmail.com)

## Fecha de Creación
Mayo 2026

## Licencia
Uso interno empresarial
