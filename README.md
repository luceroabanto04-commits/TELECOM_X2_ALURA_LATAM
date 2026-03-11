# 📡 TelecomX LATAM — Análisis de Churn + Modelo Predictivo


## 📋 Descripción del Proyecto

Este proyecto extiende el análisis de evasión de clientes de **TelecomX** incorporando un modelo de **Machine Learning** para predecir qué clientes tienen mayor probabilidad de abandonar el servicio.

El flujo completo cubre: ETL del dataset JSON anidado, análisis exploratorio con visualizaciones y construcción de un modelo de clasificación con árbol de decisión evaluado mediante validación cruzada estratificada.

---

## 🎯 Objetivo

Identificar los factores que influyen en el abandono de clientes de TelecomX, construir un modelo predictivo de churn y proponer recomendaciones estratégicas para reducirlo.

---

## 📁 Estructura del Proyecto
```
TelecomX_LATAM/
│
├── TelecomX_LATAM.ipynb     
└── README.md
```

---

## 🗃️ Dataset

- **Fuente:** [Alura Cursos — GitHub](https://raw.githubusercontent.com/alura-cursos/challenge2-data-science-LATAM/refs/heads/main/TelecomX_Data.json)
- **Formato original:** JSON con columnas anidadas (`customer`, `phone`, `internet`, `account`)
- **Registros originales:** 7.267
- **Registros tras limpieza:** 7.032
- **Variables analizadas:** 22
- **Variable objetivo:** `Abandonó_Servicio` (Churn)

### Diccionario de Datos

| Variable | Descripción |
|---|---|
| `ID` | Identificación única del cliente |
| `Abandonó_Servicio` | Si el cliente dejó la empresa (1 = Sí, 0 = No) |
| `Género` | Género del cliente |
| `Cliente_Señor` | Si el cliente tiene 65 años o más |
| `Antigüedad` | Meses de permanencia en la empresa |
| `Servicio_Internet` | Tipo de servicio de internet contratado |
| `Tipo_Contrato` | Modalidad del contrato (Mensual, Anual, Bienal) |
| `Método_Pago` | Forma de pago utilizada |
| `Cargo_Mensual` | Monto facturado mensualmente |
| `Cargo_Total` | Monto total facturado |
| `Cargo_Diario` | Variable derivada: Cargo Mensual / 30 |

---

## ⚙️ Proceso ETL

### 🔵 Extracción
- Carga del dataset en formato JSON desde repositorio GitHub
- Inspección inicial de dimensiones y tipos de datos

### 🟡 Transformación
- Desanidado de columnas JSON con `pd.json_normalize`
- Eliminación de 235 registros con `Churn` vacío y `Charges.Total` no numérico
- Conversión de variables binarias (`Yes`/`No`) → enteros (`1`/`0`)
- Normalización de valores: `No internet service` y `No phone service` → `0`
- Traducción de columnas y valores al español
- Creación de variable derivada `Cargo_Diario`
- Reset de índice para integridad del DataFrame

### 🟢 Carga y Análisis
- Estadísticas descriptivas con `.describe()`
- Visualizaciones de distribución de churn
- Análisis por variables categóricas y numéricas
- Matriz de correlación de variables numéricas

---

## 📊 Principales Hallazgos

### Tasa de Abandono General

| Categoría | Porcentaje |
|---|---|
| Clientes que permanecieron | 73.42% |
| Clientes que abandonaron | **26.58%** |

> ⚠️ 1 de cada 4 clientes abandona el servicio. Esta tasa supera el umbral crítico del 20%.

### 🔴 Segmentos con Mayor Riesgo de Churn

| Factor | Tasa de Abandono |
|---|---|
| Cheque electrónico (método de pago) | **45.3%** |
| Contrato Mensual | **42.7%** |
| Fibra Óptica | **41.9%** |
| Cliente Senior | **41.7%** |

### 📉 Antigüedad y Churn
Los clientes con **menor antigüedad (0–18 meses)** presentan la mayor tasa de abandono. A mayor tiempo en la empresa, menor probabilidad de churn.

---

## 🤖 Modelo de Machine Learning

### Preprocesamiento
- Codificación de variables categóricas con `OneHotEncoder` (drop `"first"`)
- Variables codificadas: `Servicio_Internet`, `Tipo_Contrato`, `Método_Pago`, `antiguedad_bin`
- División train/test: **80% / 20%** con `random_state=42`

### Modelos Entrenados

| Modelo | Descripción |
|---|---|
| Baseline (DummyClassifier) | Predice siempre la clase mayoritaria |
| Árbol de Decisión | `max_depth=5`, evaluado con validación cruzada estratificada (5-fold) |

### Métricas de Evaluación

| Modelo | Exactitud | Precisión | Recall | F1-Score |
|---|---|---|---|---|
| Baseline | ~73% | — | — | — |
| Árbol de Decisión (Test) | ~79% | evaluado | evaluado | evaluado |

> La validación cruzada estratificada (`StratifiedKFold`, 5 folds) garantiza que la proporción de churn se mantenga equilibrada en cada fold, evitando resultados optimistas por azar.

---

## 💡 Recomendaciones Estratégicas

1. **Incentivar contratos de largo plazo** — Los contratos anuales y bienales reducen significativamente el churn.
2. **Revisar la experiencia con Fibra Óptica** — Alta tasa de abandono sugiere problemas de calidad o precio.
3. **Migrar clientes a pagos automáticos** — El cheque electrónico concentra el mayor churn.
4. **Onboarding reforzado en los primeros 18 meses** — El período más crítico para la retención.
5. **Usar el modelo predictivo para intervención temprana** — Identificar clientes en riesgo antes de que abandonen.

---

## 🛠️ Tecnologías Utilizadas

- **Python 3**
- **Pandas** — Manipulación y limpieza de datos
- **NumPy** — Operaciones numéricas
- **Matplotlib / Seaborn** — Visualizaciones
- **Scikit-learn** — Preprocesamiento y modelos de ML
- **Google Colab** — Entorno de ejecución

---

## ▶️ Cómo Ejecutar

Abre el notebook directamente en Google Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/KonungurProFer/Proyecto_telecom_X/blob/main/TelecomX_LATAM.ipynb)

El dataset se carga automáticamente desde GitHub, no necesitas descargar nada.

---

## 👩‍💻 Autora

**Lucero Abanto**  
Challenge Data Science — Alura LATAM
