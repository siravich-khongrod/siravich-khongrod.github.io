## Projects

While quoted by Michio Kaku, the higher dimensions might be oout of bounds of our universe. I believe we are living in the third dimension for having either time or spapial position as the fourth dimension. These choices have always been ambigious and we attempt to visualize these data. This was why I have chosen time series and geospatial data analysis as my electives.

Visionary, I am, that I tell myself these are the essential skills that would suffice my career path. Over the past years I have shaped my self to be fully equipted with analytic skills that allow me to further strive to the management role directly involved with the Data Analytics team. 

Storytelling, was the one that I pin the essence. The art of conveying complex ideas would connect my world of computing to the ecosystem of business network leading to the future of technology

Project Directory
* <a href="#listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb">Temporal Chicago Crime Interactive Map and Analysis under User-centric hierarchies (R and Shiny module)</a>
* <a href="#temporal-chicago-crime-interactive-map-and-analysis-under-user-centric-hierarchies-r-and-shiny-module">Listing search tool on leaflet NodeJS web application to query assets in proximity of train station (GeoJSON and MongoDB)</a>
* Automated Valuation Modeling using Linear Regression Techniques (SAS)
* Listing search tool on leaflet NodeJS web application to query assets in proximity of train station (GeoJSON and MongoDB)
* Decentralized Arduino Supervision and Control System for Home Automation
* Three Dimension LED Matrix Multiplexing on Arduino Platform
* Private Software as a Service using Optware on DD-WRT Custom Firmware Routers
* Private Cloud Solution for Remote Multimedia Access

## Listing search tool on leaflet NodeJS web application to query assets in proximity of train station (GeoJSON and MongoDB)

The purpose of this project is to demonstrate practical application of one of the databases of big data and its feature. The concept is to develop a tool that allow users to search apartments and condominiums within a specific distance from the train station. As we all know, in a highly dense area or the central business district, the location, and to be more specific, the distance to the public transportation is a major factor in purchasing  or renting a condominium. Also, the traffic is pretty extreme in Bangkok so transportation by bus or personal vehicle was not considered.

In class, I learned MongoDB and Cassandra. I’ve chosen Mongo for it superior ability to store and manipulate JSON format. Also, the geoJSON features allow data to be fetched using geospatial queries which is suitable for the design of the web application.

The web application communicates to the database using the nodeJS API. The scope is to implement a website that includes the front end interface using an interactive map, the backend that facilitates such communication to the database as well as the database that stores the information of condominium projects.

Data acquisition

The data was acquired from web scrapping techniques in python. I used beautiful soup to parse the html element. The original data was from a website of listings which included pages of information regarding condominium projects in bangkok. The results are saved in a JSON file and are processed to comply with geoJSON format and imported to MongoDB.

<p align="center">
  <img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/source-website.jpg"/>
</p>

Ideally, we want information in the following format

| Property Type  | Property Detial | Property Developer | ... |
| ------------- | ------------- | --- | ------------- | 
| Condominium  | A few steps from the Ekkamai BTS station lies a luxurious comdominium complex...  | AP Thai | ... |
| Single Family Home  | Cutaway from the busy city of Bangkok, you can enjoy the most of family time...  | Sansiri | ... |

<p align="center">
  <img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/scrape-snippet.jpg" width="600"/>
</p>

To save this information as JSON format, the data is stored as a dictionary
```
{     
     "Name":"Life Ladprao"
,    "Price":"฿ 2,900,000"
,    "Coordinates":"13.817551,100.562723"
,    "Property Type":"Condominium"
,    "Property detail":""
,    "Property Developer":"AP Public Co.,Ltd."
,    "Location :"Chatuchak"
,    "Project Area":"\n  7 Rai 71 sq.w."
,    "Room":"studio , 1-2 room , Duplex"
,    "Date":"July 2018"
,    "MRT":"Phahon Yothin"
}
```

The data is then converted into GeoJSON format
```
{   "_id":"5b6756ac06e472949300c43b"
,   "type":"Feature"
,   "geometry":
{       "type":"Point"
,       "coordinates":[          100.562723,         13.817551      ]   }
,   "properties":
{      "Name":"Life Ladprao"
,      "Price":"2900000"   }
}
```

Now on to the web application, these are the elements in the main user interface.
<p align="center">
<img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/web-elements.png"/>
</p>

MongoDB is push with query by the following mechanism
<p align="center">
<img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/code-query-mongodb.png"/>
</p>

Everything orchestrates into a map user interface that is interactive and displays content is most user friendly.
<p align="center">
<img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/nodejs-leaflet-mongo-arch.png"/>
</p>

This is the output of a search query
<p align="center">
<img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/user-interface.png"/>
</p>

## Temporal Chicago Crime Interactive Map and Analysis under User-centric hierarchies (R and Shiny module)

This project is inspired by my personal experience when I was deciding on which Chicago neighborhood is ideal for renting an apartment. The things i consider are safety, distance to public transportation, distance to downtown and price.

Safety is the most important concern of all. It can be broken down into two aspects which are in regards to the apartment building and the route from the public transportation to the building. I looked at the no go zone map when I started researching for apartments in Chicago. Here is an example:

<p align="center">
<img src="http://i.yochicago.com/images/hpmain/773/285773.jpg?preset=yosize"/>
</p>

This map, however, fails to account for explicitly showing the two safety aspects mentioned. The map includes all types of crime which are not relevant to user needs. Thus an interactive map that allows user to switch through crime types would better provide this information. Alternatively, several static maps of different crime types can also answer to these needs.

I contributed the this project in programming visualization in R and by driving team forward to meet deliverable requirements. After teaming with five people through the discussion forum, I’ve set up meetings and agendas and have taken minutes of meeting on three meetups. When it was close to project deliverable deadline, we did not have directions as our exploratory data analysis does not show any interesting patterns nor correlation on the our hypothesis questions which is crossing Chicago crime data with other data (eg. census data). I got inspired by making examples of heatmap with Excel using pivot tables, data slicers, and colored auto-formatting as shown below.
<p align="center">
<img src="temporal-interactive-chicago-crime-map/excel-heatmap.png"/>
</p>

The example above shows the occurance of violent crime based on time of the day and day of the week. It was a successful prototype that led to making user centric heirachies that are related to deciding which part of the community area is ideal.

Since it is hard to figure out what would be the user requirements, I made an interactive R program using Shiny module. It generates a HTML page that allow parameters to be adjusted.

<p align="center">
  <img src="temporal-interactive-chicago-crime-map/chicago-crime-map-loop.png"/>
  <img src="temporal-interactive-chicago-crime-map/chicago-crime-map-lincoln-park.png"/>
</p>

Sample Code:

``` R
ui <- fluidPage(theme = shinytheme("lumen"),
                titlePanel("Chicago Crime"),
                sidebarLayout(
                  sidebarPanel(
                    
                    # Select type of trend to plot
                    selectInput(inputId = "input_community_area", label = strong("Community Area"),
                                choices = unique(community_area_centroid$community_area),
                                selected = "LOOP"),
                    
                    sliderInput("month", "Month:",
                                min = 1, max = 12,
                                value = c(1,12)),

                    sliderInput("year", "Year:",
                                min = 2010, max = 2017,
                                value = c(2017,2017)),
                    radioButtons("ctype", "Choose one:",
                                 choiceNames = list(
                                   "Violent Crime",
                                   "Property Crime",
                                   "All Crimes"
                                 ),
                                 choiceValues = list(
                                   "violent", "property", "all"
                                 ))
                  ),
                  
                  # Output: Description, lineplot, and reference
                  mainPanel(
                    plotOutput(outputId = "chicagoCrimeMap", height = "500px")#,
                    #  textOutput(outputId = "desc"),
                    #  tags$a(href = "https://www.google.com/finance/domestic_trends", "Source: Google Domestic Trends", target = "_blank")
                  )
                )
)

# Define server function
server <- function(input, output) {
  
  output$chicagoCrimeMap <- renderPlot({
    
    # SUBSET DATA
    crimedata <- crimecsv %>%
      filter(
        month >= as.numeric(input$month[1]) & month <=as.numeric(input$month[2]) &
          year >= as.numeric(input$year[1]) & year <=as.numeric(input$year[2])
      )
    if(input$ctype=='violent') {
      crimedata <- filter(crimedata, Primary.Type=='HOMICIDE'|Primary.Type=='CRIMINAL SEXUAL ASSAULT'|Primary.Type=='ROBBERY'|Primary.Type=='ASSAULT'|Primary.Type=='BATTERY')
    } else if(input$ctype=='property') {
      crimedata <- filter(crimedata, Primary.Type=='BURGLARY'|Primary.Type=='LARCENY'|Primary.Type=='MOTOR VEHICLE THEFT'|Primary.Type=='ARSON')
    }
    
    # GET MAP from GOOGLE
    community_area_coord <- 
      community_area_centroid[community_area_centroid$community_area==input$input_community_area,]
    mapImage <- get_map(location = c(lon = community_area_coord$lon, lat = community_area_coord$lat), zoom=13)
    
    # DRAW COMMUNITY AREA BORDER
    area_border <- area
    area_border <- subset(area_border, community==input$input_community_area)
    
    # GET SHAPE WITHIN COMM AREA
    area2.sub <- raster::intersect(area_border,area2.sub)
    area2_sub_fortify <- fortify(area2.sub, region = "beat_num")
    
    # AGG CRIME DATA
    coords <- crimedata[c("Longitude","Latitude")]
    coords <- na.omit(coords)
    sp <- SpatialPoints(coords)
    shape_over <- over(sp,area2.sub)
    by_beat_num <- shape_over %>%
      group_by(beat_num) %>%
      summarize(total=n())
    
    by_beat_num <- by_beat_num[!is.na(by_beat_num$beat_num),]
    colnames(by_beat_num) <- c("id","total")
    by_beat_num$id <- as.character(by_beat_num$id)
    
    # JOIN WITH MAP
    total_map <- left_join(area2_sub_fortify,by_beat_num)

    ggmap(mapImage) +
      geom_polygon(data = total_map, aes(x=long, y=lat, group=group, fill=total), 
                   color="black",size=0.5,alpha=0.9) +
      # scale_fill_gradient(low='white', high='red') +
      scale_fill_distiller(type="seq", trans="reverse", palette = "Reds") +
      geom_polygon(data = area_border, aes(x=long, y=lat, group=group, fill=total), 
                   color = "black", fill=NA, size=1) +
      labs(fill='Total no. of Crime')  +
      theme(axis.line=element_blank(),axis.text.x=element_blank(),
            axis.text.y=element_blank(),axis.ticks=element_blank(),
            axis.title.x=element_blank(),
            axis.title.y=element_blank())
  })
  
}
```

More Upcoming Project
* Automated Valuation Modeling using Linear Regression Techniques (SAS)
* Listing search tool on leaflet NodeJS web application to query assets in proximity of train station (GeoJSON and MongoDB)
* Decentralized Arduino Supervision and Control System for Home Automation
* Three Dimension LED Matrix Multiplexing on Arduino Platform
* Private Software as a Service using Optware on DD-WRT Custom Firmware Routers
* Private Cloud Solution for Remote Multimedia Access

[editor on GitHub](https://github.com/siravich-khongrod/siravich-khongrod.github.io/edit/master/README.md) 


[Find me on LinkedIn](https://www.linkedin.com/in/siravich-folk-khongrod/)
<p align="center">
<img src="linkedin.png"/>
</p>
