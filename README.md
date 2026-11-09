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
| Última actualización | 2026-11-09 |
| Licencia | CC BY 4.0 |
| Norma de metadatos | NTM-INEGI 2024 (2.0) |

Del 2026-04-19 al 2026-11-08: 192 de 204 días con datos (94.1 %) y 17947 de 19584 registros esperados (91.6 %).

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
