## Geocoding Address Data to Structure Data and Retrieve Additional Attributes

``` R
osm_api <- function(params) {
  params <- gsub(' ','+',paste(params))
  getstr <- paste0('https://nominatim.openstreetmap.org/search?format=json&addressdetails=1&q=',params[1],',Chicago')
  resp <- GET(getstr)
  if (http_type(resp) != "application/json") {
    stop("API did not return json", call. = FALSE)
  }
  parsed <- jsonlite::fromJSON(content(resp, "text"), simplifyVector = F)
  parsed[1]
  # unlist(parsed[[1]]$address)
}
```

### Sample Output

| place_id              | 171681040                                                                            |
|-----------------------|--------------------------------------------------------------------------------------|
| licence               | Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright               |
| osm_type              | way                                                                                  |
| osm_id                | 435724109                                                                            |
| boundingbox1          | 41.8781154                                                                           |
| boundingbox2          | 41.8781333                                                                           |
| boundingbox3          | -87.6337282                                                                          |
| boundingbox4          | -87.6322422                                                                          |
| lat                   | 41.8781229                                                                           |
| lon                   | -87.6329526                                                                          |
| display_name          | West Jackson Boulevard, Loop, South Loop, Chicago, Cook County, Illinois, 60604, USA |
| class                 | highway                                                                              |
| type                  | secondary                                                                            |
| importance            | 0.41                                                                                 |
| address.road          | West Jackson Boulevard                                                               |
| address.neighbourhood | Loop                                                                                 |
| address.suburb        | South Loop                                                                           |
| address.city          | Chicago                                                                              |
| address.county        | Cook County                                                                          |
| address.state         | Illinois                                                                             |
| address.postcode      | 60604                                                                                |
| address.country       | USA                                                                                  |
| address.country_code  | us                                                                                   |
