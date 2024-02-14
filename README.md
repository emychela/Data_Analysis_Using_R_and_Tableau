# CYCLISTIC_BIKE_SHARE_Q3_2020_ANALYSIS.

## Table of Contents
- [Introduction.](#introduction)
- [Business Task.](#business-task)
- [Install Required R Packages.](#install-required-r-packages)
- [Import Data.](#import-data)
- [Combine Q3 Data.](#combine-q3-data)
- [Data Cleaning.](#data-cleaning)
- [Data Analysis.](#data-analysis)
- [Export Q3 Dataframe.](#export-q3-dataframe)
- [Visualizations.](#visualizations)
- [Findings.](#findings)
- [Recommendations.](#recommendations)
- [Limitations.](#limitations)

  
## Introduction.

This analysis is a case study from the Google Data Analytics Certificate course. The datasets were made available by Motivate International Inc.
In this project, I will be merging datasets from July, August, and September 2020 to get a single dataset for Q3 2020, conduct simple analysis to help answer the key question "In what ways do member and casual riders use cyclistic bikes differently?"

I cleaned and performed some calculations of the datasets on Microsoft Excel, before exporting them to R i.e.
 - I dropped Columns representing the ‘starting latitude’, ‘ending latitude’ ‘starting longitude’ ‘ending longitude’ of rides were removed as they were not necessary for the business task.
 - 6,846 (July 1745, August 2769, September 2132) Records of data with negative ride_length was removed because they were taken out of the docks for quality checks.
 - I added 3 rows to help solve the business task i.e ‘ride_length’, ‘member_casual’, ‘day_of_week’.
   
## Business Task.
To understand in what ways, do members and casual riders use Cyclistic bikes differently. From these insights a new marketing strategy will be designed to convert casual riders into annual members.
Key stakeholders are Cyclistic company executive team.

## Install Required R Packages.

tidyverse is for data import and wrangling.

```r
install.packages("tidyverse")
library("tidyverse")
```

readr is used for reading rectangular data e.g. csv file.

```r
install.packages("readr")
library("readr")
```

ggplot is for visualizations.

```r
install.packages("ggplot2")
library(ggplot2) 
```

libridate for date functions

```r
install.packages("libridate")
library("libridate")
```

dplyr help complete some data manipulation tasks.

```r
install.packages("dplyr")
library("dplyr")
```

purr works with functions and vectors to help with coding.

```r
install.packages("purrr")
library("purrr")
```

## Select Working Directory.
 
I selected my working directory using the session tab.

getwd() will wisplay my working directory
```r
getwd()
```
## Import Data.

I imported csv cyclistic bike share Q3 data to global environment.
 
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


## Combine Q3 Data.

I inspected and changed data types on the files tab. I converted ride_id and rideable_type to character in all datasets so that they can stack correctly.

- Stacked cyclistic data from July, August, September 2020 to get Q3 data.
I used bind_rows function to merge the data from July,August, September 2020 to get Q3 dataframe.
       
```r
cyclistic_bike_share_Q3_2020 <- bind_rows(cyclistic_bike_share_july_2020,cyclistic_bike_share_august_2020,cyclistic_bike_share_sept_2020)
View(cyclistic_bike_share_Q3_2020)
```


## Data Cleaning.

I installed the 'skimr' and 'janitor' packages for data cleaning.
skimr helps to summarize data.

```r
install.packages("skimr")
library("skimr")
```

janitor helps in data cleaning.

```r
install.packages("janitor")
library("janitor")
```


Inspect new Q3 table.
```r
colnames(cyclistic_bike_share_Q3_2020)
```

Number of rows in Q3.
```r
nrow(cyclistic_bike_share_Q3_2020)
```

Dimensions of the Q3 dataframe.
```r
dim(cyclistic_bike_share_Q3_2020)
```

Check list of columns and data types in Q3 dataset.

```r
str(cyclistic_bike_share_Q3_2020)
```


Summary of Q3 data.

```r
summary(cyclistic_bike_share_Q3_2020)
```

Covert ride_length to numeric to run claculations on the data.

```r
is.factor(cyclistic_bike_share_Q3_2020$ride_length)
```

```r
cyclistic_bike_share_Q3_2020$ride_length <-as.numeric(as.character(cyclistic_bike_share_Q3_2020$ride_length))
```

To confirm if the ride_length column has changed to numeric run this code.

```r
is.numeric(cyclistic_bike_share_Q3_2020$ride_length)
```

```r
cyclistic_bike_share_Q3_2020_2 <-cyclistic_bike_share_Q3_2020_complete = na.omit(cyclistic_bike_share_Q3_2020$ride_length)
```

Remove ride length with NA values and create a new dataframe.

Install the tidyr package to help remove NA values in the ride_length column.

```r
install.packages("tidyr")
library("tidyr")
```

```r
cyclistic_bike_share_Q3_2020_2 <- cyclistic_bike_share_Q3_2020 %>% drop_na(ride_length)
```


Arrange Q3 data in ascending order based on the time the ride started.

```r
library("dplyr")
```

```r
cyclistic_bike_share_Q3_2020_2 <- arrange(cyclistic_bike_share_Q3_2020_2, started_at)
```


## Data Analysis.

Mean ride_length (all figures in seconds).

```r
mean(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**1587.013**

Average ride length in seconds.
```r
median(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**953**

Longest ride in seconds.
```r
max(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**86389**

Shortest ride in seconds ignoring the missing values.

```r
min(cyclistic_bike_share_Q3_2020_2$ride_length)
```
**0**

Compare member and casual riders.

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

Average ride time for members and casual riders by day of the week.

```r
aggregate(cyclistic_bike_share_Q3_2020_2$ride_length ~ cyclistic_bike_share_Q3_2020_2$member_casual+cyclistic_bike_share_Q3_2020_2$day_of_week, FUN = mean)
```


## Export Q3 Dataframe.

I exported Q3 dataframe to desktop folder (csv files) for further analysis and visualizations in Tableau. 

```r
write.csv(cyclistic_bike_share_Q3_2020_2, "C:\\Users\\emily\\Desktop\\csv files\\cyclistic_bike_share_Q3_2020.csv", row.names = FALSE)
```

## Visualizations.

I created visualizations in Tableau.

1. Bar graph showing total number of rides for members and casual riders.


![Total no  of rides](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/3a2a3a81-27ce-4ee2-8eff-9d519639a33c)


2. Line graph showing total number of rides for users by day of the week.

![No  of rides for users by day of the week](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/b31386ec-7615-4ec4-a286-56aa908d5052)


5. Line graph showing average ride length for members and casual riders by day of the week.

![avg  ride length by day of week](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/c8b29d09-16be-4dcc-ab21-29b9290fa69f)


4. Bar graph showing average ride length for members and casual riders.
 
![avg  ride length for users](https://github.com/emychela/Data_analysis_using_R_and_Tableau/assets/150371945/74238cb4-167e-4cf0-8c7e-4b0a88d40af9)


## Findings.

1. Data shows that in Q3, member riders had the highest number of rides 912,632 and casual riders had 786,003.
2. Casual riders used bikes for a lengthy period compared to member riders. The average ride length for casual riders was 2302 seconds i.e. 38 minutes:22 seconds, member riders had an average ride length of 971 seconds i.e. 16 minutes:11 seconds.
3. Casual riders had the highest number of rides on Saturday 176,201 rides.
4. Suprises the data revealed were:
    - Casual riders used bikes longer compared to member riders even though members had the highest number of rides.
    - Saturday had the highest number of rides 176,201 and this was from casual riders.
    - Member riders use bikes more on weekdays and casual riders use bikes more on weekends.
      
## Recommendations.


1. Targeted marketing should be done to casual riders. Casual riders are using the bikes for a lengthy period and marketing to this specific group, will have a higher chance of converting them to annual members.
2. There should be a membership subscription offer to casual riders who are interested in becoming annual members to attract more casual riders to get the annual membership.
3. A survey to be conducted targeting all Cyclistic customers, to understand their rates of satisfaction. This will be used to see the areas that need improvement to maintain the current customers and convert casual riders to annual members.

## Limitations.

1.  I removed Columns representing the ‘starting latitude’, ‘ending latitude’ ‘starting longitude’ ‘ending longitude’ as they were not necessary for the business task.
2. 6,846 (July 1745, August 2769, September 2132) Records of data with negative ride_length was removed because they were taken out of the docks for quality checks.








