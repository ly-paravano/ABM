### Exercise 3 - Extracting Populations from a Raster and Aggregating to each Unit

**Responsibilities**

- Plot the population distribution of administrative subdivisions of Gabon.

Installing and loading packages.

```R
install.packages('sf',dependencies=T)
install.packages('tidyverse',dependencies=T)
install.packages('raster',dependencies=T)

library(sf)
library(tidyverse)
library(raster)
```

Load in and plot data.

```R
gabpop <- raster('gab_ppp_2018_1km_Aggregated.tif')
gab_int <- read_sf('GABON/gadm36_GAB_0.shp')
gab_adm1 <- read_sf('GABON/gadm36_GAB_1.shp')
gab_adm2 <- read_sf('GABON/gadm36_GAB_2.shp')

plot(gabpop)
plot(st_geometry(gab_adm1), add=T)
```

![](/Users/lucaparavano/Desktop/DATA440/DATA440/images/Ex3plot1.png)

### Challenge Questions

Add population distribution to graph

```R
pop_gab_adm1 <- raster::extract(gabpop,gab_adm1,df=T)

totals_adm1 <- pop_gab_adm1 %>%
  group_by(ID) %>%
  summarize(pop18 = sum(gab_ppp_2018_1km_Aggregated, na.rm = TRUE))

gab_adm1 <- gab_adm1 %>%
  add_column(pop18 = totals_adm1$pop18)

ggplot(gab_adm1) +
  geom_sf(aes(fill = log(pop18))) +
  geom_sf_text(aes(label = NAME_1),
               color = "black",
               size = 3) +
  scale_fill_gradient(low = "white", high = "red")
```

![](/Users/lucaparavano/Desktop/DATA440/DATA440/images/Ex3plot2.png)

Population density of Gabon's counties.

```R
pop_gab_adm2 <- raster::extract(gabpop,gab_adm2,df=T)

totals_adm2 <- pop_gab_adm2 %>%
  group_by(ID) %>%
  summarize(pop18 = sum(gab_ppp_2018_1km_Aggregated, na.rm = TRUE))

gab_adm2 <- gab_adm2 %>%
  add_column(pop18 = totals_adm2$pop18)

ggplot()+
  geom_sf(data=gab_adm1,size=1.5,color='gray50')+
  geom_sf(data= gab_adm2, size=.85, color='gray50',aes(fill = log(pop18)))+
  geom_sf_text(data=gab_adm2,
               aes(label=NAME_2),
               size=2)+
  xlab("longitude") + ylab("latitude") +
  ggtitle("Population density of Gabon ") +
  scale_fill_gradient(low = "white", high = "red") +
  theme(plot.title = element_text(hjust = 0.5))
```

![](/Users/lucaparavano/Desktop/DATA440/DATA440/images/Ex3plot3.png)


