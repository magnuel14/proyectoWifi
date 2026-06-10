# Proyecto ETL WiFi CSI - Detección de presencia y actividad humana

Este proyecto corresponde al componente práctico de la Semana 1, orientado a la integración, carga y análisis exploratorio de datos mediante PostgreSQL, Docker y Python. El dominio seleccionado fue el análisis de señales WiFi CSI para la detección de presencia, objetos, movimiento y actividad humana, con una posible aplicación futura en seguridad física y ciberseguridad.

## Objetivo general

Construir un flujo inicial de análisis de datos a partir de fuentes heterogéneas relacionadas con señales WiFi CSI, utilizando PostgreSQL como base de datos, Docker como entorno de ejecución y Python para la extracción, transformación, carga y análisis exploratorio de la información.

## Datasets utilizados

Los datasets no se incluyen directamente en el repositorio debido a su tamaño. Para ejecutar el notebook, deben descargarse manualmente desde Kaggle y ubicarse en la estructura indicada.

### 1. WiFi CSI Object Detection

Link:
https://www.kaggle.com/datasets/armandokeller/wifi-csi-object-detection

Archivo utilizado en el proyecto:

```text
data/postgres/raw_data.csv
```

DataFrame generado:

```text
df_postgres
```

Este dataset fue seleccionado porque contiene mediciones estructuradas de señales WiFi CSI, principalmente variables de amplitud y fase, además de información asociada al tipo de objeto, posición y configuración. Su uso permite analizar cómo cambia la señal inalámbrica cuando existen objetos o variaciones físicas en el entorno.

En el proyecto, este dataset se carga en PostgreSQL y posteriormente se consulta desde Python para generar el primer DataFrame principal.

---

### 2. WiFi CSI Human Activity Recognition

Link:
https://www.kaggle.com/datasets/alanwake12/wifi-csi-human-activity-recognition

Archivo utilizado en el proyecto:

```text
data/json/data_apml_min.json
```

DataFrame generado:

```text
df_har
```

Este dataset fue seleccionado porque está orientado al reconocimiento de actividad humana mediante señales WiFi CSI. A partir del archivo JSON se construyen variables resumidas como promedio, máximo, mínimo y desviación estándar de la señal.

Su aporte principal es permitir una aproximación inicial al análisis de patrones de señal relacionados con presencia o movimiento humano. En una investigación futura, estas variables podrían servir como base para modelos de inteligencia artificial orientados a clasificar actividades humanas.

---

### 3. HAR-ORT 5GHz 80MHz Nexmon Extracted CSI

Link:
https://www.kaggle.com/datasets/brunobellizzi/har-ort-5ghz-80mhz-nexmon-extracted-csi?resource=download

Archivo utilizado en el proyecto:

```text
data/har_ort/annotations.csv
```

DataFrame generado:

```text
df_annotations
```

Este dataset fue seleccionado porque aporta anotaciones de actividades humanas asociadas a archivos de captura PCAP. En esta práctica se utiliza el archivo `annotations.csv`, que contiene información como actividad, usuario y nombre del archivo de captura.

Aunque los archivos PCAP no se procesan directamente en esta fase, representan una fuente de datos cruda que podría ser utilizada en una etapa posterior para extraer características más avanzadas de señales WiFi CSI y entrenar modelos de clasificación de actividades.

## Justificación de la selección de los datasets

Los tres datasets fueron seleccionados porque se complementan entre sí y permiten abordar el problema desde diferentes niveles de análisis.

El primer dataset permite estudiar cómo varía la señal WiFi CSI frente a objetos, posiciones y configuraciones físicas. El segundo dataset permite analizar señales relacionadas con actividad humana y generar variables estadísticas útiles para modelado. El tercer dataset aporta etiquetas de actividades humanas y archivos de captura que podrían ser procesados en fases futuras.

En conjunto, estas fuentes permiten construir una base inicial para una futura investigación sobre detección de presencia, movimiento, caídas o actividad no autorizada mediante señales WiFi CSI e inteligencia artificial. Esta línea puede aplicarse como apoyo a controles de seguridad física y ciberseguridad en áreas restringidas o espacios críticos.

## Estructura esperada de datos

Para ejecutar correctamente el notebook, los archivos deben ubicarse de la siguiente manera:

```text
data/
├── postgres/
│   └── raw_data.csv
├── json/
│   └── data_apml_min.json
└── har_ort/
    └── annotations.csv
```

Los archivos `.pcap` del dataset HAR-ORT no son necesarios para la ejecución principal del notebook, ya que en esta fase solo se utiliza el archivo `annotations.csv`.

## Archivos principales del proyecto

```text
desarrolloGrupo10.ipynb
docker-compose.yml
.env
data/README.md
evidencias/
```

## Tecnologías utilizadas

* Python
* Pandas
* SQLAlchemy
* PostgreSQL
* Docker
* Jupyter Notebook
* Visual Studio Code

## Ejecución general

1. Descargar los datasets desde los enlaces indicados.
2. Ubicar los archivos en la estructura de carpetas esperada.
3. Levantar PostgreSQL con Docker:

```bash
docker compose up -d
```

4. Verificar el contenedor:

```bash
docker ps
```

5. Ejecutar el notebook:

```text
desarrolloGrupo10.ipynb
```

## Nota sobre los datos

Los datasets no se cargan en este repositorio debido a su tamaño. En su lugar, se documentan los enlaces oficiales de descarga y la estructura esperada para reproducir el análisis localmente.
