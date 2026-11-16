# Documento de Estrategia PACE - Proyecto Automatidata

## PACE: Etapa de Planificación (Plan)

**¿Cómo puede prepararse mejor para comprender y organizar la información proporcionada?**

Revisando detenidamente el diccionario de datos de la TLC para entender qué representa cada una de las 18 variables. También verificando el entorno de Jupyter Notebook, asegurando que las bibliotecas necesarias como `pandas` y `numpy` estén importadas, y revisando los requerimientos clave solicitados por Luana, como la validación de los tipos de datos y los datos nulos.

**¿Qué libros de códigos de seguimiento y de autorrevisión le ayudarán a realizar este trabajo?**

El cuaderno interactivo de Python proporcionado en el laboratorio, la documentación oficial de la biblioteca `pandas` —para funciones como `read_csv`, `info` y `describe`— y las guías de estilo de codificación de Python (PEP 8) para mantener el código limpio.

**¿Qué actividades adicionales realizaría un alumno ingenioso antes de empezar a programar?**

Comprender a fondo el contexto del negocio repasando la propuesta del proyecto. Además, buscar información básica sobre el funcionamiento del sistema de taxis de Nueva York, como tarifas base, recargos y zonas TLC, para tener una referencia que permita identificar valores atípicos o errores de registro.

## PACE: Etapa de Análisis (Analyze)

**¿La información disponible será suficiente para lograr el objetivo basándose en su intuición y el análisis de las variables?**

Sí, para esta etapa inicial de inspección, las 18 variables y las 408.294 filas proporcionan la información transaccional necesaria. Sin embargo, para construir modelos predictivos precisos más adelante, probablemente se necesitarán datos adicionales que proporcionen contexto a los viajes, como factores climatológicos o índices de tráfico.

**¿Cómo construiría estadísticas resumidas de los dataframes y evaluaría el rango mínimo y máximo de los datos?**

Utilizaría los métodos nativos de `pandas`. Aplicaría `df.describe()` para obtener estadísticas descriptivas, como conteo, media, desviación estándar, mínimos, máximos y cuartiles, de las columnas numéricas. Para revisar los tipos de datos y el recuento de valores no nulos, utilizaría `df.info()`.

**¿Las medias de alguna de las variables de datos parecen inusuales? ¿Puede describir los datos de los intervalos?**

Al ejecutar los resúmenes, buscaría activamente anomalías lógicas. Por ejemplo, medias de distancias de viaje demasiado bajas, tarifas promedio negativas o recuentos de pasajeros inconsistentes. Las fechas y horas requerirían una revisión para confirmar que los viajes tengan una duración lógica y evitar registros con duraciones de segundos o de días completos.

## PACE: Etapa de Construcción (Construct)

**¿Qué variables significativas podría crear o modificar?**

Podría crear una variable `trip_duration` calculando la diferencia entre `tpep_dropoff_datetime` y `tpep_pickup_datetime`. También podría crear una variable `tip_percentage` dividiendo `tip_amount` entre `fare_amount`, siempre que la tarifa sea mayor que cero. Estas variables ayudarían a analizar la duración de los viajes y el comportamiento de las propinas.

**¿Qué pasos tomaría para preparar los datos?**

Convertiría las columnas de fecha y hora al tipo `datetime`, verificaría valores nulos y duplicados, revisaría valores negativos o imposibles y confirmaría que las columnas numéricas tengan los tipos de datos correctos. También documentaría las columnas irrelevantes para el análisis posterior.

## PACE: Etapa de Ejecución (Execute)

**Dado su conocimiento actual de los datos, ¿qué recomendaría inicialmente a su gerente investigar más a fondo antes de realizar el análisis exploratorio de datos (EDA)?**

Recomendaría investigar el impacto del campo `tip_amount`. El diccionario de datos indica que solo se rellena automáticamente para propinas pagadas con tarjeta de crédito, por lo que los pagos en efectivo pueden aparecer con propina de 0. Esto introduciría un sesgo importante si se intenta analizar o predecir propinas sin considerar el tipo de pago.

**¿Qué datos se presentan inicialmente como portadores de anomalías?**

Las fechas y horas (`tpep_pickup_datetime` y `tpep_dropoff_datetime`) pueden presentar errores de registro. También `fare_amount` podría contener valores atípicos muy altos o negativos, al igual que los viajes registrados con cero pasajeros o con distancias imposibles.

**¿Qué tipos adicionales de datos podrían reforzar este conjunto de datos?**

Los datos meteorológicos históricos podrían ayudar, ya que las precipitaciones pueden aumentar la demanda y alterar los tiempos de viaje. También sería útil un diccionario geográfico que permita relacionar los códigos de zona (`PULocationID` y `DOLocationID`) con nombres de vecindarios reales para facilitar el análisis geográfico.
