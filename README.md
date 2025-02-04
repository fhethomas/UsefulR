# UsefulR
R Code

## Load data into a dataframe
```
# load data
df <- read.csv(file='C:/file.csv')
# return data head
head(df)
```

## Create Histogram
```
# remove  where population below this number
lower_limit<-10
df<-df[which(df$Population>lower_limit),]

#plot histogram with median, 5th & 95th percentile
library(ggplot2)

g<-ggplot(df,aes(x=Retention)) + geom_histogram(binwidth=2)+geom_vline(aes(xintercept=median(Retention)),
            color="red", linetype="solid", size=1)+geom_vline(aes(xintercept=quantile(Retention,c(0.05))),
            color="red", linetype="dashed", size=1)+geom_vline(aes(xintercept=quantile(Retention,c(0.95))),
            color="red", linetype="dashed", size=1)


g
```

## return all items below 5th percentile
```
percentile_5 = quantile(df$Retention,c(0.05))
df[which(df$Retention<percentile_5),]
```

## Chi-Square
```
arr<-matrix(c(269,557,262,824),,nrow=2,ncol=2)
chisq <- chisq.test(arr)
chisq
```

## T-test
```
# load data - test data
df <- read.csv(file='C:/testData.csv')
# return data head
head(df)
# histogram with 2 different distributions
ggplot(df,aes(x=Value,fill=Category)) + geom_histogram(color="#e9ecef", alpha=0.6, position = 'identity')
a_group<-df[which(df$Category=="a"),]
b_group<-df[which(df$Category=="b"),]
t.test(a_group$Value,b_group$Value)
```

## Scatter plot with linear fit
```
# Create 5xValue + random
a_group_x <- a_group$Value ^2+a_group$Value  + rnorm(500, mean=50, sd=20)
a_group$x = a_group_x
# create plot with scatter (geom_point), line (geom_smooth)
# Can just use linear regression with geom_smooth(method=lm)
# Can get rid of standard error with geom_smooth(se=False)
ggplot(a_group, aes(x=Value, y=x)) + geom_point()+geom_smooth()
```
## Create a scatter plot with 2 categories
```
a_y <- df[which(df$Category=="a"),]
a_y$Value2 <- a_y$Value^3.5 + rnorm(500, mean=50, sd=20)
b_y <- df[which(df$Category=="b"),]
b_y$Value2 <- b_y$Value^4 + rnorm(500, mean=50, sd=20)
df_y<-rbind(a_y, b_y)
# print dimension
dim(df_y)
head(df_y)
# Create scatter plot with non-linear fit
ggplot(df_y,aes(x=Value,y=Value2,fill=Category,color=Category))+ geom_point()+geom_smooth()

```

