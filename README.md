# DataLimpia_WDI_VDem_LatAm
# Datos limpios WDI + V-Dem para América Latina (2000–2024)

Este repositorio contiene una base de datos en formato panel que combina
indicadores de desarrollo del **World Development Indicators (WDI, World Bank)**
con índices de democracia del **Varieties of Democracy (V-Dem)** para países de
América Latina entre 2000 y 2024.[web:209][web:214][web:100]

El objetivo es disponer de una tabla **limpia y documentada** para análisis
cuantitativo (regresiones) y visualizaciones (dashboards).

---

## Archivo principal

- `data_limpia_latam_democracia.csv`

Cada fila corresponde a una combinación **país–año**.
Las columnas incluyen:

### Identificadores

| Variable       | Descripción                        |
|----------------|------------------------------------|
| `Country Name` | Nombre del país (WDI)             |
| `Country Code` | Código ISO del país (WDI)         |
| `year`         | Año (2000–2024)                   |

### Indicadores WDI (World Bank)

| Variable                | Descripción (traducción libre)                                                                 |
|-------------------------|------------------------------------------------------------------------------------------------|
| `SI.POV.GINI`           | Gini index (medida de desigualdad de ingresos).[web:211]                                       |
| `SI.POV.NAHC`           | Poverty headcount ratio at national poverty lines (% de la población).[web:211]               |
| `EN.GHG.CO2.PC.CE.AR5`  | Emisiones de CO₂ per cápita (toneladas de CO₂e).[web:211]                                     |
| `ER.PTD.TOTL.ZS`        | Áreas protegidas terrestres y marinas (% del total de la superficie del país).[web:214]       |
| `NY.GDP.MINR.RT.ZS`     | Renta minera (% del PIB).[web:211]                                                            |
| `NY.GDP.FRST.RT.ZS`     | Renta forestal (% del PIB).[web:211]                                                          |
| `NY.GDP.TOTL.RT.ZS`     | Renta total de recursos naturales (% del PIB).[web:211]                                       |
| `SP.POP.TOTL`           | Población total.[web:211]                                                                      |

Los valores faltantes de WDI (originalmente codificados como `..`) se
reemplazaron por `NA`. Todas estas columnas están en formato numérico.

### Indicadores V-Dem

| Variable       | Descripción                                                            |
|----------------|------------------------------------------------------------------------|
| `country_id`   | Identificador numérico de país en V-Dem.[web:105]                     |
| `v2x_libdem`   | Índice de democracia liberal (liberal democracy index).[web:105]      |
| `v2x_egaldem`  | Índice de democracia igualitaria (egalitarian democracy index).[web:105] |
| `v2x_api`      | Índice agregado de democracia/autocratización (polyarchy).[web:105]   |

Los índices V-Dem están escalados aproximadamente entre 0 y 1, donde valores más
altos indican mayor nivel de democracia en la dimensión correspondiente.[web:105][web:114]

---

## Cobertura geográfica

Incluye países de América Latina según disponibilidad de datos en WDI y V-Dem, por ejemplo:[file:186][file:185]

- Argentina, Bolivia, Brazil, Chile, Colombia, Ecuador, Paraguay, Peru,
  Uruguay, Venezuela.
- México y países de Centroamérica y el Caribe con datos comparables.

---

## Uso sugerido en R

Ejemplo para cargar la base desde R:

```r
library(readr)
library(dplyr)
library(ggplot2)

# Leer después de descargar el archivo a tu directorio de trabajo
datos <- read_csv("data_limpia_latam_democracia.csv")

# Modelo sencillo: democracia liberal ~ rentas de recursos + pobreza
mod1 <- lm(
  v2x_libdem ~ NY.GDP.TOTL.RT.ZS + SI.POV.GINI + EN.GHG.CO2.PC.CE.AR5,
  data = datos,
  na.action = na.omit
)

summary(mod1)

# Gráfico: democracia vs rentas de recursos
ggplot(datos, aes(x = NY.GDP.TOTL.RT.ZS, y = v2x_libdem)) +
  geom_point(alpha = 0.4) +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(
    x = "Rentas totales de recursos naturales (% del PIB)",
    y = "Democracia liberal (V-Dem v2x_libdem)"
  )
```

---

## Cómo citar los datos

### World Development Indicators (World Bank)

De acuerdo con las recomendaciones del Banco Mundial, una cita típica es:[web:214][web:209]

> World Bank. *World Development Indicators*. Washington, DC: World Bank.  
> Available at: https://data.worldbank.org/ (accessed YYYY-MM-DD).

En un trabajo académico en español, podrías usar por ejemplo:

> Banco Mundial. *World Development Indicators*. Washington, DC: Banco Mundial.  
> Datos descargados de https://data.worldbank.org/ el DD/MM/AAAA.

### Varieties of Democracy (V-Dem)

V-Dem recomienda citar el dataset y el paper metodológico correspondiente.[web:100][web:113]

Cita general (adaptada al español; ajusta el año/versión a la que realmente
uses, por ejemplo v16):

> Coppedge, Michael, John Gerring, Carl Henrik Knutsen, Staffan I. Lindberg,  
> Svend-Erik Skaaning, Jan Teorell, et al. *Varieties of Democracy (V-Dem)  
> [Country–Year Dataset] v16*. Gothenburg: V-Dem Institute, Universidad de  
> Gotemburgo.

Y puedes agregar una cita abreviada en el texto:

> “Índices de democracia tomados de V-Dem (Varieties of Democracy, dataset país–año, versión 16).”

Consulta siempre la página oficial de V-Dem para verificar la cita recomendada
para la versión concreta que hayas descargado.[web:100][web:113]

---

## Licencia y uso

- Los datos originales pertenecen al Banco Mundial (WDI) y al V-Dem Institute.
- Esta tabla es una integración y limpieza propia para uso académico
  y de investigación. Verifica las condiciones de uso de WDI y V-Dem
  si vas a redistribuir los datos o usarlos más allá de fines académicos.[web:100][web:214]
