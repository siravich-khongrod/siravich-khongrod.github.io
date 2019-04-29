beat.agg <- read.csv('test_beat_agg.csv')


head(beat.agg)
library(dplyr)
dplyr::group_by(beat.agg,Row.Labels)

library(reshape)
cast(beat.agg, beat ~ .)


unpivot <- (melt(data = beat.agg,id.vars=c('beat')))
colnames(unpivot) <- c('beat','type','count')

boxplot(beat ~ type, data=unpivot)


library(ggplot2)



ggplot(unpivot[!(unpivot$type %in% c('Other.Crime','Drug.Related')),], aes(x=type, y=count)) + 
  geom_boxplot(fill="slateblue", alpha=0.2) + 
  xlab("type")

ggplot(unpivot[!(unpivot$type %in% c('Other.Crime','Drug.Related')),], aes(x=type, y=count)) + 
  geom_violin(fill="slateblue", alpha=0.2) + geom_boxplot(width=0.05)

ggplot(unpivot[!(unpivot$type %in% c('Other.Crime','Drug.Related')),], aes(x=type, y=count)) + 
  geom_violin(fill="slateblue", alpha=0.2) + stat_summary(fun.data="mean_sdl", mult=1, 
                                                          geom="crossbar", width=0.02, color='red')


ggplot(unpivot[!(unpivot$type %in% c('Other.Crime','Drug.Related')),], aes(x=type, y=count)) + 
  geom_violin(fill="slateblue", alpha=0.2) + stat_summary(fun.data=mean_sdl, mult=1, 
  geom="pointrange", color="red")


tract.agg <- read.csv('test_tract_agg.csv',stringsAsFactors = F)
head(tract.agg)
tract.agg <- aggregate(tract.agg$COUNT,by=list(tract.agg$TRACT_FIPS_10_NBR,tract.agg$TYPE),FUN=sum)


ggplot(tract.agg[!(tract.agg$TYPE %in% c('Other Crime','Drug Related')),], aes(x=TYPE, y=COUNT)) + 
  geom_boxplot(fill="slateblue", alpha=0.2) + 
  xlab("type")

ggplot(tract.agg[!(tract.agg$TYPE %in% c('Other Crime','Drug Related')),], aes(x=TYPE, y=COUNT)) + 
  geom_violin(fill="slateblue", alpha=0.2) + geom_boxplot(width=0.05)
  xlab("type")

  
ggplot(tract.agg[!(tract.agg$TYPE %in% c('Other Crime','Drug Related')),], aes(x=TYPE, y=COUNT)) + 
    geom_violin(fill="slateblue", alpha=0.2)  + stat_summary(fun.data=mean_sdl, mult=1, 
                                                             geom="pointrange", color="red")