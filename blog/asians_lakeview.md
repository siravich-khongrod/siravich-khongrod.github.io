# How to make maps visualization worth another thousand words more
![output](https://github.com/siravich-khongrod/siravich-khongrod.github.io/blob/master/blog/res/Asians%20in%20Lakeview.jpg?raw=true)
![output](https://github.com/siravich-khongrod/siravich-khongrod.github.io/blob/master/blog/res/Asians%20in%20Uptown.jpg?raw=true)

Part of my work at the Instutute for Housing Studies is mapping. It has been a challenge when there are many variables to visualize on a map. Having a cartographic map can sometime be misleading because we perceive not only the color contrast but the area of shapes drawn on the map.

I was making a map that visualizes population and the share of population with certain characteristics. I would like to share my methodology so I made another visualization for demographic composition of ethnicity for a community area.

Data
``` R
race.var <- acs.lookup(table.number = "B03002",span = 5,endyear = 2017)
race.acs <- acs.fetch(geography = geo,span = 5, endyear = 2017,table.number = "B03002")
race <- as.data.frame(estimate(race.acs))
race$population <- race$B03002_001
race$asian <- race$B03002_006
```

Post Process
``` R
lakeview <- read.csv('lakeview17race.csv')
head(lakeview)
library(reshape2)
melted <- melt(lakeview[1:ncol(lakeview)],id.vars="TRACT.FIPS.10.NBR")
write.csv(melted[melted$variable!="X",], "lakeview_melt.csv")
```

Census Data: https://factfinder.census.gov/faces/tableservices/jsf/pages/productview.xhtml?pid=ACS_17_5YR_B03002&prodType=table

Tracts Shapefile: https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2018&layergroup=Census+Tracts
