# CYCLISTIC_BIKE_SHARE_Q3_2020_ANALYSIS.
This analysis is a case study from Google Data Analytics Certificate course. The datasets were made available by Motivate International Inc.
In this project, I will be merging datasets from July, August and September 2020 to get a single dataset for Q3 2020, conduct simple analysis to help answer the key question "In what ways do member and casual riders use cyclistic bikes differently?"

I cleaned and performed some calculations of the datasets on Microsoft Excel, before exporting them to R i.e
 - I dropped Columns representing the ‘starting latitude’, ‘ending latitude’ ‘starting longitude’ ‘ending longitude’ of rides were removed as they were not necessary for the business task.
 - 6,846 (July 1745, August 2769, September 2132) Records of data with negative ride_length were removed because they were taken out of the docks for quality checks.
 - I added 3 rows to help solve the business task i.e ‘ride_length’, ‘member_casual’, ‘day_of_week’.

## Installing required R packages.

tidyverse is for data import and wrangling 

```r
install.packages("tidyverse")
library("tidyverse")
```

readr is used for reading rectangular data e.g csv file

```r
install.packages("readr")
library("readr")
```

ggplot is for visualizations

```r
install.packages("ggplot2")
library(ggplot2) 
```

libridate for date functions

```r
install.packages("libridate")
library("libridate")
```

dplyr help complete some data manipulation tasks

```r
install.packages("dplyr")
library("dplyr")
```

purr works with functions and vectors to help with coding

```r
install.packages("purrr")
library("purrr")
```

## Select working directory with Q3 csv files
 
I selected my working directory using the session tab.

getwd() will wisplay my working directory
```r
getwd()
```
# 1. COLLECT DATA 

## Import csv cyclistic bike share q3 data to global environment
 
- July 2020 dataset
 
```r
library(readr)
cyclistic_bike_share_july_2020 <- read_csv("cyclistic_bike_share_july_2020.csv")
View(cyclistic_bike_share_july_2020)
```


- August 2020 dataset

```r
library(readr)
cyclistic_bike_share_august_2020 <- read_csv("cyclistic_bike_share_august_2020.csv")
View(cyclistic_bike_share_august_2020)
```

- September 2020 dataset

```r
library(readr)
cyclistic_bike_share_sept_2020 <- read_csv("cyclistic_bike_share_sept_2020.csv")
View(cyclistic_bike_share_sept_2020)
```


# 2. WRANGLE DATA AND COMBINE INTO A SINGLE FILE

## Inspected and changed data type on the files tab
I converted ride_id and rideable_type to character in all datasets so that they can stack correctly.

Stacked cyclistic data from July, August, September 2020 to get Q3 data.

- I used bind_rows function to merge the data from July,August, September 2020 to get Q3 dataframe.
       
```r
cyclistic_bike_share_Q3_2020 <- bind_rows(cyclistic_bike_share_july_2020,cyclistic_bike_share_august_2020,cyclistic_bike_share_sept_2020)
View(cyclistic_bike_share_Q3_2020)
```


# 3. CLEANING Q3 DATA

I installed the 'skimr' and 'janitor' packages for data cleaning.
skimr helps to summarize data

```r
install.packages("skimr")
library("skimr")
```

janitor helps in data cleaning

```r
install.packages("janitor")
library("janitor")
```


## Inspect new Q3 table
```r
colnames(cyclistic_bike_share_Q3_2020)
```

## number of rows in Q3 
```r
nrow(cyclistic_bike_share_Q3_2020)
```

## dimensions of the Q3 dataframe
```r
dim(cyclistic_bike_share_Q3_2020)
```

## see list of columns and data types in q3 dataset

```r
str(cyclistic_bike_share_Q3_2020)
```


## Summary of Q3 data

```r
summary(cyclistic_bike_share_Q3_2020)
```

## Covert ride_length to numeric to run claculations on the data

```r
is.factor(cyclistic_bike_share_Q3_2020$ride_length)
```

```r
cyclistic_bike_share_Q3_2020$ride_length <-as.numeric(as.character(cyclistic_bike_share_Q3_2020$ride_length))
```

## To confirm if the ride_length column has changed to numeric run this code

```r
is.numeric(cyclistic_bike_share_Q3_2020$ride_length)
```

```r
cyclistic_bike_share_Q3_2020_2 <-cyclistic_bike_share_Q3_2020_complete = na.omit(cyclistic_bike_share_Q3_2020$ride_length)
```

## remove ride length with NA values and create new data frame

Install the tidyr package to help remove NA values in the ride_length column

```r
install.packages("tidyr")
library("tidyr")
```

```r
cyclistic_bike_share_Q3_2020_2 <- cyclistic_bike_share_Q3_2020 %>% drop_na(ride_length)
```


## arrange Q3 data in ascending order based on the time the ride started.

```r
library("dplyr")
```

```r
cyclistic_bike_share_Q3_2020_2 <- arrange(cyclistic_bike_share_Q3_2020_2, started_at)
```


# 4. DATA ANALYSIS

## mean ride_length (all figures in seconds)

```r
mean(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**1587.013**

## average ride length in seconds
```r
median(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**953**

## longest ride in seconds 
```r
max(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**86389**

## Shortest ride in seconds ignoring the missing values

```r
min(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**0**

## compare member and casual riders

```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual, FUN =mean)
```

```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual, FUN = median)
```
```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual, FUN = max)
```
```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual, FUN = max)
```

## Average ride time for members and casual riders by day of the week

```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual+cyclistic_bike_share_Q3_2020_2$day_of_week, FUN = mean)
```


## Export Q3 dataset to desktop folder (csv files) for further analysis and visualizations in Tableau 

```r
write.csv(cyclistic_bike_share_Q3_2020_2, "C:\\Users\\emily\\Desktop\\csv files\\cyclistic_bike_share_Q3_2020.csv", row.names = FALSE)
```

# 5. VISUALIZATIONS.

I created vusualizations in Tableau.

1. line graph showing number of rides 
![No  of rides for users by day of the week](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/b31386ec-7615-4ec4-a286-56aa908d5052)



![avg  ride length by day of week](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/c8b29d09-16be-4dcc-ab21-29b9290fa69f)


![avg  ride length for users](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/74238cb4-167e-4cf0-8c7e-4b0a88d40af9)





