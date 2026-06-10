New-Item -ItemType Directory -Force data

@"
# Carpeta de datos

Los datasets utilizados en la práctica no se incluyen en el repositorio debido a su tamaño.

Para ejecutar el notebook localmente, colocar los archivos en la siguiente estructura:

data/postgres/raw_data.csv
data/json/data_apml_min.json
data/har_ort/annotations.csv

Los archivos PCAP se consideran datos crudos y no son necesarios para la ejecución principal del notebook.
"@ | Out-File -Encoding utf8 data/README.md