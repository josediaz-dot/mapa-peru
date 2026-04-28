# Tutorial: Cómo elaborar un mapa del Perú con R

## Introducción

Un mapa coroplético es una herramienta que permite representar datos estadísticos mediante colores en distintas regiones geográficas. En este tutorial se explica cómo elaborar un mapa del Perú usando R, integrando datos y visualización geográfica.

------------------------------------------------------------------------

## Contexto: resultados electorales

Los resultados electorales a nivel nacional permiten conocer la distribución general de votos por partido político. Sin embargo, este tipo de datos no muestra cómo varían los resultados entre regiones del país.

------------------------------------------------------------------------

## Problema

Los datos nacionales no permiten identificar diferencias geográficas dentro del Perú. Para ello, es necesario trabajar con información desagregada por departamentos.

------------------------------------------------------------------------

## Solución

Utilizar un archivo geográfico (GeoJSON) del Perú y combinarlo con datos organizados por departamento para generar un mapa que represente la información mediante colores.

------------------------------------------------------------------------

## Herramientas utilizadas

-   R / RStudio
-   Librerías:
    -   `sf` (manejo de datos geográficos)
    -   `ggplot2` (visualización)
    -   `dplyr` (manipulación de datos)

------------------------------------------------------------------------

## Paso 1: cargar librerías

```{r}
library(sf)
library(ggplot2)
library(dplyr)
```

## Paso 2: cargar el mapa del Perú

```{r}
mapa <- st_read("peru_departamental_simple.geojson")
head(mapa)
```

## Paso 3: crear datos por departamento

```{r}
df <- data.frame(
  NOMBDEP = c(
    "AMAZONAS","ANCASH","APURIMAC","AREQUIPA","AYACUCHO",
    "CAJAMARCA","CALLAO","CUSCO","HUANCAVELICA","HUANUCO",
    "ICA","JUNIN","LA LIBERTAD","LAMBAYEQUE","LIMA",
    "LORETO","MADRE DE DIOS","MOQUEGUA","PASCO","PIURA",
    "PUNO","SAN MARTIN","TACNA","TUMBES","UCAYALI"
  ),
  valor = 1:25
)
```

## Paso 4: unir datos

```{r}
mapa <- mapa %>%
  left_join(df, by = "NOMBDEP")
```

## Paso 5: crear el mapa

```{r}
ggplot(data = mapa) +
  geom_sf(aes(fill = valor)) +
  scale_fill_viridis_c(
    option = "viridis",
    na.value = "grey",
    name = "Valor"
  ) +
  ggtitle("Mapa del Perú por Departamentos") +
  theme_minimal()
```

------------------------------------------------------------------------

## Interpretación

El mapa permite visualizar cómo los valores varían entre los distintos departamentos del Perú. Aunque en este caso los datos son simulados, la técnica permite identificar patrones geográficos y comparar regiones de manera visual.

------------------------------------------------------------------------

## Conclusión

La elaboración de mapas coropléticos facilita el análisis espacial de datos, permitiendo representar información de forma clara y comprensible. Este tipo de herramientas es útil en diversos campos como la política, la economía y la salud pública.
