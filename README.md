# 🌎 Proyecto Integrador – Actividad 3  
### Big Data – IUDigital

**Autores:**  
- Indira Hamdam  
- Cristian Vicioso  

**Entorno:** Databricks Free Edition  
**Lenguaje:** PySpark / SQL / Python  

---

## 🎯 Objetivo General
Transformar, limpiar y visualizar datos de calidad del aire en Databricks, aplicando técnicas de ingeniería de datos, análisis temporal y visualización con PySpark y SQL.  
El propósito final es consolidar un flujo completo de procesamiento de datos —desde la ingestión hasta la interpretación visual— utilizando el dataset **AirQualityUCI**.

---

## 🧩 Descripción del Proyecto
Este proyecto forma parte del **Proyecto Integrador** de la asignatura *Big Data*, y corresponde a la **Evidencia de Aprendizaje (EA3)**.

A partir del dataset trabajado en la Actividad 2, se realizaron las siguientes tareas:
1. **Creación de columnas derivadas de fecha** (año, mes, día, día de la semana y nombre del día).  
2. **Generación de un resumen mensual** con métricas descriptivas.  
3. **Limpieza del dataset** (trimming, conversión de tipos, imputación de nulos, eliminación de outliers).  
4. **Visualizaciones estadísticas y comparativas** mediante *seaborn*, *matplotlib* y consultas SQL equivalentes.  
5. **Documentación académica y justificación** de cada transformación realizada.

---

## 🧠 Contexto del Dataset

- **Nombre:** AirQualityUCI  
- **Fuente:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Air+Quality)  
- **Descripción:**  
  Conjunto de datos con concentraciones horarias de contaminantes atmosféricos (CO, NO₂, NMHC, etc.) y variables meteorológicas (temperatura, humedad, presión) medidas por sensores de calidad del aire en Italia (2004–2005).  
- **Campos principales:**
  - `Date` / `Time` — Fecha y hora de medición  
  - `CO(GT)` — Concentración de monóxido de carbono  
  - `NOx(GT)`, `NO2(GT)` — Óxidos de nitrógeno  
  - `T`, `RH`, `AH` — Temperatura, humedad relativa y concentración de humedad  

---

## ⚙️ Entorno de Ejecución

| Componente | Valor |
|-------------|-------|
| **Plataforma** | Databricks Free Edition |
| **Runtime** | Databricks Runtime 13.x (Spark 3.4.x, Scala 2.12) |
| **Lenguaje base** | PySpark (Python 3.10) |
| **Cluster** | 1 Worker – 1 Driver (2 cores, 4 GB RAM aprox.) |
| **Almacenamiento** | `/Workspace/Users/indira.hamdam@est.iudigital.edu.co/` |

📄 *Evidencia técnica disponible en el notebook (celdas iniciales con configuración y `spark.conf.getAll()` resumido).*

---

## 🧱 Diseño del Esquema de Datos

**Entidad:** `air_quality`  
**Campos principales:**

| Campo | Tipo | Descripción | Nulos |
|--------|------|--------------|-------|
| `fecha` | date | Fecha combinada de `Date` y `Time` | No |
| `anio` | int | Año derivado | No |
| `mes` | int | Mes derivado | No |
| `dia` | int | Día del mes | No |
| `nombre_dia` | string | Día de la semana (lunes a domingo) | No |
| `CO_GT` | double | Monóxido de carbono (ppm) | Sí |
| `T` | double | Temperatura (°C) | Sí |
| `RH` | double | Humedad relativa (%) | Sí |

> ✨ Estas transformaciones permiten analizar patrones de **sazonalidad**, variaciones **intradía** y tendencias **ambientales**.

---

## 🔄 Transformaciones Implementadas

1. **Creación de columna `fecha`** combinando `Date` y `Time`.  
2. **Derivación de columnas temporales:** `anio`, `mes`, `dia`, `dia_semana`, `nombre_dia`.  
3. **Limpieza y estandarización:**  
   - Trimming y conversión a minúsculas.  
   - Conversión robusta de tipos (`T`, `CO_GT`, `RH`) reemplazando comas y nulos.  
   - Imputación de medias y eliminación de outliers (IQR).  
4. **Persistencia:** creación de la tabla `air_quality_clean` y `resumen_mensual`.  
5. **Validación:** verificación de registros y tipos mediante `printSchema()` y `describe()`.

---

## 📊 Visualizaciones

### 1️⃣ Promedio de CO(GT) por día de la semana

Visualización en **Seaborn** (PySpark → Pandas → gráfico de barras):

```python
plt.figure(figsize=(8,4))
sns.barplot(data=pdf, x="nombre_dia", y="CO_GT", estimator="mean",
            order=["lunes","martes","miércoles","jueves","viernes","sábado","domingo"])
plt.title("Promedio de CO(GT) por día de la semana")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
