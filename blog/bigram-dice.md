### Using BiGrams and Dice Coefficient to Aid Joining Survey Data

While I was studying Information Retreval, I came accross ways to compare parameters of the search query. Calculating Dice Coefficients using Bigrams is one of the ways we can match a set of string to measure the similarity.

For instance, the word statistics and statistical can be broken down into the following:
```
> slicer('statistics')
[1] "st" "ta" "at" "ti" "is" "st" "ti" "ic" "cs"
> slicer('statistictical')
[1] "st" "ta" "at" "ti" "is" "st" "ti" "ic" "ct" "ti" "ic" "ca" "al"
dice(slicer('statistics'), slicer('statistictical'))
[1] 0.75
```

``` R
slicer <- function(text){
  if(is.na(text)) {return (NA)}
  bigrams = c()
  for (i in 2:nchar(text)-1) {
    bigrams <- append(bigrams,c(substr(text,i,i+1)))
  }
  return(bigrams)
}

dice <- function(seta,setb){
  (length(intersect(seta,setb))*2)/(length(unique(seta))+length(unique(setb)))
}

singleunit17$dice1516 <- NA
singleunit17$dice1617 <- NA
for (i in 1:nrow(singleunit17)){
  singleunit17[i,]$dice1516 <- dice(slicer(tolower(gsub('[[:punct:] ]+','',singleunit17[i,]$Name15))),
                                    slicer(tolower(gsub('[[:punct:] ]+','',singleunit17[i,]$Name16))))
  singleunit17[i,]$dice1617 <- dice(slicer(tolower(gsub('[[:punct:] ]+','',singleunit17[i,]$Name16))),
                                    slicer(tolower(gsub('[[:punct:] ]+','',singleunit17[i,]$Name17))))
}
```
