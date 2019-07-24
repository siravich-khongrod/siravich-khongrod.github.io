# Asians in Lakeview
![output](https://github.com/siravich-khongrod/siravich-khongrod.github.io/blob/master/blog/res/Asians%20in%20Lakeview.jpg?raw=true)

Data
```
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
