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
| Última actualización | 2026-11-08 |
| Licencia | CC BY 4.0 |
| Norma de metadatos | NTM-INEGI 2024 (2.0) |

Del 2026-04-19 al 2026-11-07: 191 de 203 días con datos (94.1 %) y 17851 de 19488 registros esperados (91.6 %).

## Diccionario de datos – CLEAN_DATA

El conjunto de datos `CLEAN_DATA` contiene registros meteorológicos transformados y limpios para su consumo. Cada registro representa una medición correspondiente a una fecha y hora determinadas.

A continuación, se describen las variables que conforman el conjunto de datos:

### Descripción de variables

| Variable | Nombre descriptivo | Tipo de dato | Unidad | Descripción |
|---|---|---|---|---|
| `id` | Identificador del registro | Integer | — | Identificador único asignado a cada registro meteorológico. |
| `fecha` | Fecha de registro | Date | YYYY-MM-DD | Fecha en la que se realizó la medición meteorológica. |
| `hora` | Hora de registro | Time | HH:MM:SS | Hora correspondiente a la captura del registro. |
| `temp_out` | Temperatura exterior | Float | °C | Temperatura del aire exterior registrada por la estación. |
| `hi_temp` | Temperatura máxima | Float | °C | Temperatura máxima exterior registrada durante el intervalo correspondiente. |
| `low_temp` | Temperatura mínima | Float | °C | Temperatura mínima exterior registrada durante el intervalo correspondiente. |
| `in_temp` | Temperatura interior | Float | °C | Temperatura registrada en el ambiente interior. |
| `in_hum_pct` | Humedad interior | Float | % | Porcentaje de humedad relativa registrado en el interior. |
| `out_hum_pct` | Humedad exterior | Float | % | Porcentaje de humedad relativa del aire exterior. |
| `rain` | Precipitación acumulada | Float | mm | Cantidad de precipitación acumulada registrada por el pluviómetro. |
| `rain_rate` | Intensidad de precipitación | Float | mm/h | Tasa de precipitación registrada por unidad de tiempo. |
| `bar` | Presión atmosférica | Float | Según configuración | Presión barométrica registrada por la estación meteorológica. |
| `wind_speed` | Velocidad del viento | Float | km/h | Velocidad del viento registrada durante el intervalo de medición. |
| `hi_speed` | Velocidad máxima del viento | Float | km/h | Velocidad máxima del viento registrada durante el intervalo correspondiente. |
| `hi_dir` | Dirección del viento máximo | String | — | Dirección cardinal desde la que provino el viento de mayor velocidad. |
| `wind_dir` | Dirección del viento | String | — | Dirección predominante del viento durante el intervalo de medición. |
| `temp_units` | Unidad de temperatura | String | — | Identificador de la unidad utilizada para las variables de temperatura. |
| `bar_units` | Unidad de presión | String | — | Identificador de la unidad utilizada para la presión atmosférica. |
| `wind_units` | Unidad de velocidad del viento | String | — | Identificador de la unidad utilizada para la velocidad del viento. |
| `rain_units` | Unidad de precipitación | String | — | Identificador de la unidad utilizada para las mediciones de precipitación. |
| `bar_ajuste_mmhg` | Ajuste barométrico | Float | mmHg | Valor de ajuste barométrico expresado en milímetros de mercurio. |

### Consideraciones sobre los datos

- **Identificación:** cada registro cuenta con un identificador único (`id`).
- **Temporalidad:** la fecha y hora permiten identificar el momento de cada observación meteorológica.
- **Temperatura:** las variables de temperatura se expresan en grados Celsius (°C), según la configuración del conjunto de datos.
- **Humedad:** las mediciones de humedad relativa se expresan en porcentaje (%).
- **Precipitación:** las variables de precipitación se expresan en milímetros (mm).
- **Viento:** las velocidades se expresan en kilómetros por hora (km/h), mientras que las direcciones se representan mediante abreviaturas cardinales.
- **Presión atmosférica:** la unidad debe interpretarse de acuerdo con la configuración y transformación aplicada a los datos originales.

## Estructura del repositorio

```
clean_data/
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

## Variables por archivo

| Archivo | Variables | Unidades |
|---|---|---|
| clean_weather | Todas las variables limpias | Varias |
| clean_temperatura | temp_exterior, temp_max, temp_min, temp_interior, humedad_exterior, humedad_interior | °C, % |
| clean_viento | velocidad_kmh, rafaga_kmh, direccion, direccion_rafaga | km/h |
| clean_lluvia | lluvia_mm, tasa_mm_hr | mm, mm/h |
| clean_presion | presion_hpa (en realidad mmHg), variacion_hpa, tendencia | mmHg |

## Limitaciones conocidas

- Presión barométrica: del 3 de julio al 20 de octubre de 2026 la consola tuvo una calibración errónea del barómetro (BARCAL) y reportó la presión 192.1 mmHg por debajo de su valor (≈572–582 mmHg). En clean_weather y clean_presion ese periodo ya está corregido: la columna bar_ajuste_mmhg (ajuste_mmhg en clean_presion) indica el ajuste sumado a cada registro. El repositorio raw-data conserva las lecturas originales de la consola.
- La columna presion_hpa (y variacion_hpa) está en mmHg, la unidad configurada en la consola, no en hPa.
- Temperatura y humedad relativa se reportan sin decimales (resolución efectiva de 1 °C y 1 %).
- El registro de las 00:00 h de cada día no se captura, por lo que la mayoría de los días completos tiene 95 registros en lugar de 96.
- Hay días sin datos que ya no pueden recuperarse: 31 de mayo; 19, 23, 24 y 29 de junio; 3, 18 y 19 de julio; 11 de agosto, y 7, 15 y 16 de septiembre de 2026. Los días de la puesta en marcha (24 de abril al 2 de mayo de 2026) y algunos días aislados están incompletos.
- Viento: cerca de dos tercios de los intervalos registran 0 km/h y alrededor de un tercio no tiene dirección (calma).
- Radiación solar, índice UV y evapotranspiración no están disponibles (sensores no instalados) y no se incluyen en las vistas limpias.

## Estándares aplicados

- **Lineamiento en Materia de Datos Abiertos DOF: 11/09/2025**: Lineamientos en materia de Datos Abiertos publicados en México.
- **NTM-INEGI 2024 (2.0)**: Norma Técnica para la Elaboación de Metadatos Geográficos.
- **ISO 19115-1:2014**: estándar internacional en el que se basa la NTM.
- **WIS 2.0 (OMM)**: recomendaciones para la publicación de datos meteorológicos.

## Licencia

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.es): puedes usar,
compartir y adaptar estos datos citando la fuente:

> Estación Meteorológica ESCOM IPN - https://github.com/TT-b033-estacion-meteorologica
