# 📈 Reto de Ingeniería de Datos: Análisis temporal de Bitcoin

## 🎯 Propósito del proyecto

Este repositorio contiene un análisis reproducible de la serie temporal diaria del precio de **Bitcoin en dólares estadounidenses (USD)**. El proyecto adapta el reto original de análisis de un portafolio de inversión a una base temporal con las columnas:

- `observation_date`: fecha de observación.
- `CBBTCUSD`: precio de Bitcoin expresado en USD.

El análisis busca responder visualmente y con evidencia numérica:

1. ¿Cómo cambió el precio de Bitcoin durante el periodo observado?
2. ¿Qué años o periodos explican principalmente el cambio global?
3. ¿Qué nivel de volatilidad y riesgo presenta la serie?
4. ¿Cómo puede construirse un análisis reproducible desde la preparación de datos hasta la interpretación?

---

## 🗂️ Contenido del repositorio

| Archivo | Descripción |
|---|---|
| [`Analisis_CBBTCUSD_Bitcoin.ipynb`](Analisis_CBBTCUSD_Bitcoin.ipynb) | Notebook completo con preparación, EDA, ingeniería de características, visualizaciones, modelamiento e interpretación. |
| `CBBTCUSD (1).csv` | Base de datos requerida para ejecutar el notebook. Debe estar en la raíz del proyecto o en una ruta indicada por el notebook. |

> El notebook busca automáticamente el CSV en varias ubicaciones. Si el archivo no está disponible, debe colocarse junto al notebook o en `/content/` cuando se utilice Google Colab.

---

## 🧭 Flujo metodológico

```text
┌─────────────────────┐
│ 1. Cargar la base   │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 2. Validar calidad  │
│ tipos, faltantes,   │
│ duplicados y fechas │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 3. Crear variables  │
│ de tiempo, retornos,│
│ medias y riesgo     │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 4. Explorar         │
│ tendencias, ciclos, │
│ extremos y cambios  │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 5. Visualizar       │
│ precio, retornos,   │
│ volatilidad y riesgo│
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 6. Modelar          │
│ baseline temporal   │
└──────────┬──────────┘
           ▼
┌─────────────────────┐
│ 7. Interpretar y    │
│ comunicar resultados│
└─────────────────────┘
```

---

## 🧪 E0 — Preparación y calidad de los datos

El notebook realiza las siguientes actividades:

- Importa las librerías necesarias.
- Localiza y carga el archivo CSV.
- Convierte `observation_date` a formato fecha.
- Convierte `CBBTCUSD` a formato numérico.
- Ordena los registros cronológicamente.
- Revisa:
  - valores faltantes;
  - fechas duplicadas;
  - precios inválidos o no positivos;
  - saltos o huecos temporales;
  - estructura y periodo cubierto.
- Conserva los valores faltantes como evidencia de calidad y evita imputarlos silenciosamente.

### 📏 Niveles de medición

| Variable | Tipo | Justificación |
|---|---|---|
| `observation_date` | Temporal / ordinal | Permite ordenar las observaciones y estudiar evolución, ciclos y periodos. |
| `CBBTCUSD` | Cuantitativa continua de razón | El precio tiene un cero significativo y permite comparar diferencias y proporciones. |
| Rendimiento diario | Cuantitativa relativa | Permite comparar cambios porcentuales entre días con distintos niveles de precio. |

---

## 🛠️ Ingeniería de características

Para transformar la serie y enriquecer el análisis se calculan:

- Cambio absoluto diario.
- Rendimiento simple diario.
- Rendimiento logarítmico.
- Año, mes y trimestre.
- Día de la semana.
- Máximo acumulado.
- `Drawdown`, o pérdida porcentual desde el máximo acumulado.
- Medias móviles de 7, 30 y 90 días.
- Volatilidad móvil de 30 días anualizada.
- Indicadores de días de subida y caída.

Estas transformaciones permiten pasar de una tabla de precios a una estructura útil para análisis exploratorio, comparación temporal y modelamiento.

---

## 📊 E1 — Análisis exploratorio

El notebook calcula y presenta:

### Indicadores principales

- Fecha inicial y final.
- Precio inicial y final.
- Cambio absoluto.
- Cambio porcentual acumulado.
- Precio mínimo y fecha correspondiente.
- Precio máximo y fecha correspondiente.
- Número de observaciones.
- Rendimiento medio diario.
- Número de días positivos y negativos.
- Mejor y peor rendimiento diario.
- Volatilidad diaria.
- Volatilidad anualizada usando 365 días.
- Máximo `drawdown`.

### Comparaciones temporales

Se generan resúmenes:

- Por año: primer precio, último precio, mínimo, máximo y variación porcentual.
- Por mes: primer precio, último precio y variación porcentual.
- Por periodo: contribución de cada año al cambio global.

> Bitcoin opera continuamente, por lo que la volatilidad anualizada utiliza una aproximación de **365 días**.

---

## 📈 Visualizaciones incluidas

El notebook contiene visualizaciones orientadas a responder la rúbrica de forma clara y visual:

| Visualización | Qué permite observar |
|---|---|
| Precio diario | Tendencia general y ciclos de mercado. |
| Escala logarítmica | Crecimiento relativo entre etapas con niveles de precio muy diferentes. |
| Medias móviles | Tendencia suavizada y posibles cambios de comportamiento. |
| Rendimientos diarios | Variación relativa y días extremos. |
| Histograma y KDE | Distribución de los rendimientos. |
| Boxplot por año | Diferencias de dispersión y valores extremos entre años. |
| Volatilidad móvil | Cambios en el nivel de riesgo a lo largo del tiempo. |
| Drawdown | Correcciones y pérdidas desde máximos anteriores. |
| Variación anual | Periodos de crecimiento y caída. |
| Heatmap mensual | Patrones de variación por año y mes. |
| Comparación temporal | Diferencia entre el inicio, el final y los periodos intermedios. |

---

## 🤖 Modelamiento temporal

Se implementa un baseline ingenuo:

> La predicción del siguiente precio es el último precio observado.

Características metodológicas:

- Separación temporal 80/20.
- No se mezclan aleatoriamente los registros.
- El conjunto de prueba ocurre después del conjunto de entrenamiento.
- Se calculan:
  - **MAE**: error absoluto medio.
  - **RMSE**: raíz del error cuadrático medio.
  - **MAPE**: error porcentual absoluto medio.

Este modelo no busca demostrar causalidad ni realizar recomendaciones de inversión. Su objetivo es proporcionar una referencia sencilla y reproducible para valorar el desempeño de modelos posteriores.

---

## 🧠 E2 — Interpretación de resultados

El notebook genera una interpretación automática con los valores calculados, incluyendo:

- dirección del cambio global;
- precio inicial y final;
- cambio absoluto y porcentual;
- mínimo y máximo históricos disponibles en la base;
- año con mejor variación;
- año con peor variación;
- volatilidad anualizada;
- máximo `drawdown`;
- riesgos y limitaciones;
- fecha final disponible en la base.

### Interpretación recomendada

El precio inicial y final no deben analizarse de manera aislada. Una conclusión completa debe considerar:

1. La trayectoria completa.
2. Los periodos de crecimiento y corrección.
3. Los rendimientos diarios.
4. La volatilidad.
5. Las pérdidas desde máximos.
6. La calidad y continuidad de los datos.

El análisis es descriptivo y educativo. No constituye asesoría financiera ni permite inferir causalidad únicamente a partir de los precios observados.

---

## ✅ Correspondencia con la rúbrica de evaluación

### Resultado de Aprendizaje — RA 1.1

> Analiza y explica los fundamentos de la ingeniería de datos y de la ingeniería de características dentro del proceso de Ciencia de Datos, incluyendo los niveles de medición de los datos y las técnicas de transformación.

| Criterio de la rúbrica | Evidencia en el proyecto |
|---|---|
| Comprensión del problema | El README y el notebook explican el objetivo, las variables y la adaptación del reto de portafolio a una serie temporal de Bitcoin. |
| Preparación de datos | Carga robusta, conversión de tipos, ordenamiento y validación de estructura. |
| Calidad de datos | Revisión de faltantes, duplicados, precios no positivos y huecos temporales. |
| Niveles de medición | Clasificación explicada de fecha, precio y rendimientos. |
| Transformación de datos | Creación de cambios, rendimientos, variables calendario, medias móviles, volatilidad y drawdown. |
| Análisis exploratorio | Estadísticos descriptivos, extremos, periodos, rendimientos positivos/negativos y tablas anual/mensual. |
| Comparación de periodos | Comparación entre fecha inicial y final, además del análisis por año y mes. |
| Visualización | Gráficos de precio, escala logarítmica, medias móviles, rendimientos, distribución, volatilidad, drawdown, variación anual y heatmap. |
| Modelamiento | Baseline temporal con separación cronológica y métricas MAE, RMSE y MAPE. |
| Interpretación | Respuestas automáticas, discusión de riesgo, limitaciones y conclusión basada en datos. |
| Comunicación | Notebook organizado por etapas, tablas, gráficos, títulos y explicaciones en español. |

### Escala de desempeño

#### 🟥 Nivel bajo

- No se carga o valida correctamente la base.
- No se explican las variables.
- No se calculan cambios ni rendimientos.
- No hay interpretación sustentada en resultados.

#### 🟨 Nivel medio

- Se realiza parte del EDA.
- Se calculan algunas variaciones.
- Hay gráficos o tablas, pero la interpretación es parcial.
- No se analizan suficientemente riesgo, calidad de datos o periodos intermedios.

#### 🟩 Nivel alto

- El flujo completo es reproducible.
- Se justifican las transformaciones.
- Se revisa la calidad de los datos.
- Se usan medidas relativas, volatilidad y drawdown.
- Se comparan fechas y periodos.
- Las visualizaciones respaldan la explicación.
- El modelamiento respeta el orden temporal.
- La conclusión integra resultados, riesgos y limitaciones.

---

## 🎙️ Aspectos que requieren la mesa redonda

El código puede demostrar el análisis técnico, pero no puede evaluar completamente:

- participación en la mesa redonda;
- claridad de la exposición oral;
- capacidad para responder preguntas;
- discusión de diferencias entre grupos;
- defensa de las decisiones metodológicas;
- trabajo colaborativo.

Para la presentación oral, se recomienda explicar en este orden:

1. **Qué contiene la base.**
2. **Qué cambios se realizaron.**
3. **Cuál fue el comportamiento general del precio.**
4. **Qué periodo tuvo mayor crecimiento o caída.**
5. **Qué riesgos se observaron.**
6. **Cuál es la conclusión principal.**

---

## ▶️ Cómo ejecutar el proyecto

### Opción 1: Jupyter Notebook

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook Analisis_CBBTCUSD_Bitcoin.ipynb
```

### Opción 2: JupyterLab

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyterlab
jupyter lab
```

### Opción 3: Google Colab

1. Abre el notebook en Colab.
2. Sube `CBBTCUSD (1).csv` al entorno.
3. Ejecuta las celdas en orden.

---

## ⚠️ Limitaciones

- El análisis depende de las fechas disponibles en el CSV.
- Los valores faltantes pueden afectar rendimientos y ventanas móviles.
- El precio de Bitcoin es altamente volátil.
- El baseline no es un modelo financiero completo.
- No se incluyen variables externas como inflación, tasas de interés, regulación, volumen, liquidez o noticias.
- No se deben realizar afirmaciones causales únicamente con esta base.
- La conclusión no debe interpretarse como recomendación de inversión.

---

## 📌 Conclusión del proyecto

Este proyecto demuestra un flujo completo de Ciencia de Datos aplicado a una serie temporal financiera: se parte de datos crudos, se verifica su calidad, se transforman las variables, se explora el comportamiento, se construyen visualizaciones, se evalúa un baseline temporal y se comunica una conclusión sustentada en evidencia.

La principal enseñanza es que una variación entre dos fechas no es suficiente para comprender una serie financiera. Es necesario analizar también la trayectoria, los rendimientos, la volatilidad, los máximos, las caídas y las limitaciones de los datos.
