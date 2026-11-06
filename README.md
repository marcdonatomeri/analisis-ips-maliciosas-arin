# Análisis de IPs maliciosas en la zona ARIN (2013–2019)

Proyecto universitario de la asignatura SDA, desarrollado en MATLAB. Analiza cómo han evolucionado las direcciones IP maliciosas en la zona de ARIN (Norteamérica, el Caribe y parte del Atlántico Norte) entre 2013 y 2019, y las pone en contexto con indicadores tecnológicos e incidentes de seguridad reales.

Todo el trabajo (código, gráficas y conclusiones) está en el Live Script [`trabajo_limpio_2.mlx`](trabajo_limpio_2.mlx).

> GitHub no muestra el contenido de los archivos `.mlx`: hay que descargarlo y abrirlo con MATLAB. El archivo conserva las salidas y las 18 figuras de la última ejecución, así que se pueden consultar sin volver a ejecutarlo.

## Qué se analiza

1. **Exploración y filtrado.** Se aíslan los rangos IPv4 y los ASN activos del fichero de delegaciones de ARIN y se cruzan con los ficheros de reputación de 2013 y 2019, convirtiendo las IP a decimal para comprobar si caen dentro de cada rango.
2. **Regresión.** Se estudia si el nivel tecnológico de un país (indicadores de la ITU) explica su número de IPs maliciosas. Los códigos ISO2 e ISO3 se relacionan usando el shapefile como tabla de conversión.
3. **Mapas.** Se generan mapas coropléticos de las IPs maliciosas, con escala logarítmica para que el volumen de Estados Unidos no oculte al resto de países, y de la variable ITU con mejor ajuste.
4. **Incidentes reales.** Se analiza la base de datos VCDB (esquema VERIS) para ver tipos de ataque, actores, activos afectados, motivos e industrias.

## Resultados principales

| Indicador | 2013 | 2019 |
| --- | ---: | ---: |
| IPs en el fichero de reputación | 258.626 | 55.153 |
| IPs dentro de la zona ARIN | 56.589 | 7.643 |
| Porcentaje de IPs en ARIN | 21,9 % | 13,9 % |
| Porcentaje de las IPs de ARIN situadas en EE. UU. | 86,0 % | 79,4 % |

- **Persistencia:** solo 8 IPs de ARIN aparecen en los dos ficheros.
- **Regresión:** el R² es muy bajo en todas las variables ITU (entre 0,014 y 0,043), por lo que el desarrollo tecnológico de un país no explica por sí solo su número de IPs maliciosas. La variable con mejor ajuste es la banda ancha fija.
- **Infraestructura:** EE. UU. concentra 25.305 de los 27.450 registros de ASN de ARIN, un dato que hay que tener en cuenta al comparar países.
- **Incidentes (VCDB, 1.590 en la zona ARIN):** las acciones más frecuentes son las físicas (unos 510), los errores (unos 390) y el hacking (unos 385). Predominan los actores externos, y el motivo conocido más habitual es el económico. Los códigos de industria más afectados corresponden a hospitales, consultas médicas y universidades.

**Limitaciones:** los ficheros de 2013 y 2019 tienen orígenes y tamaños distintos, por lo que la caída en cifras absolutas no equivale necesariamente a una caída real de la actividad maliciosa. Además, todas las IPs de ARIN de 2019 figuran como `Malicious Host`, mientras que en 2013 hay varias categorías (84 % `Scanning Host`), así que el cambio de tipo de amenaza puede deberse en parte a cómo clasifica cada fichero.

## Cómo ejecutarlo

Necesitas **MATLAB** (el proyecto se guardó con R2026a) y **Mapping Toolbox**, que se usa para `shaperead`, `mapshow` y `makesymbolspec`.

Este repositorio **no incluye los datos ni las funciones auxiliares**, y el script no se ejecutará sin ellos. Deben estar en la misma carpeta que el `.mlx`, con esta estructura:

```text
├── trabajo_limpio_2.mlx
├── LoadArin.m                  # no incluido: lee el fichero delegated en una tabla
├── LeerRep.m                   # no incluido: lee los ficheros de reputación en una tabla
├── delegated_arin.txt
├── reputation_2013
├── reputation_2019
├── variables_2013/variables_2013.csv
├── variables_2019/variables_2019.csv
├── mapas/ne_110m_admin_0_countries.shp   # con sus archivos asociados
└── vcdb/                       # incidentes en formato JSON
```

Las dos funciones deben devolver estas columnas, que son las que usa el script:

- `LoadArin`: `pais`, `tipo`, `ip`, `valor` y `estado`.
- `LeerRep`: `IP`, `paises`, `ciudades` y `tipo`.

## Fuentes de datos

- [ARIN](https://www.arin.net/): delegaciones de IPs y ASN.
- [ITU](https://www.itu.int/): indicadores mundiales de telecomunicaciones.
- [VCDB](http://veriscommunity.net/vcdb.html): base de datos abierta de incidentes de seguridad.
- [Natural Earth](https://www.naturalearthdata.com/): cartografía (`ne_110m_admin_0_countries`).
- Ficheros de reputación: el de 2013 procede del libro de la asignatura y el de 2019 se usó en clase.

## Autores

| Autores | Parte |
| --- | --- |
| Abel Castillo González y Marc Donato Merí | Exploración, limpieza de datos y cruce de reputación con ARIN |
| Iván Martínez Castellot y Andrés Nadal Manzanares | Regresión, datos ITU y cruce de códigos ISO2/ISO3 |
| Ruben Saiz López y Rubén Vilanova Candel | Mapas coropléticos y escalas logarítmicas |
| David Maudos Soriano | Análisis de incidentes VCDB y conclusiones globales |
