---
title: 'Weekly Exercises #5'
author: "Bea Green"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
lettuce_graph <- garden_harvest %>% 
  filter(vegetable == "lettuce") %>%
  group_by(variety) %>%
  summarize(count = n()) %>% 
  ggplot(aes(x = count, y = fct_reorder(variety,count), text = variety)) +
  geom_col(fill = "darkgreen") +
  labs(title = "Number of Harvests by Variety of Lettuce",
       x = "Number of Harvests",
       y = "")

ggplotly(lettuce_graph,
         tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-2bc3366a187bf6a47b50" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-2bc3366a187bf6a47b50">{"x":{"data":[{"orientation":"v","width":[27,29,1,3,9],"base":[3.55,4.55,0.55,1.55,2.55],"x":[13.5,14.5,0.5,1.5,4.5],"y":[0.9,0.9,0.9,0.9,0.9],"text":["count: 27<br />Farmer's Market Blend","count: 29<br />Lettuce Mixture","count:  1<br />mustard greens","count:  3<br />reseed","count:  9<br />Tatsoi"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(0,100,0,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":133.698630136986},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Number of Harvests by Variety of Lettuce","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.45,30.45],"tickmode":"array","ticktext":["0","10","20","30"],"tickvals":[0,10,20,30],"categoryorder":"array","categoryarray":["0","10","20","30"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Number of Harvests","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,5.6],"tickmode":"array","ticktext":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"tickvals":[1,2,3,4,5],"categoryorder":"array","categoryarray":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"58855d23a2ef":{"x":{},"y":{},"text":{},"type":"bar"}},"cur_data":"58855d23a2ef","visdat":{"58855d23a2ef":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

  

```r
 library(tidytuesdayR)
 tuesdata <- tidytuesdayR::tt_load(2021, week = 24)
```

```
## 
## 	Downloading file 1 of 2: `stocked.csv`
## 	Downloading file 2 of 2: `fishing.csv`
```

```r
 fishing <- tuesdata$fishing
 stocked <- tuesdata$stocked
```


```r
bullhead_data <-fishing %>% 
  filter(species == "Bullheads") %>% 
  drop_na(values) %>% 
  group_by(year, lake) %>% 
  summarize(yearly_catch = sum(values))

 fishing_graph <- ggplot(bullhead_data) +
  geom_point(aes(x = year, y = yearly_catch, color = lake, text = lake), size = .6) +
  facet_wrap(vars(lake)) +
  scale_x_continuous(breaks = c(1910, 1930, 1950, 1970, 1990, 2010)) +
  scale_y_continuous(expand = c(0,0),
                     limits = c(0, 1090)) +
  labs(title = "Bullhead Fish Caught in the Great Lakes, 1913-2015",
       y = "" ,
       x = "",
       caption = "Plot created by Bea Green, data from the Great Lakes Fishery Commission") +
  theme_solarized() +
  scale_color_solarized() +
  theme(panel.grid.minor.y = element_blank(),
        legend.position = "none",
        plot.title.position = "plot") 
 
ggplotly(fishing_graph,
         tooltip = c("y", "text"))
```

```{=html}
<div id="htmlwidget-c416ce06c911de04a0d6" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c416ce06c911de04a0d6">{"x":{"data":[{"x":[1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015],"y":[48,185,186,453,139,133,1056,346,446,304,107,56,5,20,72,122,248,758,690,64,55,224,24,33,30,36,224,53,49,54,47,86,794,458,310,250,154,134,257,299,527,630,621,398,296,180,126,219,294,313,288,193,122,71,67,91,92,135,90,112,163,187,185,240,192,176,141,293,177,205,183,132,176,174,120,168,150,100,80,78,46,86,42,50,85,273,90,52,60,46,25,47,67,78,118,155,162,216,120,102,55,82],"text":["yearly_catch:   48<br />Erie","yearly_catch:  185<br />Erie","yearly_catch:  186<br />Erie","yearly_catch:  453<br />Erie","yearly_catch:  139<br />Erie","yearly_catch:  133<br />Erie","yearly_catch: 1056<br />Erie","yearly_catch:  346<br />Erie","yearly_catch:  446<br />Erie","yearly_catch:  304<br />Erie","yearly_catch:  107<br />Erie","yearly_catch:   56<br />Erie","yearly_catch:    5<br />Erie","yearly_catch:   20<br />Erie","yearly_catch:   72<br />Erie","yearly_catch:  122<br />Erie","yearly_catch:  248<br />Erie","yearly_catch:  758<br />Erie","yearly_catch:  690<br />Erie","yearly_catch:   64<br />Erie","yearly_catch:   55<br />Erie","yearly_catch:  224<br />Erie","yearly_catch:   24<br />Erie","yearly_catch:   33<br />Erie","yearly_catch:   30<br />Erie","yearly_catch:   36<br />Erie","yearly_catch:  224<br />Erie","yearly_catch:   53<br />Erie","yearly_catch:   49<br />Erie","yearly_catch:   54<br />Erie","yearly_catch:   47<br />Erie","yearly_catch:   86<br />Erie","yearly_catch:  794<br />Erie","yearly_catch:  458<br />Erie","yearly_catch:  310<br />Erie","yearly_catch:  250<br />Erie","yearly_catch:  154<br />Erie","yearly_catch:  134<br />Erie","yearly_catch:  257<br />Erie","yearly_catch:  299<br />Erie","yearly_catch:  527<br />Erie","yearly_catch:  630<br />Erie","yearly_catch:  621<br />Erie","yearly_catch:  398<br />Erie","yearly_catch:  296<br />Erie","yearly_catch:  180<br />Erie","yearly_catch:  126<br />Erie","yearly_catch:  219<br />Erie","yearly_catch:  294<br />Erie","yearly_catch:  313<br />Erie","yearly_catch:  288<br />Erie","yearly_catch:  193<br />Erie","yearly_catch:  122<br />Erie","yearly_catch:   71<br />Erie","yearly_catch:   67<br />Erie","yearly_catch:   91<br />Erie","yearly_catch:   92<br />Erie","yearly_catch:  135<br />Erie","yearly_catch:   90<br />Erie","yearly_catch:  112<br />Erie","yearly_catch:  163<br />Erie","yearly_catch:  187<br />Erie","yearly_catch:  185<br />Erie","yearly_catch:  240<br />Erie","yearly_catch:  192<br />Erie","yearly_catch:  176<br />Erie","yearly_catch:  141<br />Erie","yearly_catch:  293<br />Erie","yearly_catch:  177<br />Erie","yearly_catch:  205<br />Erie","yearly_catch:  183<br />Erie","yearly_catch:  132<br />Erie","yearly_catch:  176<br />Erie","yearly_catch:  174<br />Erie","yearly_catch:  120<br />Erie","yearly_catch:  168<br />Erie","yearly_catch:  150<br />Erie","yearly_catch:  100<br />Erie","yearly_catch:   80<br />Erie","yearly_catch:   78<br />Erie","yearly_catch:   46<br />Erie","yearly_catch:   86<br />Erie","yearly_catch:   42<br />Erie","yearly_catch:   50<br />Erie","yearly_catch:   85<br />Erie","yearly_catch:  273<br />Erie","yearly_catch:   90<br />Erie","yearly_catch:   52<br />Erie","yearly_catch:   60<br />Erie","yearly_catch:   46<br />Erie","yearly_catch:   25<br />Erie","yearly_catch:   47<br />Erie","yearly_catch:   67<br />Erie","yearly_catch:   78<br />Erie","yearly_catch:  118<br />Erie","yearly_catch:  155<br />Erie","yearly_catch:  162<br />Erie","yearly_catch:  216<br />Erie","yearly_catch:  120<br />Erie","yearly_catch:  102<br />Erie","yearly_catch:   55<br />Erie","yearly_catch:   82<br />Erie"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(38,139,210,1)","opacity":1,"size":2.26771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(38,139,210,1)"}},"hoveron":"points","name":"Erie","legendgroup":"Erie","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015],"y":[64,60,22,36,8,72,20,4,4,16,32,104,160,84,34,20,26,48,56,62,62,58,80,90,84,84,130,128,132,108,36,18,12,20,62,80,126,92,40,10,6,6,25,20,12,16,4,2,10,24,56,114,90,114,80,87,82,60,50,24,6,4,4,16,14,8,11,8,3,6,8,12,4,4,0,2,0,2,2,2,2,6,4,2,2,2,2,2,2,0,0,0,2,2,2,6,4],"text":["yearly_catch:   64<br />Huron","yearly_catch:   60<br />Huron","yearly_catch:   22<br />Huron","yearly_catch:   36<br />Huron","yearly_catch:    8<br />Huron","yearly_catch:   72<br />Huron","yearly_catch:   20<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:   16<br />Huron","yearly_catch:   32<br />Huron","yearly_catch:  104<br />Huron","yearly_catch:  160<br />Huron","yearly_catch:   84<br />Huron","yearly_catch:   34<br />Huron","yearly_catch:   20<br />Huron","yearly_catch:   26<br />Huron","yearly_catch:   48<br />Huron","yearly_catch:   56<br />Huron","yearly_catch:   62<br />Huron","yearly_catch:   62<br />Huron","yearly_catch:   58<br />Huron","yearly_catch:   80<br />Huron","yearly_catch:   90<br />Huron","yearly_catch:   84<br />Huron","yearly_catch:   84<br />Huron","yearly_catch:  130<br />Huron","yearly_catch:  128<br />Huron","yearly_catch:  132<br />Huron","yearly_catch:  108<br />Huron","yearly_catch:   36<br />Huron","yearly_catch:   18<br />Huron","yearly_catch:   12<br />Huron","yearly_catch:   20<br />Huron","yearly_catch:   62<br />Huron","yearly_catch:   80<br />Huron","yearly_catch:  126<br />Huron","yearly_catch:   92<br />Huron","yearly_catch:   40<br />Huron","yearly_catch:   10<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:   25<br />Huron","yearly_catch:   20<br />Huron","yearly_catch:   12<br />Huron","yearly_catch:   16<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:   10<br />Huron","yearly_catch:   24<br />Huron","yearly_catch:   56<br />Huron","yearly_catch:  114<br />Huron","yearly_catch:   90<br />Huron","yearly_catch:  114<br />Huron","yearly_catch:   80<br />Huron","yearly_catch:   87<br />Huron","yearly_catch:   82<br />Huron","yearly_catch:   60<br />Huron","yearly_catch:   50<br />Huron","yearly_catch:   24<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:   16<br />Huron","yearly_catch:   14<br />Huron","yearly_catch:    8<br />Huron","yearly_catch:   11<br />Huron","yearly_catch:    8<br />Huron","yearly_catch:    3<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:    8<br />Huron","yearly_catch:   12<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    0<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    0<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:    4<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    0<br />Huron","yearly_catch:    0<br />Huron","yearly_catch:    0<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    2<br />Huron","yearly_catch:    6<br />Huron","yearly_catch:    4<br />Huron"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(220,50,47,1)","opacity":1,"size":2.26771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(220,50,47,1)"}},"hoveron":"points","name":"Huron","legendgroup":"Huron","showlegend":true,"xaxis":"x2","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1914,1917,1919,1920,1921,1922,1923,1924,1925,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2014],"y":[46,5,1,2,1,2,25,1,124,0,2,72,132,80,84,70,24,1,128,84,134,204,156,168,202,212,330,9,372,458,230,189,124,84,102,216,312,288,138,96,69,18,42,99,126,129,96,54,27,30,63,120,73,69,36,99,99,219,360,354,336,87,78,60,84,90,9,45,12,33,51,18,24,15,0,6,3,0,0,0,0,0,0,0,0,0,0,0,0,3,3,0,0,0,0],"text":["yearly_catch:   46<br />Michigan","yearly_catch:    5<br />Michigan","yearly_catch:    1<br />Michigan","yearly_catch:    2<br />Michigan","yearly_catch:    1<br />Michigan","yearly_catch:    2<br />Michigan","yearly_catch:   25<br />Michigan","yearly_catch:    1<br />Michigan","yearly_catch:  124<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    2<br />Michigan","yearly_catch:   72<br />Michigan","yearly_catch:  132<br />Michigan","yearly_catch:   80<br />Michigan","yearly_catch:   84<br />Michigan","yearly_catch:   70<br />Michigan","yearly_catch:   24<br />Michigan","yearly_catch:    1<br />Michigan","yearly_catch:  128<br />Michigan","yearly_catch:   84<br />Michigan","yearly_catch:  134<br />Michigan","yearly_catch:  204<br />Michigan","yearly_catch:  156<br />Michigan","yearly_catch:  168<br />Michigan","yearly_catch:  202<br />Michigan","yearly_catch:  212<br />Michigan","yearly_catch:  330<br />Michigan","yearly_catch:    9<br />Michigan","yearly_catch:  372<br />Michigan","yearly_catch:  458<br />Michigan","yearly_catch:  230<br />Michigan","yearly_catch:  189<br />Michigan","yearly_catch:  124<br />Michigan","yearly_catch:   84<br />Michigan","yearly_catch:  102<br />Michigan","yearly_catch:  216<br />Michigan","yearly_catch:  312<br />Michigan","yearly_catch:  288<br />Michigan","yearly_catch:  138<br />Michigan","yearly_catch:   96<br />Michigan","yearly_catch:   69<br />Michigan","yearly_catch:   18<br />Michigan","yearly_catch:   42<br />Michigan","yearly_catch:   99<br />Michigan","yearly_catch:  126<br />Michigan","yearly_catch:  129<br />Michigan","yearly_catch:   96<br />Michigan","yearly_catch:   54<br />Michigan","yearly_catch:   27<br />Michigan","yearly_catch:   30<br />Michigan","yearly_catch:   63<br />Michigan","yearly_catch:  120<br />Michigan","yearly_catch:   73<br />Michigan","yearly_catch:   69<br />Michigan","yearly_catch:   36<br />Michigan","yearly_catch:   99<br />Michigan","yearly_catch:   99<br />Michigan","yearly_catch:  219<br />Michigan","yearly_catch:  360<br />Michigan","yearly_catch:  354<br />Michigan","yearly_catch:  336<br />Michigan","yearly_catch:   87<br />Michigan","yearly_catch:   78<br />Michigan","yearly_catch:   60<br />Michigan","yearly_catch:   84<br />Michigan","yearly_catch:   90<br />Michigan","yearly_catch:    9<br />Michigan","yearly_catch:   45<br />Michigan","yearly_catch:   12<br />Michigan","yearly_catch:   33<br />Michigan","yearly_catch:   51<br />Michigan","yearly_catch:   18<br />Michigan","yearly_catch:   24<br />Michigan","yearly_catch:   15<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    6<br />Michigan","yearly_catch:    3<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    3<br />Michigan","yearly_catch:    3<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan","yearly_catch:    0<br />Michigan"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(211,54,130,1)","opacity":1,"size":2.26771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(211,54,130,1)"}},"hoveron":"points","name":"Michigan","legendgroup":"Michigan","showlegend":true,"xaxis":"x","yaxis":"y2","hoverinfo":"text","frame":null},{"x":[1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015],"y":[57,51,63,55,82,152,519,428,417,440,485,470,437,371,335,256,230,253,191,164,124,140,196,293,331,272,244,286,321,399,369,446,399,387,401,338,281,323,436,408,527,263,379,327,282,264,288,195,241,201,166,172,194,217,182,155,124,100,94,83,79,38,34,14,11,5,5,4,7,6],"text":["yearly_catch:   57<br />Ontario","yearly_catch:   51<br />Ontario","yearly_catch:   63<br />Ontario","yearly_catch:   55<br />Ontario","yearly_catch:   82<br />Ontario","yearly_catch:  152<br />Ontario","yearly_catch:  519<br />Ontario","yearly_catch:  428<br />Ontario","yearly_catch:  417<br />Ontario","yearly_catch:  440<br />Ontario","yearly_catch:  485<br />Ontario","yearly_catch:  470<br />Ontario","yearly_catch:  437<br />Ontario","yearly_catch:  371<br />Ontario","yearly_catch:  335<br />Ontario","yearly_catch:  256<br />Ontario","yearly_catch:  230<br />Ontario","yearly_catch:  253<br />Ontario","yearly_catch:  191<br />Ontario","yearly_catch:  164<br />Ontario","yearly_catch:  124<br />Ontario","yearly_catch:  140<br />Ontario","yearly_catch:  196<br />Ontario","yearly_catch:  293<br />Ontario","yearly_catch:  331<br />Ontario","yearly_catch:  272<br />Ontario","yearly_catch:  244<br />Ontario","yearly_catch:  286<br />Ontario","yearly_catch:  321<br />Ontario","yearly_catch:  399<br />Ontario","yearly_catch:  369<br />Ontario","yearly_catch:  446<br />Ontario","yearly_catch:  399<br />Ontario","yearly_catch:  387<br />Ontario","yearly_catch:  401<br />Ontario","yearly_catch:  338<br />Ontario","yearly_catch:  281<br />Ontario","yearly_catch:  323<br />Ontario","yearly_catch:  436<br />Ontario","yearly_catch:  408<br />Ontario","yearly_catch:  527<br />Ontario","yearly_catch:  263<br />Ontario","yearly_catch:  379<br />Ontario","yearly_catch:  327<br />Ontario","yearly_catch:  282<br />Ontario","yearly_catch:  264<br />Ontario","yearly_catch:  288<br />Ontario","yearly_catch:  195<br />Ontario","yearly_catch:  241<br />Ontario","yearly_catch:  201<br />Ontario","yearly_catch:  166<br />Ontario","yearly_catch:  172<br />Ontario","yearly_catch:  194<br />Ontario","yearly_catch:  217<br />Ontario","yearly_catch:  182<br />Ontario","yearly_catch:  155<br />Ontario","yearly_catch:  124<br />Ontario","yearly_catch:  100<br />Ontario","yearly_catch:   94<br />Ontario","yearly_catch:   83<br />Ontario","yearly_catch:   79<br />Ontario","yearly_catch:   38<br />Ontario","yearly_catch:   34<br />Ontario","yearly_catch:   14<br />Ontario","yearly_catch:   11<br />Ontario","yearly_catch:    5<br />Ontario","yearly_catch:    5<br />Ontario","yearly_catch:    4<br />Ontario","yearly_catch:    7<br />Ontario","yearly_catch:    6<br />Ontario"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(133,153,0,1)","opacity":1,"size":2.26771653543307,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgba(133,153,0,1)"}},"hoveron":"points","name":"Ontario","legendgroup":"Ontario","showlegend":true,"xaxis":"x2","yaxis":"y2","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":59.0386052303861,"r":7.97011207970112,"b":27.8953922789539,"l":37.4595267745953},"plot_bgcolor":"rgba(253,246,227,1)","paper_bgcolor":"rgba(253,246,227,1)","font":{"color":"rgba(147,161,161,1)","family":"","size":15.9402241594022},"title":{"text":"Bullhead Fish Caught in the Great Lakes, 1913-2015","font":{"color":"rgba(101,123,131,1)","family":"","size":19.1282689912827},"x":0,"xref":"paper"},"xaxis":{"domain":[0,0.488139714167111],"automargin":true,"type":"linear","autorange":false,"range":[1907.9,2020.1],"tickmode":"array","ticktext":["1910","1930","1950","1970","1990","2010"],"tickvals":[1910,1930,1950,1970,1990,2010],"categoryorder":"array","categoryarray":["1910","1930","1950","1970","1990","2010"],"nticks":null,"ticks":"outside","tickcolor":"rgba(147,161,161,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":true,"linecolor":"rgba(147,161,161,1)","linewidth":0.724555643609193,"showgrid":true,"gridcolor":"rgba(238,232,213,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"y2","title":"","hoverformat":".2f"},"yaxis":{"domain":[0.543171440431714,1],"automargin":true,"type":"linear","autorange":false,"range":[0,1090],"tickmode":"array","ticktext":["0","250","500","750","1000"],"tickvals":[0,250,500,750,1000],"categoryorder":"array","categoryarray":["0","250","500","750","1000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(147,161,161,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":true,"linecolor":"rgba(147,161,161,1)","linewidth":0.724555643609193,"showgrid":true,"gridcolor":"rgba(238,232,213,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.488139714167111,"y0":0.543171440431714,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"rgba(51,51,51,1)","width":0.724555643609193,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.488139714167111,"y0":0,"y1":25.5043586550436,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.511860285832889,"x1":1,"y0":0.543171440431714,"y1":1},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"rgba(51,51,51,1)","width":0.724555643609193,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.511860285832889,"x1":1,"y0":0,"y1":25.5043586550436,"yanchor":1,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":0.488139714167111,"y0":0,"y1":0.456828559568286},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"rgba(51,51,51,1)","width":0.724555643609193,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":0.488139714167111,"y0":0,"y1":25.5043586550436,"yanchor":0.456828559568286,"ysizemode":"pixel"},{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0.511860285832889,"x1":1,"y0":0,"y1":0.456828559568286},{"type":"rect","fillcolor":"rgba(217,217,217,1)","line":{"color":"rgba(51,51,51,1)","width":0.724555643609193,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0.511860285832889,"x1":1,"y0":0,"y1":25.5043586550436,"yanchor":0.456828559568286,"ysizemode":"pixel"}],"annotations":[{"text":"Erie","x":0.244069857083556,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":12.7521793275218},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Huron","x":0.755930142916444,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":12.7521793275218},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Michigan","x":0.244069857083556,"y":0.456828559568286,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":12.7521793275218},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"},{"text":"Ontario","x":0.755930142916444,"y":0.456828559568286,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(26,26,26,1)","family":"","size":12.7521793275218},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"bottom"}],"xaxis2":{"type":"linear","autorange":false,"range":[1907.9,2020.1],"tickmode":"array","ticktext":["1910","1930","1950","1970","1990","2010"],"tickvals":[1910,1930,1950,1970,1990,2010],"categoryorder":"array","categoryarray":["1910","1930","1950","1970","1990","2010"],"nticks":null,"ticks":"outside","tickcolor":"rgba(147,161,161,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":true,"linecolor":"rgba(147,161,161,1)","linewidth":0.724555643609193,"showgrid":true,"domain":[0.511860285832889,1],"gridcolor":"rgba(238,232,213,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"y2","title":"","hoverformat":".2f"},"yaxis2":{"type":"linear","autorange":false,"range":[0,1090],"tickmode":"array","ticktext":["0","250","500","750","1000"],"tickvals":[0,250,500,750,1000],"categoryorder":"array","categoryarray":["0","250","500","750","1000"],"nticks":null,"ticks":"outside","tickcolor":"rgba(147,161,161,1)","ticklen":3.98505603985056,"tickwidth":0.724555643609193,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":12.7521793275218},"tickangle":-0,"showline":true,"linecolor":"rgba(147,161,161,1)","linewidth":0.724555643609193,"showgrid":true,"domain":[0,0.456828559568286],"gridcolor":"rgba(238,232,213,1)","gridwidth":0.724555643609193,"zeroline":false,"anchor":"x","title":"","hoverformat":".2f"},"showlegend":false,"legend":{"bgcolor":"rgba(253,246,227,1)","bordercolor":"transparent","borderwidth":2.06156048675734,"font":{"color":"rgba(147,161,161,1)","family":"","size":12.7521793275218}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"588522efb219":{"x":{},"y":{},"colour":{},"text":{},"type":"scatter"}},"cur_data":"588522efb219","visdat":{"588522efb219":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```


  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>% 
  group_by(arrival_station, year) %>% 
  summarize(tot_trips = sum(total_num_trips), year, arrival_station) %>% 
  filter(tot_trips > 40000) %>% 
  arrange(desc(tot_trips)) %>% 
  ggplot(aes(x = year, y = tot_trips, group = arrival_station)) +
  geom_point() +
  geom_line() +
  transition_states(arrival_station)  +
  labs(title = "How Many Trains Arrived in the Most Travelled\n Stations in France from 2015-2018?",
       x = "",
       y = "Number of Arriving Trains",
       subtitle = "Station: {closest_state}") +
  scale_y_continuous(labels = scales::comma) +
  theme_bw() +
  theme(plot.background = element_rect(fill = "darkblue"),
        panel.background = element_rect(fill = "ivory"),
        axis.text = element_text(color = "ivory"),
        axis.title = element_text(color = "ivory", size = 14),
        plot.title = element_text(color = "ivory", size = 18),
        plot.title.position = "plot",
        plot.subtitle = element_text(color = "ivory", size = 14))
```



![](small_trains_graph3.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 

I have started the code for you below. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0.


```r
tomato_graph <- garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, date, fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>% 
  mutate(cum_weight_lb = cumsum(daily_harvest_lb)) %>% 
  ungroup() %>% 
  mutate(variety2 = fct_reorder(variety, daily_harvest_lb, sum)) %>% 
  ggplot(aes(x = date, y = cum_weight_lb, fill = variety2)) + 
  geom_area() +
  labs(title = "Cumulative Harvest of Varieties of Tomatoes over Time",
       x = "",
       y = "Cumulative Harvest in lbs",
       subtitle = "Date: {frame_along}",
       fill = "Variety") +
  transition_reveal(date)
```


```r
animate(tomato_graph, end_pause = 15, width=750, height=450)
```



![](cumsum_tomatoes.gif)<!-- -->



## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 2.34, bottom = 39.54, right = 2.66, top = 39.71), 
    maptype = "terrain",
    zoom = 11)
```


```r
ggmap(mallorca_map) +
  geom_path(data = mallorca_bike_day7, 
             aes(x = lon, y = lat, color = ele),
             size = 2) +
  geom_point(data = mallorca_bike_day7, 
             aes(x = lon, y = lat),
             color = "red", size = 2.5) +
  scale_color_viridis_c(option = "mako") +
  theme_map() +
  labs(title = "Lisa's Mallorca Bike Ride with Current Elevation Displayed",
       color = "Elevation",
       subtitle = "Time: {frame_along}") +
  geom_text(data = mallorca_bike_day7 %>% 
              mutate(ele2= formatC(ele, digits = 2, format = "f")),
            aes(label = ele2)) +
  theme(legend.position = "bottom") +
  transition_reveal(time)
```



![](mallorca_bike_graph.gif)<!-- -->

**Comment**: I prefer this moving graph a lot over the static one! It really shows the active nature of the bike ride, and allows the viewer to feel as if they are going along with the route. It is a little harder to look at the route overall, but that could be improved by adding a longer pause at the end of the animation. I also added the current elevation as a geom_text to my plot, which is slightly hard to read but does provide more information for the viewer if they wanted to know a more specific elevation at any one point. 


  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_map <- get_stamenmap(
    bbox = c(left = -79.6, bottom = 8.9, right = -79.5, top = 9), 
    maptype = "terrain",
    zoom = 12)
```


```r
panama_all <- bind_rows(panama_swim, panama_bike, panama_run)
```


```r
ggmap(panama_map) +
  geom_path(data = panama_all, 
             aes(x = lon, y = lat, color = ele),
             size = 1) +
  scale_color_viridis_c(option = "inferno") +
  theme_map() +
  geom_point(data = panama_all, 
             aes(x = lon, y = lat, fill = event),
            size = 2.5,
            shape = 21) +
  labs(title = "Heather Lendway's Panama Triathlon",
       color = "Elevation",
       fill = "Event",
       subtitle = "Time: {frame_along}") +
  transition_reveal(time)
```



![](panama_triathlon.gif)<!-- -->

  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the y-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
covid_log_graph <- covid19 %>% 
  group_by(state) %>%
  mutate(seven_day_lag = lag(cases, n=7),
         week_cases = cases-seven_day_lag) %>% 
  replace_na(list(seven_day_lag = 0,
                  week_cases = 0)) %>% 
  filter(cases > 20) %>% 
  ggplot(aes(x = cases, y = week_cases, group = state)) +
  geom_path(color = "gray67") +
  scale_x_log10(labels = scales::comma) +
  scale_y_log10(labels = scales::comma) +
  geom_point(color = "red", size = 2) +
  geom_text(aes(label = state), check_overlap = TRUE) +
  transition_reveal(date) +
  labs(title = "Trajectory of US Covid19 Cases in All States",
       y = "Cases in the Past Week (log scale)",
       x = "Total Cases (log scale)",
      subtitle = "Date: {frame_along}")
```


```r
animate(covid_log_graph, nframes = 200, duration = 30)
```



![](covid_log_graph2.gif)<!-- -->
  
 **Comment**: This graph shows the current intensity of Covid19 in the different states: if the dot is going down, that means fewer cases in that week than previous weeks, while a dot going up indicates a spike in cases that week. A state with a constant number of cases per week would just travel along a horizontal line. Early on, each state follows relatively the same trajectory where almost all of their total cases occurred in the past week, but some states shoot up again later while others slowly go down even as total cases continue to increase.
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  



```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")
```


```r
cases_by_pop <-
  covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018,
            by = "state") %>% 
  mutate(cases_per_10000 = ((cases*10000)/est_pop_2018),
         day = wday(date, label=TRUE)) %>% 
  filter(day == "Fri")

covid_states_graph <- ggplot(cases_by_pop) +
  geom_map(map = states_map,
           aes(map_id = state, fill = cases_per_10000, group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  labs(title = "Cumulative Covid19 Cases per 10,000 People in Each State",
       fill = "Cases per 10,000 People",
       subtitle = "Date: {frame_time}") +
  theme_map() +
  transition_time(date)
```


```r
animate(covid_states_graph, end_pause = 10, nframes = 200)
```



![](covid_states_graph.gif)<!-- -->

**Comment**: This graph allows the viewer to see what states and parts of the country were hit hardest by the Covid19 pandemic over time with respect to their population. The number of cases per 10,000 residents takes off first in the midwest in November, but gets significantly worse across the country in the winter of 2020. The very top of the Northwest and the Northeast both had much lower cases per 10,000 throughout the whole period. 


## Your first `shiny` app (for next week!)

NOT DUE THIS WEEK! If any of you want to work ahead, this will be on next week's exercises.

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.
  
[Bea's 05exersizes.md](https://github.com/bgreen78/wk5_exersizes/blob/main/Bea_Green_05_exercises.md)


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
