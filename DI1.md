### Data Insights 1 - gganimate

For the this data insight assignment, I chose to demontrate the story telling potential of the package gganimate. 

Visualizations can make / break data science projects, as they are one of primary way to express insights found from your data. A p value or mean squared error statistic may get lost on certain audiences, as they might not have the experience / education to fully comprehend what they mean. However, almost everyone can understand the message bar / scatter plot try to explain.

gganimate helps in furthering a message by adding animation to most standard graphs. 

To demonstrate gganimate's functionality, I will be using the gapminder dataset. Gapminder contains information on a country's life expectancy, population and GDP per capita from 1952-2007.

![](/Users/lucaparavano/Desktop/DATA440/DATA440/DataInsight1/gapmindertable.png)

If we wanted to plot the relationship between GDP per capita and life expectancy we could:

* Plot every observation on the graph
  * Might be to busy, difficult for the audience to pull any insights
* Group by country then graph
  * Will lose out on a country's potential change throughout the years, might jump to false conclusions
* Use a seperate graph for every year
  * Might be too much information for the audience to interpret, might miss out on small details

OR

We could use the gganimate package.

```R
library(ggplot2)
library(gganimate)
library(gapminder)

View(gapminder)

p <- ggplot(gapminder, 
            aes(gdpPercap, lifeExp, size=pop, colour=country))+
  geom_point(alpha=.7)+
  scale_color_manual(values=country_colors)+
  scale_size(range = c(2,15))+
  scale_x_log10()+
  facet_wrap(~continent)+
  theme(legend.position = 'none')+
  theme(axis.text=element_text(size=20),
        axis.title=element_text(size=22, face='bold'),
        strip.text = element_text(size=20))

p2 <- p+
  labs(title='Year: {frame_time}', x='GDP per Capita',y='life expectancy')+
  transition_time(year)+
  theme(plot.title = element_text(size=23, face='bold'))+
  theme(plot.title = element_text(hjust = 0.5))

animate(p2, nframes=120,
        renderer= gifski_renderer('PerCapGDP.gif'),
        height=700,width=1000)
```

![](/Users/lucaparavano/Desktop/DATA440/DATA440/DataInsight1/PerCapGDP.gif)

In my opinion, using gganimate is the clear winner when it comes to cleanly displaying this data.

####  Applications to DATA 441

Just to familiarize myself to the package, I went through the student work section of the syllabus and filterred the gapminder dataframe to only inclucde countries that were used for project one. Here is the resulting graphic.

![](/Users/lucaparavano/Desktop/DATA440/DATA440/DataInsight1/SfdafPerCapGDP.gif)



Additionally, and more importantly, we can potentially use gganimate to better visualize the population counts / distribution of a country from the years 2000 - 2020 as Worldpop.org has that data.

![](/Users/lucaparavano/Desktop/DATA440/DATA440/DataInsight1/worldpop.png)

An example graph to add year as a variable that might be interesting. (Currently working on it)

![](/Users/lucaparavano/Desktop/DATA440/DATA440/images/Ex3plot3.png)










