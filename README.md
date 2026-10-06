# analisis-movilidad-latam

Sprint 5 | Proyecto 5: Movilidad urbana y productividad económica

## 🌎 Contexto y objetivos

Desarrollé este proyecto a partir de un escenario de análisis para el American Development Bank. Asumí el rol de analista de datos con el propósito de estudiar cómo la movilidad urbana se relaciona con el desempeño económico de las ciudades.

Mi objetivo fue integrar información sobre congestión, tiempos de viaje y retrasos con indicadores de PIB per cápita, desempleo y población. Con esta base, busqué aportar información para orientar la evaluación de inversiones en infraestructura de transporte y bienestar urbano.

Trabajé con dos fuentes de información:

- **TomTom Traffic Index:** datos de movilidad urbana y condiciones de tráfico.
- **OECD Cities:** indicadores económicos, demográficos y ambientales por ciudad.

### Preguntas de negocio

Durante el proyecto, me planteé las siguientes preguntas:

- ¿Qué ciudades presentan alta congestión y baja productividad económica?
- ¿Cuáles combinan una movilidad más eficiente con indicadores económicos favorables?
- ¿Qué variables muestran patrones de relación más claros con el desarrollo urbano?

### Objetivos del análisis

Para responder estas preguntas, me propuse:

- Construir un dataset único a partir de dos fuentes diferentes.
- Limpiar y estandarizar nombres, formatos y tipos de datos.
- Validar la consistencia de la información antes de combinarla.
- Enfocar el análisis en los registros correspondientes a 2024.
- Calcular indicadores agregados por ciudad y año.
- Explorar las variables mediante estadísticas descriptivas y visualizaciones.
- Documentar el procedimiento en un Jupyter Notebook.
- Exportar una tabla final preparada para análisis posteriores.

## 🛠️ Herramientas y fuentes

### Herramientas utilizadas

| Herramienta | Cómo la utilicé |
| --- | --- |
| Jupyter Notebook | Organicé el proyecto y documenté el código, las decisiones y las interpretaciones. |
| Python | Desarrollé el flujo de preparación y análisis de los datos. |
| pandas | Cargué, limpié, transformé, agregué y combiné los datasets. |
| NumPy | Apoyé el tratamiento de variables numéricas y valores faltantes. |
| seaborn | Construí visualizaciones para explorar distribuciones y relaciones. |
| matplotlib | Ajusté la presentación de los gráficos y sus elementos visuales. |

### Datasets del proyecto

| Archivo | Fuente | Información principal | Descarga |
| --- | --- | --- | --- |
| `tomtom_traffic.csv` | TomTom Traffic Index | Congestión, retrasos y tiempos de viaje por ciudad. | [Descargar dataset de tráfico](https://drive.google.com/uc?export=download&id=1ZnjjZcDt8dx7sSNBNXiDMHmIj3q3HEN3) |
| `oecd_city_economy.csv` | OECD Cities | PIB per cápita, desempleo, contaminación y población por ciudad y año. | [Descargar dataset económico](https://drive.google.com/uc?export=download&id=1Tbf7qdKnubLhOCoEa_rZY-KF9roQPTLp) |

Utilicé ambas fuentes de manera complementaria: la primera describe las condiciones de movilidad y la segunda proporciona el contexto económico de cada ciudad.

## 📚 Diccionario de datos

### Dataset de movilidad: `tomtom_traffic.csv`

Trabajé con registros que representan actualizaciones puntuales del estado del tráfico en las ciudades monitoreadas por TomTom.

En el siguiente diccionario describo el significado de las columnas originales y cómo las interpreté dentro del proyecto.

| Columna | Tipo de dato | Ejemplo | Cómo interpreté la variable |
| --- | --- | --- | --- |
| `Country` | STRING | `ARE` | Identifiqué el país mediante su código ISO-3; por ejemplo, `ARE` corresponde a Emiratos Árabes Unidos. |
| `City` | STRING | `abu-dhabi` | Identifiqué la ciudad o el área metropolitana mediante su nombre estandarizado. |
| `UpdateTimeUTC` | DATETIME | `2025-01-13 04:01:30.001` | Tomé esta columna como la fecha y hora UTC de la actualización del tráfico. |
| `JamsDelay` | FLOAT | `650.7` | Interpreté el valor como el retraso total, en minutos, provocado por la congestión en las vías monitoreadas. |
| `TrafficIndexLive` | FLOAT | `36.0` | Interpreté el índice de tráfico actual en una escala de 0 a 100, donde un valor mayor representa mayor congestión. |
| `JamsLengthInKms` | FLOAT | `109.1` | Identifiqué la longitud total, en kilómetros, de los embotellamientos activos. |
| `JamsCount` | INTEGER | `162` | Identifiqué la cantidad de embotellamientos activos en el momento del registro. |
| `TrafficIndexWeekAgo` | FLOAT | `30.0` | Reconocí este campo como el índice de tráfico registrado una semana antes. |
| `UpdateTimeUTCWeekAgo` | DATETIME | `2025-01-06 04:01:30.000` | Identifiqué la fecha de referencia de la medición de la semana anterior. |
| `TravelTimeLivePer10KmsMins` | FLOAT | `11.614767` | Interpreté el tiempo medio actual, en minutos, necesario para recorrer 10 kilómetros. |
| `TravelTimeHistoricPer10KmsMins` | FLOAT | `10.26533` | Interpreté el tiempo medio histórico, en minutos, para recorrer 10 kilómetros bajo las condiciones de referencia. |
| `MinsDelay` | FLOAT | `1.349437` | Interpreté la diferencia entre el tiempo actual y el histórico como el retraso medio por cada 10 kilómetros. |

Los ejemplos anteriores ilustran los formatos de las columnas. Mi análisis se enfocó en el año 2024, independientemente del año utilizado en los ejemplos del diccionario.

### Dataset económico: `oecd_city_economy.csv`

Trabajé con indicadores anuales recopilados por la OECD. Cada registro representa una ciudad en un año específico, lo que permite organizar comparaciones entre territorios y periodos.

| Columna | Tipo de dato | Ejemplo | Cómo interpreté la variable |
| --- | --- | --- | --- |
| `Year` | INTEGER | `2023` | Identifiqué el año al que corresponde el registro económico. |
| `City` | STRING | `buenos-aires` | Identifiqué la ciudad o el área metropolitana. |
| `Country` | STRING | `Argentina` | Identifiqué el país al que pertenece la ciudad. |
| `City GDP/capita` | FLOAT | `15782.00` | Interpreté el PIB per cápita en USD como un indicador del desempeño económico por habitante. |
| `Unemployment %` | FLOAT | `6.2` | Interpreté el porcentaje de desempleo de la población económicamente activa. |
| `PM2.5 (μg/m³)` | FLOAT | `15.2` | Identifiqué la concentración media anual de partículas finas como indicador de contaminación del aire. |
| `Population (M)` | FLOAT | `15.30` | Interpreté la población de la ciudad expresada en millones de habitantes. |

## 🔄 Proceso de trabajo

Organicé el análisis mediante un flujo de siete pasos. Utilicé la tabla de tráfico como base principal y resumí sus registros antes de combinarla con los indicadores económicos anuales.

| Paso | Qué hice | Propósito |
| :---: | --- | --- |
| 1 | Cargué y exploré los dos datasets. | Identifiqué columnas, tipos de datos y estructura general de los archivos. |
| 2 | Limpié y corregí los formatos. | Estandaricé nombres de columnas y preparé los tipos de datos para el análisis. |
| 3 | Extraje el año de los registros de tráfico y filtré 2024. | Delimité ambas fuentes al periodo de interés. |
| 4 | Calculé promedios de tráfico por ciudad y año. | Transformé las actualizaciones puntuales en una vista anual consolidada. |
| 5 | Combiné los datasets de tráfico y economía. | Construí una tabla con información de movilidad y contexto económico por ciudad y año. |
| 6 | Visualicé distribuciones y relaciones entre variables. | Exploré patrones, diferencias y posibles comportamientos atípicos. |
| 7 | Elaboré el informe y la reflexión final. | Documenté las interpretaciones, sus implicaciones y los límites del análisis. |

### Criterios de integración

Durante la preparación de la tabla final, consideré los siguientes aspectos:

- **Tabla principal:** utilicé `tomtom_traffic.csv` como fuente de movilidad.
- **Nivel de detalle:** resumí los múltiples registros de tráfico antes de realizar la unión.
- **Periodo:** mantuve el enfoque en 2024.
- **Estandarización:** revisé la correspondencia de ciudades y países entre las fuentes.
- **Unidad de análisis:** organicé el resultado con una fila por ciudad y año.
- **Documentación:** registré las decisiones y transformaciones en el notebook.

La agregación previa fue una parte importante del proceso: me permitió comparar mediciones frecuentes de tráfico con indicadores económicos disponibles a escala anual.

### Organización del notebook

Seguí la secuencia de trabajo del Jupyter Notebook y utilicé sus celdas para organizar:

- La carga y exploración de los archivos.
- Las transformaciones y validaciones.
- La agregación de los indicadores.
- La combinación de las fuentes.
- Las visualizaciones.
- Las interpretaciones y la reflexión final.

## 📦 Entregables y reflexión

### Entregables del proyecto

Mi trabajo se centró en preparar los siguientes entregables:

| Entregable | Contenido |
| --- | --- |
| Jupyter Notebook | Documenté el procedimiento, el código, las visualizaciones y las interpretaciones. |
| Dataset final en CSV | Organicé una tabla unificada de movilidad y economía por ciudad y año, enfocada en 2024. |
| Informe ejecutivo | Sinteticé los patrones observados y sus posibles implicaciones para las preguntas del negocio. |
| Reflexión final | Expuse las decisiones metodológicas, los aprendizajes y las limitaciones del análisis. |

### Exportación del dataset

Al finalizar la preparación de los datos, ejecuté la celda de exportación del notebook para generar el archivo CSV.

Después, localicé el archivo en el explorador de carpetas del entorno de trabajo y lo descargué mediante la opción `Download`.

Con esta exportación, dejé disponible una base estandarizada para continuar el análisis o utilizarla en otras herramientas de visualización.

### Reflexión sobre el alcance

En este proyecto, mi prioridad fue construir una base coherente que permitiera estudiar conjuntamente la movilidad y el desempeño económico urbano.

La preparación de los datos me permitió trabajar con fuentes de distinta frecuencia y nivel de detalle: actualizaciones puntuales de tráfico e indicadores económicos anuales.

En la interpretación, mantuve la diferencia entre identificar relaciones y explicar sus causas. Mi análisis exploratorio sirve como punto de partida para formular preguntas y orientar investigaciones posteriores, sin convertir automáticamente una asociación observada en una recomendación definitiva de inversión.
```
