### Fetching Census Data from American Community Survey (ACS)

Using API is a great of retrieving information especially throught the internet of overwhelming information. Today, I demonstrate a way to retrieve information from the American Comminuty Survey using an iterative approach. The reason for this design is because the API have changed over the year and it is no longer possible to retrieve data for all Census Tract for a large unit of geography. In this case, I would like to retrieve data for Michigan State.

There are two major parts of R Coding. The first is the function takes in the search parameter which is the table name and the geography area. The second is the function caller which accepts data from the ACS API and further process the data. For proprietary reasons I cannot disclose the calculations the snippets below would allow retrieving a large dataset of large geographies. This information is useful for doing geospatial analysis, resulting in reports that helps policy makers, developers and other stakeholders to make better decision based on data of the American population.


#### Retrieval Mechanism
``` R
counties <- geo.lookup(state="MI",county = "*")[4]
counties <- counties[!is.na(counties)]

acs.fetcher <- function(acsvar,year=acsyear) {
  i=0
  est<-NA
  se<-NA
  for (county in counties) {
    print(county)
    acs.obj.itr <- NULL
    attempt=1
    while( is.null(acs.obj.itr) && attempt <= 3 ) {
      print(attempt)
      attempt <- attempt + 1
      try(
        acs.obj.itr <- acs.fetch(geography=geo.make(state = "IL",county=county, tract = "*"),
                                 endyear=year,variable = acsvar)
      )
    } 
    # if(!exists("est") && !exists("se")){
    if(i==0){
      est <- estimate(acs.obj.itr)
      se <- standard.error(acs.obj.itr)
    }
    else {
      est <- rbind(est,estimate(acs.obj.itr))
      se <- rbind(se,standard.error(acs.obj.itr))
    }
    # print(head(estimate(acs.obj.itr),1))
    i=i+1
    # if (i>=3){break()}
  }
  acs.obj.itr@estimate <- est
  acs.obj.itr@standard.error <- se
  return(acs.obj.itr)
}
```

#### Retriever and Information Processing
``` R
acsyear = 2017
tblnam <- c("Population_by_Age","...")
tblnam <- paste0("ACS",acsyear,"_",tblnam)

# AGE & POPULATION
tname <- grep("B01001", tblnam, value=TRUE)[1]
age.var <- acs.lookup(table.number = "B01001",span = 5,endyear = 2015, dataset = "acs") # create variable
age <- acs.fetcher(age.var,acsyear)
```
