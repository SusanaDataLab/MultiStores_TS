# MultiStores_TS
# 📈 Pronóstico de Demanda Regional y Optimización de Replenishment (Año 2022)

Este repositorio contiene la solución de **Machine Learning y Series Temporales** desarrollada para predecir la demanda mensual por región durante el año 2022. La arquitectura implementa un modelo **Random Forest Suavizado con `log1p`**, optimizando la planificación de inventario y reabastecimiento (*replenishment*).

El modelo final redujo el Error Medio Absoluto (**MAE**) en un **53.8%** respecto al modelo base (pasando de 1,351 a **623 unidades**), alcanzando un coeficiente de determinación ($R^2$) de **0.9963**.

---

## 📊 Diagnóstico Visual y Análisis por Región (Año 2022)

El análisis comparativo de las curvas de **Demanda Real vs. Predicción** permite evaluar el comportamiento operativo por zona geográfica:

### **1. Región con Mayor Volumen de Ventas**
* **East China:** Se consolidó como la región con la mayor cantidad de unidades vendidas en todo el periodo, mostrando un ajuste óptimo en las proyecciones.

### **2. Regiones con Solapamiento Perfecto**
* En la gran mayoría de las regiones analizadas, **las líneas de demanda real y predicción se solapan casi por completo**, demostrando una captura precisa de la tendencia y estacionalidad por parte del modelo.

### **3. Casos Específicos de Ajuste**
* **North China:** La demanda real observada se mantuvo constante en torno a las **150,000 unidades**. La predicción del modelo inició en un nivel superior a las **180,000 unidades** y experimentó un ajuste progresivo a la baja hasta situarse en cerca de las **15,000 unidades**.
* **Southwest China:** La demanda real se mantuvo estable cerca de las **150,000 unidades**. Por su parte, la predicción del modelo mostró una trayectoria que descendió desde un poco más de las **104,000 unidades** hasta estabilizarse en torno a las **100,000 unidades**.

---

## 🔍 Interpretación Técnica del $R^2$ = 0.9963

Un coeficiente de determinación cercano a **1.0** suele ser motivo de revisión por posible sobreajuste (*overfitting*). En este contexto, el resultado responde a la naturaleza de las series temporales analizadas:

1. **Estructura Rígida de Metas:** En las regiones donde la demanda no sufrió fluctuaciones abruptas, las metas proyectadas mantuvieron una inercia lineal constante año tras año.
2. **Impacto del Feature Engineering:** Las variables de rezago (`lag_1`) y los promedios móviles (`rolling_mean_3m`) explicaron más del **84% de la varianza**, permitiendo al algoritmo replicar la trayectoria observada con mínima desviación.

---

## 🛠️ Pipeline Metodológico

1. **Feature Engineering:**
   * `lag_1`: Demanda del mes inmediatamente anterior.
   * `rolling_mean_3m`: Promedio móvil trimestral para capturar la tendencia a corto plazo.
   * Variables cíclicas estacionales (`sin_month`, `cos_month`).
2. **Estabilización de Varianza (`log1p`):**
   * Transformación logarítmica de la variable objetivo ($y_{trans} = \log(1 + y)$) para reducir el impacto de picos atípicos.
   * Reversión de la escala mediante $\exp(x) - 1$.
3. **Validación Temporal:**
   * **Entrenamiento:** Registro histórico de 2019 a 2021.
   * **Evaluación:** Evaluación sobre el periodo completo de 2022.

---

## 📈 Tabla Comparativa de Desempeño

| Enfoque | MAE (Unidades) | RMSE (Unidades) | $R^2$ Score | Impacto Relativo |
| :--- | :---: | :---: | :---: | :--- |
| **Random Forest Estándar** | 1,351.19 | 4,040.35 | 0.9956 | Línea base en unidades reales |
| **Random Forest + Log1p** | **623.15** | **3,708.44** | **0.9963** | **Reducción del 53.8% en MAE** |

---

## 📂 Estructura del Repositorio

```text
├── data/
│   ├── predicciones_demanda_2022_final.csv       # Dataset con demanda real y predicciones 2022
│   └── metricas_desempeno_por_region_2022.csv   # Resumen de MAE, RMSE y R2 por región
├── notebooks/
│   └── pronostico_demanda_2022.ipynb             # Código en Python (EDA, Modelado y Gráficos)
├── README.md                                     # Documentación principal del proyecto
└── requirements.txt                              # Librerías necesarias para la ejecución
