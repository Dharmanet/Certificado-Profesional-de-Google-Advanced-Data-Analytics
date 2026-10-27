# Proyecto de Análisis de Datos: Optimización Logística de Rutas de Aseo Público en Zapopan

Este documento detalla las 5 fases del proyecto de análisis de datos para evaluar el cambio del patio de maniobras de Constituyentes (Calle 2) a Tesistán, utilizando metodologías estándar de la industria (CRISP-DM, Data Storytelling y el Principio de la Pirámide de Minto).

---

## Fase 1: Propuesta de Proyecto (Metodología CRISP-DM)

**Prompt para Gemini:**
> "Actúa como un Líder de Proyectos de Datos Senior. Redacta el esquema para un documento compartido y la agenda de la reunión inicial utilizando la fase de 'Comprensión del Negocio' de la metodología CRISP-DM. El objetivo es analizar el impacto logístico de mover nuestro patio de maniobras de Constituyentes a Tesistán, evaluando cómo afectará los tiempos de traslado (inicio/fin de jornada) para los camiones de los 8 sectores de recolección en Zapopan. Define los entregables clave."

**Respuesta Generada:**
**Objetivo del Proyecto (Business Understanding)**
Evaluar la viabilidad técnica, operativa y financiera de reubicar el patio de maniobras a Tesistán, determinando qué camiones y sectores operativos (de los 8 existentes) resultan viables bajo el nuevo esquema de distancias.

**Agenda de la Sesión Inicial**
1. **Definición del Problema:** Impacto del cierre de Matatlán y la saturación operativa actual en Constituyentes.
2. **Identificación de Variables Críticas:** Tiempos de trayecto (origen-destino), consumo de combustible, depreciación de unidades y tiempos muertos.
3. **Métricas de Éxito (KPIs):** Porcentaje de viabilidad por unidad.
4. **Flujo de Trabajo (Data Mining):** Responsables de recolección de datos y limpieza de registros.
5. **Calendario y Entregables:** Fechas para el Análisis Exploratorio de Datos (EDA) y entrega del Resumen Ejecutivo.

---

## Fase 2: Limpieza de Datos (Codificación en Python)

**Prompt para Gemini:**
> "Escribe código robusto en Python utilizando `pandas` para limpiar un conjunto de datos logísticos sobre rutas y tiempos de camiones recolectores. Aplica mejores prácticas de limpieza: elimina valores nulos críticos, descarta filas duplicadas y utiliza el método del Rango Intercuartílico (IQR) para identificar y filtrar valores atípicos (outliers) en los tiempos de traslado."

**Respuesta Generada:**
```python
import pandas as pd
import numpy as np

# 1. Carga de datos
df = pd.read_csv('rutas_zapopan_sectores.csv')

# 2. Tratamiento de Valores Nulos
df_clean = df.dropna(subset=['sector', 'tiempo_traslado_minutos', 'id_camion'])

# 3. Eliminación de Duplicados Exactos
df_clean = df_clean.drop_duplicates()

# 4. Tratamiento de Valores Atípicos (Outliers) mediante IQR
Q1 = df_clean['tiempo_traslado_minutos'].quantile(0.25)
Q3 = df_clean['tiempo_traslado_minutos'].quantile(0.75)
IQR = Q3 - Q1

limite_inferior = Q1 - 1.5 * IQR
limite_superior = Q3 + 1.5 * IQR

# Filtramos conservando solo los traslados estadísticamente lógicos
df_final = df_clean[(df_clean['tiempo_traslado_minutos'] >= limite_inferior) & 
                    (df_clean['tiempo_traslado_minutos'] <= limite_superior)]
```

---

## Fase 3: Visualización de Datos (Data Storytelling)

**Prompt para Gemini:**
> "Basado en principios de 'Data Storytelling', sugiere visualizaciones de datos avanzadas para analizar el impacto del cambio de patio de maniobras a Tesistán. Tenemos datos de tiempos de traslado, distribución por los 8 sectores, y porcentajes de viabilidad operativa. Explica el objetivo técnico de cada gráfico propuesto."

**Respuesta Generada:**
1. **Diagrama de Caja y Bigotes (Boxplot) por Sector:**
   * **Objetivo:** Mostrar la dispersión y mediana de los tiempos de traslado hacia Tesistán vs. Constituyentes. Revela la volatilidad y márgenes de error por tráfico.
2. **Mapa de Calor (Heatmap) de Correlación Espacio-Temporal:**
   * **Objetivo:** Cruzar las horas de salida/entrada con los 8 sectores operativos para identificar ventanas de tiempo colapsadas.
3. **Gráfico de Barras Apiladas 100%:**
   * **Objetivo:** Representar el estado de viabilidad (Viable / Revisión / No Viable) de las unidades para cada uno de los 8 sectores.

---

## Fase 4: Modelado de Aprendizaje Automático

**Prompt para Gemini:**
> "Explica los principales usos y ventajas del aprendizaje automático (Machine Learning) aplicado a la logística gubernamental. Detalla cómo algoritmos supervisados pueden usarse para predecir la viabilidad de los camiones recolectores al cambiar de patio, y cómo esto supera a la analítica tradicional."

**Respuesta Generada:**
* **Predicción Multivariable Compleja:** Modelos como Random Forest pueden procesar clima, historial de tráfico y tonelaje simultáneamente para predecir el tiempo exacto de traslado.
* **Clasificación de Viabilidad:** Se puede entrenar un algoritmo supervisado para categorizar automáticamente si un camión será "Viable" operando desde Tesistán, minimizando el sesgo humano.
* **Optimización Prescriptiva:** El modelo sugiere redistribución dinámica de sectores, asignando camiones más rápidos a rutas más alejadas del nuevo patio.

---

## Fase 5: Resumen Ejecutivo (Principio de Minto)

**Prompt para Gemini:**
> "Enumera las mejores prácticas para redactar un Resumen Ejecutivo sobre un modelo predictivo de datos, dirigido a la dirección del Ayuntamiento (público sin formación técnica). Estructura los consejos utilizando el 'Principio de la Pirámide de Minto'."

**Respuesta Generada:**
1. **Veredicto en el Párrafo 1 (La Cima):** Inicia con la conclusión. Ej: "Mover el patio a Tesistán es operativo para el 65% de la flota, pero incrementará costos de combustible un 12%."
2. **Argumentos Clave (El Centro):** Agrupa hallazgos en viñetas (Impacto en Tiempo, Impacto Financiero).
3. **Evidencia Visual (La Base):** Omite métricas complejas (como RMSE). Incluye solo gráficos orientados a la decisión de negocio.
4. **Llamado a la Acción:** Ofrece escenarios claros (Ej. "Migración total vs. Migración parcial").
