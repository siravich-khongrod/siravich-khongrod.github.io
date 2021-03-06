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
,   "geometry":{
        "type":"Point"
,       "coordinates":[          100.562723,         13.817551      ]   }
,   "properties":{
        "Name":"Life Ladprao"
,       "Price":"2900000"   }
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
