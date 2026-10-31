# 📊 TPI-EDA: Trabajo Práctico Integrador

El Trabajo Práctico Integrador consiste en el desarrollo de un proyecto aplicado de análisis de datos e inteligencia artificial para la materia **Desarrollo de Sistemas de Inteligencia Artificial (DSIA)** en el **IFTS N° 24** (Docente: Prof. Valeria Feito).

A lo largo de la cursada, se aborda el ciclo integral de la ciencia de datos: desde la ingesta y exploración inicial del dataset, pasando por el análisis exploratorio y contraste de hipótesis de negocio, hasta la ingeniería de features, entrenamiento de modelos de Machine Learning y evaluación de métricas.

---

## 👥 Datos del Grupo y Repositorios

**Grupo N° 4**
* **Nicolás Galarza Vázquez:** [github.com/Nikovaz/CDIA.DSdIA](https://github.com/Nikovaz/CDIA.DSdIA) (Dataset seleccionado: *E-commerce Blanquería y Hogar*)
* **Alba Rotela:** [properati.csv]
* **Rodrigo Godoy:** [Trending YouTube Video Statistics]
* **Kevin Gutierrez:** [megaGymDataset.csv]
* **Jessica Lopez:** [netflix_titles]

---

## 📁 Instancias de Entrega

| Entrega | Notebook | Estado | Descripción Principal |
| :--- | :--- | :---: | :--- |
| **Entrega 1** | [`TPI_1entrega.ipynb`](./TPI_1entrega.ipynb) | ✅ Aprobada | Auditoría inicial de 5 datasets de más de 2.500 filas y 9 columnas. Justificación del dataset elegido. |
| **Entrega 2** | [`TPI_2entrega.ipynb`](./TPI_2entrega.ipynb) | ✅ Aprobada | EDA univariado, bivariado y multivariado. Planteo y contraste gráfico de 4 hipótesis de negocio. |
| **Entrega 3** | [`TPI_3entrega.ipynb`](./TPI_3entrega.ipynb) | 🚀 Finalizada | Preprocesamiento con `ColumnTransformer`, entrenamiento de 3 modelos de regresión y comparativa de métricas. |
| **Entrega Final** | [`TPI_Final.ipynb`](./TPI_Final.ipynb) | 🎯 Consolidada | Documento completo integrado con trazabilidad de principio a fin, conclusiones y recomendaciones de negocio. |

---

## 📈 Resumen Ejecutivo de la Entrega 3 (Modelado Supervisado)

### 1. Dataset Utilizado
* **Archivo:** [`dataset_ecommerce_hogar_2600.csv`](./dataset_ecommerce_hogar_2600.csv)
* **Dimensiones:** 2.680 filas × 10 columnas
* **Variable Target ($y$):** `Ventas_Mensuales` (unidades vendidas por mes)
* **Calidad de Datos:** 0 valores nulos, 0 filas duplicadas.

### 2. Pipeline de Preprocesamiento (`sklearn.compose.ColumnTransformer`)
* **Variables Continuas:** `StandardScaler()` aplicado sobre `Precio_Publico_ARS`, `Unidades_Bulto` y `Cantidad_Piezas`.
* **Variables Categóricas:** `OneHotEncoder(handle_unknown='ignore')` sobre `Rubro`, `Medida`, `Diseño`, `Color` y `Estado_Stock`.
* **División de Datos:** 80% Train ($N=2.144$) y 20% Test ($N=536$) con `random_state=42` para garantizar reproducibilidad.

### 3. Modelos Entrenados y Comparativa de Métricas

Se evaluaron tres arquitecturas complementarias de regresión:
1. **Regresión Lineal:** Modelo base paramétrico.
2. **Árbol de Decisión (`DecisionTreeRegressor`):** Modelo no lineal con regularización de profundidad (`max_depth=5`) para prevenir sobreajuste.
3. **Random Forest (`RandomForestRegressor`):** Ensamble por bagging de 100 estimadores (`max_depth=6`, `random_state=42`).

#### Tabla de Rendimiento sobre el Set de Evaluación (Test):

| Modelo | MAE (Unidades) | RMSE (Unidades) | R² Score |
| :--- | :---: | :---: | :---: |
| **Regresión Lineal** | 85.76 | 100.09 | -0.0220 |
| **Árbol de Decisión (max_depth=5)** | 85.81 | 101.48 | -0.0507 |
| **Random Forest Regressor (max_depth=6)** | **85.12** | **99.37** | **-0.0075** |

### 4. Modelo Seleccionado y Conclusión Teórica
* **Selección:** Se selecciona **Random Forest Regressor** por presentar el **menor error absoluto medio (MAE = 85.12)** y la **menor penalización cuadrática (RMSE = 99.37)**.
* **Diagnóstico de R² ≈ 0:** La dispersión cuasi-uniforme de las ventas en el catálogo refleja la naturaleza sintética del generador de datos, donde las variables explicativas son ortogonales a la rotación mensual. Los modelos de ensamble capturan la tendencia central de la demanda (~180 unidades) minimizando el riesgo de varianza extrema.
* **Propuesta de Mejora:** Reformulación del problema hacia **Clasificación Multiclase** (Rotación Alta / Media / Baja) o **Clustering no supervisado** para segmentación de catálogo por ticket promedio y volumen.
