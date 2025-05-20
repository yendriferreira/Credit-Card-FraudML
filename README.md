# Credit-Card-FraudML

Análisis Exploratorio de Datos para Detección de Fraude en Tarjetas de Crédito

Este repositorio presenta un análisis detallado del conjunto de datos de fraude en tarjetas de crédito, enfocándose en la comprensión profunda de las características y en cómo el desbalance afecta su evaluación.

---

## 1. Descripción del Contexto del Problema

Las compañías de tarjetas de crédito enfrentan el desafío crítico de detectar transacciones fraudulentas en tiempo real para proteger a sus clientes. Este problema tiene particularidades que lo hacen ideal para soluciones basadas en Machine Learning:

- **Volumen masivo de datos:** millones de transacciones diarias que deben ser evaluadas en tiempo real.  
- **Patrones complejos:** los fraudes evolucionan constantemente y presentan patrones sutiles difíciles de detectar con reglas estáticas.  
- **Costos asimétricos:** el costo de no detectar un fraude es mucho mayor que el de investigar una falsa alarma.  
- **Necesidad de adaptación:** los patrones de fraude cambian con el tiempo, requiriendo sistemas que puedan aprender y adaptarse.

La aplicación de Machine Learning ofrece la capacidad de detectar patrones complejos y sutiles, adaptarse a nuevos tipos de fraude y optimizar el balance entre detectar fraudes reales y minimizar falsos positivos.

---

## 2. Composición de la Base de Datos

El dataset contiene transacciones realizadas con tarjetas de crédito en septiembre de 2013 por titulares europeos:

- **Número de muestras:** 284 807 transacciones  
- **Número de variables:** 31 (incluyendo la variable objetivo)  
- **Clases:** 492 fraudes (0.172 %) vs. 284 315 transacciones normales (99.828 %)  
- **Ratio de desbalance:** 1:578 (por cada transacción fraudulenta hay 578 normales)  

### Variables

- **Time:** segundos transcurridos entre cada transacción y la primera (numérica continua)  
- **Amount:** monto de la transacción (numérica continua)  
- **V1–V28:** componentes principales obtenidas mediante PCA (numéricas continuas, normalizadas)  
- **Class:** variable objetivo binaria (1 = fraude, 0 = normal)  

**Aspectos destacables:**

- No hay valores faltantes.  
- Las características V1–V28 están transformadas mediante PCA por razones de confidencialidad.  
- Todas las variables son numéricas continuas.  
- El dataset presenta un desbalance extremo (0.172 % de fraudes).

---

## 3. Impacto del Desbalance en la Evaluación de Características

El desbalance extremo del dataset afecta significativamente la evaluación de características por las siguientes razones:

### Efectos del desbalance

- **Sesgos en métricas tradicionales:**  
  - Las correlaciones de Pearson quedan dominadas por la clase mayoritaria.  
  - Las estadísticas globales enmascaran los patrones de la clase minoritaria.  
  - Los métodos de selección de características estándar tienden a ignorar variables importantes para detectar fraudes.

### Evidencia de impacto

- Las correlaciones en el dataset original vs. un conjunto balanceado muestran diferencias sustanciales.  
- Características que parecen poco importantes en el dataset completo resultan altamente discriminativas cuando se analizan específicamente para fraudes.  
- Los rangos de importancia de características cambian significativamente cuando se evalúan con métodos robustos al desbalance.

### Hallazgos clave

- V17, V14, V12, V10 y V16 son las más discriminativas según correlaciones en datos balanceados.  
- La información mutua identifica V14, V4, V12, V10 y V17 como las más relevantes.  
- El test de Kolmogorov-Smirnov destaca V14, V12, V10, V16 y V3 por sus diferencias distribucionales.

### Estrategias implementadas

- **Métodos robustos al desbalance:**  
  - Test de Kolmogorov-Smirnov para detectar diferencias distribucionales entre clases.  
  - Información Mutua como medida de dependencia estadística menos sensible al desbalance.  
  - Creación de un subconjunto balanceado para análisis comparativo.

- **Visualizaciones adaptadas:**  
  - Gráficos separados por clase.  
  - Uso de escalas logarítmicas para visualizar mejor la clase minoritaria.  
  - Análisis de densidad y distribuciones acumulativas.

- **Análisis estratificado:**  
  - Evaluación de características por rangos de monto y tiempo.  
  - Identificación de segmentos con mayor tasa de fraude.  
  - Análisis de umbrales óptimos para cada característica clave.

---

## 4. Hallazgos Clave del Análisis

- **Patrones temporales:** la tasa de fraude varía significativamente según la hora del día; existen franjas horarias con mayor incidencia de fraudes.  
- **Patrones en montos:** las transacciones fraudulentas tienen una distribución de montos diferente a las normales; ciertos rangos de monto presentan tasas de fraude significativamente mayores.  
- **Características discriminativas:** varias componentes PCA muestran distribuciones claramente diferentes entre clases; las más importantes difieren según el método de evaluación.  
- **Detección de anomalías:** los métodos de detección de anomalías (Isolation Forest) pueden identificar una proporción importante de fraudes, mostrando solapamiento con las transacciones fraudulentas.

---

## 5. Paradigma de Aprendizaje Recomendado

Basado en el análisis exploratorio, se recomienda un paradigma **supervisado** con ajustes específicos para datos desbalanceados:

### Justificación del enfoque supervisado

- **Datos etiquetados:** existe retroalimentación clara sobre la corrección de predicciones.  
- **Objetivo binario:** clasificar nuevas transacciones en “fraude” o “normal”.  

### Ajustes para el desbalance extremo

- **Técnicas de muestreo:** SMOTE, SMOTETomek, ADASYN.  
- **Ponderación de clases** en el entrenamiento.  
- **Umbrales de decisión optimizados** según la métrica deseada.  

### Algoritmos y métricas recomendadas

- **Modelos robustos:** XGBoost, LightGBM con ajuste de pesos.  
- **Ensamblados balanceados:** EasyEnsemble, BalancedRandomForest.  
- **Evaluación:** Area Under the Precision–Recall Curve (AUPRC) y matrices de confusión ponderadas.

---

Con esta estructura, tu README conservará todo el contenido original y se verá claro y profesional en GitHub.

