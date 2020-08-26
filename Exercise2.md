###  Exercise 2 - Projecting, Plotting and Labelling Administrative Subdivisions

**Responsibilites**

- Map out the administrative subdivisions of a country of our chosing.
  - I personally chose to analyze Gabon



Installing and loading packages.

```R
install.packages('tidyverse',dependencies=T)
install.packages('sf',dependencies=T)
library(sf)
library(tidyverse)
```

Read in `.shp` files.

```R
som_int <- read_sf('work/GABON/gadm36_GAB_0.shp')
som_int1 <- read_sf('work/GABON/gadm36_GAB_1.shp')
som_int2 <- read_sf('work/GABON/gadm36_GAB_2.shp')
```

Plot of Gabon's highest 2 levels of regions.

```R
ggplot() +
  geom_sf(data=som_int1,
          size=.65,
          color='gray50',
          fill='gold3',
          alpha=.65) +
  geom_sf(data = som_int,
          size=2,
          alpha=0) +
  geom_sf_label(data = som_int1,
               aes(label=som_int1$NAME_1),
               size=2.5) +
  geom_sf_text(data=som_int,
                aes(label=som_int$NAME_0),
                size=10,
                color='black')

```

![](images/Ex2plot1.png)

Plot of all of Gabon's administrative levels.

```R
ggplot() +
  geom_sf(data=som_int2,
          size=.45,
          color='gray50',
          fill='gold3',
          alpha=.65) +
  geom_sf(data=som_int1,
          size=1.35,
          color='gray50',
          alpha=0) +
  geom_sf(data=som_int,
          size=1.75,
          color='black',
          alpha=0)+
  geom_sf_text(data=som_int2,
               aes(label=som_int2$NAME_2),
               size=1.5)+
  geom_sf_text(data=som_int1,
               aes(label=som_int1$NAME_1),
               size=3)
```

![](images/Ex2plot2.png)

# Challenge Questions Coming Soon!! (Laptop broke)




