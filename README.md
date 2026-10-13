# 📈 Reto de Ciencia de Datos: análisis temporal de Bitcoin

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Estado](https://img.shields.io/badge/estado-reproducible-success.svg)](#-reproducibilidad)

## 1. Descripción general

Este proyecto adapta el reto **“¿Qué pasó con el portafolio?”** a una serie temporal financiera de Bitcoin. En lugar de comparar posiciones nacionales e internacionales en dos fechas, se estudia la evolución diaria del precio de Bitcoin expresado en dólares estadounidenses.

El propósito es demostrar, mediante un flujo reproducible de Ciencia de Datos, cómo:

- preparar y validar datos temporales;
- identificar los niveles de medición de las variables;
- transformar una serie de precios mediante ingeniería de características;
- realizar análisis exploratorio de datos (EDA);
- comparar fechas, años y meses;
- comunicar resultados mediante tablas y visualizaciones;
- aplicar un modelo temporal de referencia;
- interpretar resultados, riesgos y limitaciones.

> **Alcance:** el análisis es académico y descriptivo. No constituye asesoría financiera ni permite establecer causalidad a partir del precio observado.

---

## 2. Pregunta orientadora

> **¿Cómo cambió el precio de Bitcoin durante el periodo disponible y qué periodos, características y medidas de riesgo explican mejor ese comportamiento?**

### Preguntas específicas

1. ¿Cuál fue el precio inicial y final?
2. ¿Cuál fue el cambio absoluto y porcentual?
3. ¿Cuáles fueron los valores mínimo y máximo y cuándo ocurrieron?
4. ¿Qué años y meses presentaron mayores aumentos o caídas?
5. ¿Qué tan variable y riesgosa fue la serie?
6. ¿Qué tan bien funciona un baseline temporal sencillo?
7. ¿Qué conclusiones pueden sostenerse y cuáles no pueden afirmarse con esta base?

---

## 3. Archivos del proyecto

| Archivo | Función | Evidencia de la rúbrica |
|---|---|---|
| [`Analisis_CBBTCUSD_Bitcoin.ipynb`](Analisis_CBBTCUSD_Bitcoin.ipynb) | Notebook ejecutable con E0, E1, ingeniería de características, modelamiento y E2. | Evidencia técnica principal. |
| [`CBBTCUSD (1).csv`](CBBTCUSD%20(1).csv) | Base diaria de precios, si se incorpora al repositorio. | Fuente de datos. |
| `README.md` | Contexto, metodología, rúbrica, reproducibilidad y guía de presentación. | Evidencia de comunicación y planificación. |

Si el CSV no está incluido en el repositorio, debe colocarse junto al notebook o cargarse en `/content/` en Google Colab.

---

## 4. Flujo metodológico

```text
Problema
   ↓
E0. Preparación y calidad
   ↓
Niveles de medición
   ↓
Ingeniería de características
   ↓
E1. Análisis exploratorio
   ↓
Visualización y comparación temporal
   ↓
Modelo baseline sin fuga temporal
   ↓
E2. Interpretación, conclusión y limitaciones
   ↓
Comunicación y mesa redonda
```

La secuencia responde al proceso de Ciencia de Datos y permite evidenciar no solo el resultado, sino también las decisiones tomadas para obtenerlo.

---

## 5. E0 — Preparación y calidad de datos

El notebook realiza estas comprobaciones:

- búsqueda robusta del archivo CSV;
- validación de las columnas `observation_date` y `CBBTCUSD`;
- conversión explícita de tipos;
- ordenamiento cronológico;
- detección de fechas inválidas;
- detección de precios faltantes;
- detección de fechas duplicadas;
- detección de precios no positivos;
- identificación de huecos temporales;
- confirmación del periodo cubierto;
- uso explícito de `dropna` solo cuando una operación requiere un valor observado.

### Decisión sobre datos faltantes

Los faltantes no se convierten automáticamente en cero ni se imputan sin justificación. Se reportan primero. Las operaciones que necesitan precios consecutivos o ventanas completas producen `NaN` cuando no existe información suficiente.

Esta decisión evita fabricar variaciones artificiales y constituye una práctica de calidad de datos.

---

## 6. Niveles de medición

| Variable | Nivel / naturaleza | Uso analítico |
|---|---|---|
| `observation_date` | Temporal y ordinal | Ordenar observaciones, agrupar por año/mes y estudiar evolución. |
| `CBBTCUSD` | Cuantitativa continua de razón | Calcular diferencias, proporciones, promedios y extremos. |
| Cambio absoluto | Cuantitativa de intervalo monetario | Medir cuánto cambió el precio en USD. |
| Rendimiento simple | Cuantitativa relativa | Comparar variaciones entre días con distintos niveles de precio. |
| Rendimiento logarítmico | Cuantitativa transformada | Analizar cambios acumulables y distribución de retornos. |
| Drawdown | Medida relativa de pérdida | Estudiar caídas desde máximos acumulados. |

El rendimiento porcentual es especialmente importante porque una diferencia de USD 1,000 no representa lo mismo cuando Bitcoin cotiza a USD 5,000 que cuando cotiza a USD 100,000.

---

## 7. Ingeniería de características

El notebook transforma la serie original en variables analíticas:

- `cambio_absoluto`;
- `rendimiento_diario`;
- `rendimiento_log`;
- año, mes, trimestre y día de la semana;
- `maximo_acumulado`;
- `drawdown_pct`;
- medias móviles de 7, 30 y 90 días;
- volatilidad móvil de 30 días anualizada;
- indicadores `subida` y `caida`.

### Justificación

Estas características permiten analizar cuatro dimensiones:

1. **Nivel:** precio observado.
2. **Cambio:** diferencia absoluta y rendimiento.
3. **Tendencia:** medias móviles.
4. **Riesgo:** volatilidad y drawdown.

---

## 8. E1 — Análisis exploratorio de datos

### Indicadores calculados

- fecha inicial y final;
- número de observaciones;
- precio inicial y final;
- cambio absoluto y porcentual;
- mínimo, máximo y fechas correspondientes;
- estadísticos descriptivos;
- media y dispersión de rendimientos;
- días positivos, negativos y sin cambio;
- mejor y peor día;
- volatilidad diaria;
- volatilidad anualizada con 365 días;
- máximo drawdown.

### Comparación temporal

Se construyen tablas:

- **anuales:** primer precio, último precio, mínimo, máximo y variación;
- **mensuales:** primer precio, último precio y variación;
- **por periodos:** identificación de los años con mayor contribución positiva o negativa.

La comparación inicial/final responde al planteamiento del reto, mientras que la desagregación anual y mensual evita una interpretación incompleta.

---

## 9. Visualizaciones y propósito

| Gráfico | Evidencia que aporta |
|---|---|
| Precio diario | Tendencia, ciclos y cambios de régimen visuales. |
| Precio en escala logarítmica | Comparación de crecimientos relativos a lo largo del tiempo. |
| Medias móviles | Tendencia suavizada y comportamiento de corto/mediano plazo. |
| Rendimiento diario | Variaciones relativas y días extremos. |
| Histograma/KDE | Forma y concentración de la distribución de retornos. |
| Boxplot anual | Comparación de dispersión y valores extremos por año. |
| Volatilidad móvil | Evolución dinámica del riesgo. |
| Drawdown | Profundidad de las correcciones desde máximos. |
| Barras de variación anual | Periodos de crecimiento y caída. |
| Heatmap mensual | Patrones de variación por año y mes. |
| Real vs. baseline | Evaluación visual del modelo temporal. |

Todas las visualizaciones incluyen títulos, ejes, unidades y leyendas cuando son necesarias.

---

## 10. Modelamiento y prevención de fuga temporal

Se utiliza un baseline ingenuo que predice el precio siguiente con el último precio disponible.

### Protocolo

- división cronológica 80/20;
- no se mezclan los registros;
- el entrenamiento precede al conjunto de prueba;
- el modelo no utiliza información futura;
- evaluación con MAE, RMSE y MAPE.

### Interpretación correcta

El baseline no pretende ser un modelo predictivo definitivo. Sirve como punto de comparación para futuros modelos de series de tiempo. Un error bajo no implica causalidad ni garantiza resultados futuros.

---

## 11. E2 — Interpretación y conclusión

El notebook genera una interpretación basada en los valores calculados y debe responder:

### ¿Cómo cambió Bitcoin?

Se comparan fecha inicial, fecha final, cambio absoluto, variación porcentual, mínimo, máximo y trayectoria intermedia.

### ¿Qué periodos explican el resultado?

Se identifican el mejor y peor año, junto con los cambios mensuales y el aporte de cada periodo al cambio global.

### ¿Qué riesgos se observan?

Se utilizan volatilidad diaria, volatilidad anualizada, mejores/peores días y máximo drawdown.

### ¿Cuál es la conclusión?

La conclusión debe integrar:

- resultado global;
- trayectoria y ciclos;
- rendimiento relativo;
- riesgo;
- calidad de los datos;
- limitaciones del análisis.

No es suficiente afirmar que “el precio subió” o “el precio bajó”; se debe justificar la afirmación con valores, tablas y gráficos.

---

## 12. Matriz de cumplimiento de la rúbrica

### Resultado de Aprendizaje RA 1.1

> Analiza y explica los fundamentos de la ingeniería de datos y de la ingeniería de características dentro del proceso de Ciencia de Datos, incluyendo niveles de medición y técnicas de transformación.

| Criterio | Evidencia concreta | Ubicación |
|---|---|---|
| Comprensión del problema | Pregunta orientadora, objetivos y adaptación explícita del reto original. | README y primera sección del notebook |
| Preparación de datos | Carga, tipos, ordenamiento y selección de columnas. | Sección E0 |
| Calidad de datos | Faltantes, duplicados, valores inválidos y huecos temporales. | Sección E0 |
| Niveles de medición | Tabla con naturaleza y uso de cada variable. | README y E0 |
| Ingeniería de características | Cambios, retornos, calendario, ventanas, volatilidad y drawdown. | Sección de características |
| EDA | Estadísticos, extremos, días positivos/negativos y resúmenes temporales. | Sección E1 |
| Comparación | Fecha inicial/final y comparación anual/mensual. | E1 |
| Visualización | Gráficos de nivel, retornos, tendencia, distribución y riesgo. | E1 |
| Modelamiento | Baseline, split temporal y métricas. | Sección de modelamiento |
| Interpretación | Respuestas automáticas y conclusión basada en datos. | E2 |
| Limitaciones | Discusión de faltantes, volatilidad, causalidad y variables no observadas. | E2 y README |
| Comunicación | Markdown, tablas, títulos, unidades, guía de ejecución y presentación. | Notebook y README |

### Criterios para aspirar al nivel alto

- El notebook se ejecuta en orden sin depender de cifras escritas manualmente.
- Cada transformación tiene una justificación.
- Los faltantes y decisiones de limpieza se documentan.
- Se analizan niveles y variaciones relativas.
- Las gráficas respaldan, no reemplazan, la interpretación.
- El modelo respeta el orden temporal.
- La conclusión utiliza resultados calculados.
- Se reconocen límites y no se realizan afirmaciones causales.

---

## 13. Guion para la mesa redonda

Cada grupo puede presentar en 3–5 minutos:

1. **Problema:** qué representa la base.
2. **Calidad:** qué faltantes o irregularidades se detectaron.
3. **Transformación:** qué características se crearon y por qué.
4. **Resultado:** cómo cambió el precio entre los extremos.
5. **Periodos clave:** qué año o mes tuvo mayor crecimiento o caída.
6. **Riesgo:** volatilidad y drawdown observados.
7. **Conclusión:** qué ocurrió y qué no puede afirmarse.
8. **Limitación:** qué información adicional sería necesaria.

El código no puede evaluar por sí solo la participación, la claridad oral, la defensa de decisiones ni el trabajo colaborativo; esos elementos deben demostrarse durante la mesa redonda.

---

## 14. Reproducibilidad

### Instalación

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### Ejecución

```bash
jupyter notebook Analisis_CBBTCUSD_Bitcoin.ipynb
```

También puede ejecutarse en JupyterLab o Google Colab. En Colab, sube primero el archivo `CBBTCUSD (1).csv`.

### Verificación mínima

Antes de interpretar resultados, confirma que el notebook:

- encuentra el CSV;
- muestra las columnas esperadas;
- reporta el periodo real disponible;
- no presenta errores en las celdas;
- genera tablas y gráficos;
- calcula las métricas del baseline;
- imprime la conclusión con valores provenientes de la base.

---

## 15. Limitaciones

- El análisis depende del periodo que realmente contiene el CSV.
- Un valor faltante puede afectar rendimientos y ventanas móviles.
- Bitcoin es un activo altamente volátil.
- El precio no incluye volumen, liquidez, inflación, tasas, regulación, noticias u otras variables externas.
- El baseline es intencionalmente sencillo.
- No se realizan pruebas de causalidad.
- Las conclusiones son descriptivas y no constituyen recomendaciones de inversión.

---

## 16. Cierre

El proyecto evidencia un proceso completo de Ciencia de Datos aplicado a una serie financiera: se define una pregunta, se inspecciona la calidad de los datos, se transforman variables, se explora el comportamiento, se visualizan patrones, se evalúa un baseline respetando el tiempo y se comunica una conclusión sustentada.

La idea central es que comparar únicamente el precio inicial y final no basta. Una interpretación sólida también considera rendimientos, tendencia, volatilidad, drawdown, periodos intermedios, calidad de datos y limitaciones metodológicas.
