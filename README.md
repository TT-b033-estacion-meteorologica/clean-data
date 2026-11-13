# Datos Meteorológicos - Estación ESCOM IPN

Registros meteorológicos automáticos capturados cada 15 minutos por la estación
ubicada en ESCOM, IPN, Ciudad de México, y publicados diariamente.

## Información general

| Campo | Valor |
|---|---|
| Institución | Escuela Superior de Cómputo - IPN |
| Ubicación | Ciudad de México (19.5046, -99.1467) |
| Altitud | ≈2,240 msnm |
| Resolución temporal | cada 15 minutos |
| Registros esperados por día | 96 (la mayoría de los días completos tiene 95) |
| Datos disponibles desde | 2026-04-19 |
| Última actualización | 2026-11-13 |
| Licencia | CC BY 4.0 |
| Norma de metadatos | NTM-INEGI 2024 (2.0) |

## Completitud de los datos

Del 2026-04-19 al 2026-11-12: 196 de 208 días con datos (94.2 %) y 18330 de 19968 registros esperados (91.8 %).

## Estructura del repositorio

```
clean-data/
├── metadata.yml        <- metadatos NTM-INEGI legibles por máquina
├── README.md           <- esta descripción
└── AAAA/
    └── MM/
        ├── clean_weather_AAAAMM.csv
        ├── clean_temperatura_AAAAMM.csv
        ├── clean_viento_AAAAMM.csv
        ├── clean_lluvia_AAAAMM.csv
        └── clean_presion_AAAAMM.csv
```

## Diccionario de datos

La siguiente tabla describe las variables que conforman el conjunto de datos limpio (`clean_weather`). Se incluye el origen de cada campo para facilitar la trazabilidad desde la información proporcionada por la estación meteorológica hasta su publicación.

| Campo | Tipo de dato | Descripción | Unidad | Origen |
|---|---|---|---|---|
| `id` | INTEGER | Identificador único del registro. | — | Generado por PostgreSQL durante la ingesta. |
| `fecha` | DATE | Fecha en que se realizó la medición. | YYYY-MM-DD | `date_raw`, convertida mediante `TO_DATE()`. |
| `hora` | TIME | Hora en que se realizó la medición. | HH:MM:SS | `time_raw`. |
| `temp_out` | NUMERIC | Temperatura exterior. | °C | `temp_out` |
| `hi_temp` | NUMERIC | Temperatura máxima exterior del intervalo. | °C | `hi_temp` |
| `low_temp` | NUMERIC | Temperatura mínima exterior del intervalo. | °C | `low_temp` |
| `in_temp` | NUMERIC | Temperatura interior. | °C | `in_temp` |
| `in_hum_pct` | NUMERIC | Humedad relativa interior. | % | `in_hum` |
| `out_hum_pct` | NUMERIC | Humedad relativa exterior. | % | `out_hum` |
| `rain` | NUMERIC | Precipitación acumulada registrada. | mm | `rain` |
| `rain_rate` | NUMERIC | Intensidad de precipitación. | mm/h | `rain_rate` |
| `bar` | NUMERIC | Presión barométrica. Incluye el ajuste correspondiente cuando aplica. | mmHg | `bar` y tabla `ajustes_barometro`. |
| `wind_speed` | NUMERIC | Velocidad del viento. | km/h | `wind_speed` |
| `hi_speed` | NUMERIC | Velocidad máxima del viento o ráfaga. | km/h | `hi_speed` |
| `hi_dir` | VARCHAR | Dirección correspondiente a la ráfaga máxima. | Cardinal | `hi_dir` |
| `wind_dir` | VARCHAR | Dirección predominante del viento. | Cardinal | `wind_dir` |
| `temp_units` | VARCHAR | Unidad de temperatura declarada por la consola. | — | `temp_units` |
| `bar_units` | VARCHAR | Unidad de presión declarada por la consola. | — | `bar_units` |
| `wind_units` | VARCHAR | Unidad de velocidad del viento declarada por la consola. | — | `wind_units` |
| `rain_units` | VARCHAR | Unidad de precipitación declarada por la consola. | — | `rain_units` |
| `bar_ajuste_mmhg` | NUMERIC | Ajuste barométrico aplicado al registro. Si no corresponde una corrección, su valor es 0. | mmHg | Tabla `ajustes_barometro`. |

**Nota:** Los valores faltantes o inválidos se representan como `NULL`. Las variables `solar_rad`, `uv_index`, `et`, `hi_solar_rad` y `hi_uv` no se incluyen en el conjunto limpio porque la estación no cuenta con los sensores necesarios para medirlas.

## Variables por archivo

Los datos limpios se distribuyen en 5 archivos CSV mensuales. Cada archivo contiene las variables correspondientes a una vista temática, lo que facilita su consulta y reutilización.

| Archivo | Variables |
|---|---|
| `clean_weather_AAAAMM.csv` | `id`, `fecha`, `hora`, `temp_out`, `hi_temp`, `low_temp`, `in_temp`, `in_hum_pct`, `out_hum_pct`, `rain`, `rain_rate`, `bar`, `wind_speed`, `hi_speed`, `hi_dir`, `wind_dir`, `temp_units`, `bar_units`, `wind_units`, `rain_units`, `bar_ajuste_mmhg` |
| `clean_temperatura_AAAAMM.csv` | `id`, `fecha`, `hora`, `temp_exterior`, `temp_max`, `temp_min`, `temp_interior`, `humedad_exterior`, `humedad_interior` |
| `clean_viento_AAAAMM.csv` | `id`, `fecha`, `hora`, `velocidad_kmh`, `rafaga_kmh`, `direccion`, `direccion_rafaga` |
| `clean_lluvia_AAAAMM.csv` | `id`, `fecha`, `hora`, `lluvia_mm`, `tasa_mm_hr` |
| `clean_presion_AAAAMM.csv` | `id`, `fecha`, `hora`, `presion_hpa`, `variacion_hpa`, `tendencia`, `ajuste_mmhg` |

**Consideraciones sobre las variables:**

- `AAAAMM` representa el año y mes del conjunto de datos. Por ejemplo, `202611` corresponde a noviembre de 2026.
- Los archivos mensuales acumulan los registros disponibles del mes y se actualizan diariamente.
- Las variables de presión se expresan en mmHg. No deben interpretarse como hPa, aunque en `clean_presion` las columnas se llamen `presion_hpa` y `variacion_hpa`.
- En `clean_presion`, `variacion_hpa` representa la variación de presión que reporta la consola y `tendencia` clasifica su comportamiento (`Creciente`, `Descendente` o `Estable`); `ajuste_mmhg` es el ajuste barométrico aplicado al registro.

## Ejemplo de consumo del repositorio

El repositorio permite descargar y utilizar los datos meteorológicos mediante archivos CSV, sin necesidad de conectarse directamente a la infraestructura de almacenamiento del proyecto.

**Repositorio:** [Datos Meteorológicos Limpios - GitHub](https://github.com/TT-b033-estacion-meteorologica/clean-data)

Los archivos se encuentran organizados por año y mes. Por ejemplo, para consultar los datos meteorológicos generales de noviembre de 2026, se puede utilizar la siguiente ruta:

`2026/11/clean_weather_202611.csv`

### Consumo mediante Python

Se puede utilizar la biblioteca `pandas` para descargar y analizar directamente los archivos CSV publicados en GitHub.

```python
import pandas as pd

# URL del archivo CSV publicado en GitHub
url = (
    "https://raw.githubusercontent.com/"
    "TT-b033-estacion-meteorologica/clean-data/"
    "main/2026/11/clean_weather_202611.csv"
)

# Lectura del conjunto de datos
df = pd.read_csv(url)

# Conversión de las columnas temporales
df["fecha"] = pd.to_datetime(df["fecha"])
df["hora"] = pd.to_datetime(
    df["hora"], format="%H:%M:%S"
).dt.time

# Consulta de los primeros registros
print(df.head())

# Ejemplo: temperatura exterior promedio por día
temperatura_diaria = (
    df.groupby("fecha")["temp_out"]
    .mean()
    .reset_index()
)

print(temperatura_diaria)
```

### Consumo mediante Excel o Power BI

Los archivos también pueden utilizarse directamente desde herramientas de análisis:

1. Ingresar al repositorio público de GitHub.
2. Navegar hasta la carpeta del año y mes que se desea consultar.
3. Seleccionar el archivo CSV de interés.
4. Descargar el archivo o copiar su dirección de descarga.
5. En Excel o Power BI, seleccionar **Obtener datos → Texto/CSV** o **Desde Web**, según corresponda.
6. Seleccionar las columnas que se utilizarán para el análisis.

Al utilizar archivos mensuales, se puede consultar el histórico de cada periodo sin tener que combinar manualmente múltiples archivos diarios.

## Metodología de limpieza

El conjunto de datos limpio se genera mediante un proceso automatizado de extracción, carga y transformación (ELT). La información original se conserva en su estado de recepción, mientras que las transformaciones se aplican mediante vistas SQL en PostgreSQL.

Este procedimiento permite mantener la trazabilidad de los datos y proporcionar archivos estructurados para su análisis y reutilización.

### 1. Conservación de los datos originales

Los registros obtenidos de la estación meteorológica se almacenan inicialmente en la tabla `raw_weather_data` de PostgreSQL.

En esta etapa:

- Los campos originales se almacenan principalmente como cadenas de texto (`VARCHAR`).
- Se conservan los valores tal como fueron recibidos por la estación.
- Los valores faltantes, como `---`, no se modifican.
- Se conserva la información necesaria para rastrear el origen de los registros.

Esta capa permite mantener una referencia de los datos originales y facilita su auditoría o reprocesamiento.

### 2. Conversión y normalización de datos

La vista `vw_clean_weather` aplica las transformaciones necesarias para obtener registros estructurados.

Las principales operaciones son:

- **Conversión de fechas:** el campo `date_raw`, recibido en formato `DD:MM:YYYY`, se convierte al tipo `DATE` mediante la función `TO_DATE()`.
- **Conversión de horas:** el campo `time_raw` se tipifica como `TIME`.
- **Tratamiento de valores faltantes:** se utiliza `NULLIF()` para convertir el marcador `---` en `NULL` antes de realizar conversiones numéricas.
- **Conversión de tipos:** las variables meteorológicas numéricas se convierten al tipo `NUMERIC`, mientras que las direcciones y unidades permanecen como cadenas de texto.
- **Normalización de nombres:** los campos `date_raw` y `time_raw` se renombran como `fecha` y `hora`; asimismo, `in_hum` y `out_hum` se convierten en `in_hum_pct` y `out_hum_pct`, respectivamente.
- **Depuración de variables:** se excluyen los campos relacionados con radiación solar, índice UV y evapotranspiración, debido a que la estación no dispone de los sensores necesarios para obtener dichas mediciones.

Los valores faltantes se conservan como ausentes y no se sustituyen mediante interpolación.

### 3. Validación y control de calidad

Durante el proceso se contemplan mecanismos para identificar inconsistencias en los registros y posibles anomalías en las mediciones.

Entre las verificaciones implementadas se encuentran:

- Revisión de formatos y tipos de datos.
- Identificación de valores faltantes.
- Prevención de registros duplicados mediante una restricción de unicidad sobre fecha y hora durante la ingesta.
- Revisión de rangos físicos esperados para las variables meteorológicas.
- Detección de cambios abruptos en la presión barométrica.

Las anomalías detectadas durante la ingesta generan alertas para facilitar su identificación y revisión. Estas alertas no implican que el registro original sea modificado automáticamente.

### 4. Corrección de la presión barométrica

Debido a una configuración incorrecta del barómetro, se identificó una discontinuidad en las mediciones de presión a partir del 3 de julio de 2026.

Para corregir el historial se implementó una tabla de ajustes denominada `ajustes_barometro`, que permite identificar el intervalo afectado y el valor de corrección correspondiente.

El procedimiento consiste en:

- Conservar las lecturas originales sin modificarlas en la capa RAW.
- Aplicar un ajuste de **+192.1 mmHg** a los registros comprendidos entre el 3 de julio de 2026 a las 00:00 h y el 20 de octubre de 2026 a las 09:15 h.
- Incorporar el ajuste aplicado en la columna `bar_ajuste_mmhg` (`clean_weather`) y `ajuste_mmhg` (`clean_presion`).
- Utilizar la presión corregida para generar los datos publicados en CLEAN.
- Recalcular la variación y tendencia barométrica para evitar que la corrección genere un cambio artificial en la serie temporal.

Los registros posteriores al 20 de octubre de 2026 a las 09:15 h no requieren este ajuste.

### 5. Organización de vistas temáticas

Una vez aplicada la limpieza general, se generan vistas especializadas en PostgreSQL para separar las variables según su naturaleza meteorológica:

| Vista | Contenido |
|---|---|
| `vw_clean_weather` | Conjunto general de variables meteorológicas limpias. |
| `vw_temperatura` | Temperatura y humedad interior y exterior. |
| `vw_viento` | Velocidad, ráfagas y dirección del viento. |
| `vw_lluvia` | Precipitación acumulada e intensidad de lluvia. |
| `vw_presion` | Presión barométrica, variación y tendencia. |

Estas vistas facilitan la consulta de información específica y sirven como fuente para la generación de los archivos CSV mensuales.

### 6. Publicación de los datos limpios

Finalmente, un proceso automatizado consulta las vistas temáticas y genera los archivos CSV correspondientes.

La publicación se realiza diariamente a través de Google Cloud Run, que obtiene los registros del mes en curso y actualiza los archivos correspondientes en el repositorio público de GitHub. Durante los primeros siete días de cada mes también se regenera el archivo del mes anterior, para incorporar los datos que se hayan recuperado en ese lapso.

De esta manera, los archivos CLEAN mantienen un histórico mensual acumulado y están disponibles para su consulta, descarga y reutilización.

**Nota:** La limpieza no reconstruye mediciones que nunca fueron capturadas por la estación. Los días sin registros, las interrupciones de comunicación y las limitaciones físicas de los sensores permanecen documentados como parte de las limitaciones del conjunto de datos.

## Limitaciones conocidas

- Presión barométrica corregida: por una configuración incorrecta del barómetro (BARCAL) hubo una discontinuidad desde el 3 de julio de 2026. En CLEAN se aplicó un ajuste constante de +192.1 mmHg entre el 3 de julio de 2026 a las 00:00 h y el 20 de octubre de 2026 a las 09:15 h; el valor aplicado se publica en bar_ajuste_mmhg (clean_weather) y ajuste_mmhg (clean_presion), y las lecturas originales se conservan sin modificar en el repositorio raw-data.
- La presión se expresa en mmHg, la unidad configurada en la consola. En clean_presion las columnas presion_hpa y variacion_hpa también están en mmHg, no en hPa.
- Temperatura y humedad relativa se reportan sin decimales (resolución efectiva de 1 °C y 1 %).
- El registro de las 00:00 h de cada día no se captura, por lo que la mayoría de los días completos tiene 95 registros en lugar de 96.
- Hay días sin datos que ya no pueden recuperarse: 31 de mayo; 19, 23, 24 y 29 de junio; 3, 18 y 19 de julio; 11 de agosto, y 7, 15 y 16 de septiembre de 2026. Los días de la puesta en marcha (24 de abril al 2 de mayo de 2026) y algunos días aislados están incompletos.
- Viento: cerca de dos tercios de los intervalos registran 0 km/h y alrededor de un tercio no tiene dirección (calma); el anemómetro podría estar resguardado, por lo que los datos reflejan la circulación local.
- Radiación solar, índice UV y evapotranspiración no están disponibles (sensores no instalados) y no se incluyen en las vistas limpias.
- Los valores atípicos no se marcan ni se eliminan en los datos publicados. En cada captura, el sistema revisa los registros nuevos (rangos físicos, saltos de presión y sensores sin lectura) y envía una alerta al equipo para revisarlos.

## Estándares aplicados

- **NTM-INEGI 2024 (2.0)**: Norma Técnica para la Elaboración de Metadatos Geográficos.
- **ISO 19115-1:2014**: estándar internacional en el que se basa la NTM.
- **WIS 2.0 (OMM)**: recomendaciones para la publicación de datos meteorológicos.

## Licencia

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.es): puedes usar,
compartir y adaptar estos datos citando la fuente:

> Estación Meteorológica ESCOM IPN - https://github.com/TT-b033-estacion-meteorologica
