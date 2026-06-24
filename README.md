# Proyecto ETL WiFi CSI - Detección de presencia y actividad humana

Este proyecto corresponde al componente práctico del curso de Inteligencia de Negocios. El trabajo se enfoca en la integración, transformación, modelado dimensional y preparación de carga de datos relacionados con señales WiFi CSI, con orientación investigativa hacia la detección de presencia, objetos, movimiento y actividad humana.

El proyecto utiliza fuentes heterogéneas provenientes de datasets públicos de Kaggle y construye un flujo ETL que permite consolidar los datos, generar una tabla contenedora, definir una taxonomía de movimiento, crear un modelo estrella, producir visualizaciones y preparar archivos finales para carga en AWS S3 y Amazon Redshift.

## Objetivo general

Construir un flujo ETL analítico a partir de datasets WiFi CSI, integrando fuentes heterogéneas para generar una base estructurada que permita analizar presencia, movimiento, objetos y actividad humana, con posible aplicación futura en seguridad física, ciberseguridad e inteligencia artificial.

## Objetivos específicos

* Inventariar y analizar fuentes de datos WiFi CSI.
* Consolidar los datasets en una tabla contenedora común.
* Normalizar etiquetas de actividad y definir una taxonomía de movimiento.
* Construir un modelo estrella con dimensiones y tabla de hechos.
* Generar visualizaciones para interpretar el corpus de datos.
* Exportar archivos CSV finales para carga en AWS S3 y Redshift.
* Documentar el proceso mediante Markdown dentro del notebook.

## Datasets utilizados

Los datasets no se incluyen directamente en el repositorio debido a su tamaño. Para ejecutar el notebook deben descargarse manualmente desde Kaggle y ubicarse en la estructura indicada.

### 1. WiFi CSI Object Detection

Link:
https://www.kaggle.com/datasets/armandokeller/wifi-csi-object-detection

Archivo utilizado:

```text
data/postgres/raw_data.csv
```

Este dataset contiene mediciones estructuradas de señales WiFi CSI, principalmente variables de amplitud y fase, junto con información de tipo de objeto, posición y configuración. Se utiliza para analizar cómo varía la señal inalámbrica ante objetos o cambios físicos en el entorno.

### 2. WiFi CSI Human Activity Recognition

Link:
https://www.kaggle.com/datasets/alanwake12/wifi-csi-human-activity-recognition

Archivo utilizado:

```text
data/json/data_apml_min.json
```

Este dataset está orientado al reconocimiento de actividad humana mediante señales WiFi CSI. A partir del archivo JSON se generan variables estadísticas como promedio, mínimo, máximo, desviación estándar y rango de la señal.

### 3. HAR-ORT 5GHz 80MHz Nexmon Extracted CSI

Link:
https://www.kaggle.com/datasets/brunobellizzi/har-ort-5ghz-80mhz-nexmon-extracted-csi?resource=download

Archivo utilizado:

```text
data/har_ort/annotations.csv
```

Este dataset aporta anotaciones de actividades humanas asociadas a archivos PCAP. En esta práctica se utiliza el archivo `annotations.csv`, que contiene actividad, usuario y nombre de archivo de captura.

## Justificación de los datasets

Los tres datasets fueron seleccionados porque se complementan entre sí. El primero permite analizar señales CSI frente a objetos, posiciones y configuraciones. El segundo aporta muestras CSI orientadas al reconocimiento de actividad humana. El tercero proporciona etiquetas de actividades humanas asociadas a archivos de captura.

En conjunto, estas fuentes permiten construir una base analítica para una futura investigación sobre detección de presencia, movimiento, caídas o actividad no autorizada mediante redes WiFi CSI e inteligencia artificial.

## Estructura esperada del proyecto

```text
proyectoWifi/
├── data/
│   ├── postgres/
│   │   └── raw_data.csv
│   ├── json/
│   │   └── data_apml_min.json
│   ├── har_ort/
│   │   └── annotations.csv
│   └── processed/
│       ├── analysis/
│       ├── figures/
│       ├── star_schema/
│       └── aws_redshift_export/
├── evidencias/
├── ProyectoG10.ipynb
├── configuracionesAWS_G10.docx
├── docker-compose.yml
├── requirements.txt
├── .env
└── README.md
```

## Tecnologías utilizadas

* Python
* Pandas
* NumPy
* Matplotlib
* PostgreSQL
* Docker
* SQLAlchemy
* Jupyter Notebook / PyCharm
* Amazon S3
* Amazon Redshift Serverless
* Boto3
* Redshift Connector

## Flujo general del proyecto

### Fase 1. Inventario de fuentes

Se identifican las fuentes disponibles, rutas, columnas, cantidad de registros y utilidad investigativa de cada dataset.

### Fase 2. Tabla contenedora consolidada

Se integran las fuentes en una tabla común llamada:

```text
df_wifi_csi_contenedora
```

Esta tabla conserva la trazabilidad de cada registro según fuente, actividad, usuario, archivo, estado de etiqueta y utilidad investigativa.

### Fase 3. Análisis exploratorio

Se analiza la composición del corpus, distribución por fuente, actividades, usuarios, etiquetas, archivos y valores nulos.

### Fase 4. Taxonomía de movimiento

Se normalizan las actividades originales en categorías analíticas:

```text
no_motion
low_motion
motion
critical_motion
object_presence
unlabeled_har_sample
```

Esto permite diferenciar registros útiles para detección de movimiento humano, detección de objetos y muestras sin etiqueta.

### Fase 5. Modelo estrella

Se construye un modelo dimensional compuesto por dimensiones y tabla de hechos.

Dimensiones generadas:

```text
dim_source
dim_activity
dim_user
dim_file
dim_object
dim_position
dim_configuration
dim_time
dim_label_quality
```

Tabla de hechos:

```text
fact_wifi_csi_capture
```

### Fase 6. Visualizaciones

Se generan visualizaciones para interpretar:

* registros por fuente de datos;
* actividades humanas etiquetadas;
* relación usuario vs actividad;
* distribución por etiqueta investigativa.

Las figuras se almacenan en:

```text
data/processed/figures/
```

### Fase 7. Exportación final

Se exportan los CSV finales del modelo estrella para carga en AWS/Redshift.

Ruta de salida:

```text
data/processed/aws_redshift_export/
```

Archivos principales generados:

```text
dim_source.csv
dim_activity.csv
dim_user.csv
dim_file.csv
dim_object.csv
dim_position.csv
dim_configuration.csv
dim_time.csv
dim_label_quality.csv
fact_wifi_csi_capture.csv
```

### Fase 8. Preparación AWS

Se documenta la configuración de Amazon S3, Amazon Redshift Serverless, grupo de seguridad, carga de archivos CSV y evidencias visuales en el archivo:

```text
configuracionesAWS_G10.docx
```

## Resultados principales

El corpus consolidado contiene registros provenientes de tres fuentes:

```text
wifi_csi_object_detection
wifi_csi_human_activity_recognition
har_ort_annotations
```

El subconjunto más relevante para detección de movimiento humano corresponde a las actividades etiquetadas:

```text
quiet
sit
stand
walk
fall
```

Estas clases permiten preparar una futura etapa de inteligencia artificial orientada a clasificación de movimiento humano mediante señales WiFi CSI.

## Ejecución del proyecto

1. Clonar el repositorio.

```bash
git clone <url-del-repositorio>
cd proyectoWifi
```

2. Crear y activar entorno virtual.

```bash
python -m venv venv
.\venv\Scripts\activate
```

3. Instalar dependencias.

```bash
pip install -r requirements.txt
```

4. Descargar los datasets desde Kaggle y ubicarlos en la estructura indicada.

5. Levantar PostgreSQL con Docker.

```bash
docker compose up -d
```

6. Verificar el contenedor.

```bash
docker ps
```

7. Ejecutar el notebook principal.

```text
ProyectoG10.ipynb
```

## Variables de entorno

El archivo `.env` debe contener las credenciales necesarias para PostgreSQL y, si se realiza la carga en la nube, para AWS y Redshift.

Ejemplo general:

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=wifi_csi
DB_USER=postgres
DB_PASSWORD=postgres

AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_DEFAULT_REGION=us-east-1

S3_BUCKET_NAME=your_bucket
S3_PREFIX=wifi-csi-g10/star_schema

REDSHIFT_HOST=your_redshift_endpoint
REDSHIFT_PORT=5439
REDSHIFT_DATABASE=dev
REDSHIFT_USER=admin
REDSHIFT_PASSWORD=your_password
REDSHIFT_IAM_ROLE=your_redshift_iam_role
```

## Nota de seguridad

El archivo `.env` no debe subirse al repositorio, ya que puede contener credenciales de base de datos, AWS o Redshift.

Se recomienda mantener en `.gitignore`:

```text
.env
venv/
data/raw/
*.zip
*.pcap
__pycache__/
.ipynb_checkpoints/
```

## Entregables de la Semana 3

Los archivos finales del entregable son:

```text
ProyectoG10.ipynb
configuracionesAWS_G10.docx
```

El notebook contiene todo el proceso técnico comentado en Markdown. El documento Word contiene las capturas de configuración, evidencias de AWS, S3, Redshift y figuras generadas.

## Aplicación futura

Este proyecto deja una base estructurada para continuar con una investigación sobre detección de movimiento humano mediante redes WiFi CSI. En futuras fases se podrían procesar archivos PCAP, extraer características avanzadas de amplitud y fase, construir ventanas temporales de señal y entrenar modelos de aprendizaje automático para clasificar actividades como reposo, movimiento normal o caídas.

## Estado actual

El proyecto se encuentra actualizado hasta la Semana 3, incluyendo:

* inventario de fuentes;
* tabla contenedora consolidada;
* análisis exploratorio;
* taxonomía de movimiento;
* modelo estrella;
* visualizaciones;
* exportación de CSV finales;
* preparación para carga en AWS/S3/Redshift.
