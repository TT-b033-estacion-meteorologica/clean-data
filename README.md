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
| Última actualización | 2026-10-29 |
| Licencia | CC BY 4.0 |
| Norma de metadatos | NTM-INEGI 2024 (2.0) |

Del 2026-04-19 al 2026-10-28: 181 de 193 días con datos (93.8 %) y 16903 de 18528 registros esperados (91.2 %).

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

- Presión barométrica no homogénea: hasta el 2 de julio de 2026 la consola reportó presión reducida al nivel del mar (≈762–774 mmHg) y desde el 4 de julio presión absoluta de la estación (≈572–582 mmHg); el 3 de julio no hay datos. No se ha aplicado ninguna corrección: analizar los dos periodos por separado.
- La columna presion_hpa (y variacion_hpa) está en mmHg, la unidad configurada en la consola, no en hPa.
- Temperatura y humedad relativa se reportan sin decimales (resolución efectiva de 1 °C y 1 %).
- El registro de las 00:00 h de cada día no se captura, por lo que la mayoría de los días completos tiene 95 registros en lugar de 96.
- Hay días sin datos o incompletos: la puesta en marcha (24 de abril al 2 de mayo de 2026) y los días 7, 15 y 16 de septiembre de 2026, que no pudieron recuperarse.
- Viento: el 65 % de los intervalos registra 0 km/h y el 31 % no tiene dirección (calma); el anemómetro podría estar resguardado, por lo que los datos reflejan la circulación local.
- Dirección del viento: en parte de los registros las columnas direccion y direccion_rafaga están intercambiadas (error de ingesta identificado y pendiente de corrección).
- Radiación solar, índice UV y evapotranspiración no están disponibles (sensores no instalados) y no se incluyen en las vistas limpias.
- Aún no se aplica en la base de datos la verificación de rangos físicos ni el marcado de valores atípicos; una verificación externa al 18 de octubre de 2026 no encontró valores fuera de rango en temperatura, humedad, precipitación ni viento.

## Estándares aplicados

- **NTM-INEGI 2024 (2.0)**: Norma Técnica para la Elaboración de Metadatos Geográficos.
- **ISO 19115-1:2014**: estándar internacional en el que se basa la NTM.
- **WIS 2.0 (OMM)**: recomendaciones para la publicación de datos meteorológicos.

## Licencia

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.es): puedes usar,
compartir y adaptar estos datos citando la fuente:

> Estación Meteorológica ESCOM IPN - https://github.com/Estacion-metereologica
