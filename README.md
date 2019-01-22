## Projects



### Listing search tool on leaflet NodeJS web application to query assets in proximity of train station (GeoJSON and MongoDB)

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
| Single Family Home  | Cutaway from the busy city of Bangkok, you can enjoy the most of family time...  | AP Thai | ... |

<p align="center">
  <img src="listing-search-tool-on-leaflet-nodejs-web-application-to-query-assets-in-proximity-of-train-station-geojson-and-mongodb/scrape-snippet.jpg" width="600"/>
</p>



## Temporal Chicago Crime Interactive Map and Analysis under User-centric hierarchies (R and Shiny module)

This project is inspired by my personal experience when I was deciding on which Chicago neighborhood is ideal for renting an apartment. The things i consider are safety, distance to public transportation, distance to downtown and price.

Safety is the most important concern of all. It can be broken down into two aspects which are in regards to the apartment building and the route from the public transportation to the building. I looked at the no go zone map when I started researching for apartments in Chicago. Here is an example:

This map however fails to account for explicitly showing the two safety aspects mentioned. The map includes all types of crime which are not relevant to user needs. Thus an interactive map that allows user to switch through crime types would better provide this information. Alternatively, several static maps of different crime types can also answer to these needs.
[editor on GitHub](https://github.com/siravich-khongrod/siravich-khongrod.github.io/edit/master/README.md) 


[Find me on LinkedIn](https://www.linkedin.com/in/siravich-folk-khongrod/)
