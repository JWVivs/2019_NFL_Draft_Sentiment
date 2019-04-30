draft\_data\_prep\_historical
================
John Viviani
April 9, 2019

``` r
# These are the required packages to run the rmd
required.packages <- c("tidyverse", "rtweet", "syuzhet", "lubridate", "glmnet", "broom", "tidypredict", "purrr", "caret", "car", "rsample")
# This will check if any of the required packages are not installed and install them.
if(!all(required.packages %in% installed.packages())){
  
  # packages that are NOT installed that are required to run document
  missing.packages <- setdiff(required.packages, installed.packages())
  
  install.packages(missing.packages)
  
  } else{
  
    print("You have all the required packages. Proceeded.")
  
  }
```

    ## [1] "You have all the required packages. Proceeded."

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.5.3

    ## -- Attaching packages ------------------------------------------------------------------------------------------------------------------------ tidyverse 1.2.1 --

    ## v ggplot2 3.1.0     v purrr   0.2.5
    ## v tibble  2.0.1     v dplyr   0.7.8
    ## v tidyr   0.8.2     v stringr 1.3.1
    ## v readr   1.3.1     v forcats 0.3.0

    ## -- Conflicts --------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rtweet)
```

    ## Warning: package 'rtweet' was built under R version 3.5.3

    ## 
    ## Attaching package: 'rtweet'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     flatten

``` r
library(syuzhet)
```

    ## Warning: package 'syuzhet' was built under R version 3.5.3

    ## 
    ## Attaching package: 'syuzhet'

    ## The following object is masked from 'package:rtweet':
    ## 
    ##     get_tokens

``` r
library(lubridate)
```

    ## Warning: package 'lubridate' was built under R version 3.5.3

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
library(glmnet)
```

    ## Warning: package 'glmnet' was built under R version 3.5.3

    ## Loading required package: Matrix

    ## 
    ## Attaching package: 'Matrix'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     expand

    ## Loading required package: foreach

    ## 
    ## Attaching package: 'foreach'

    ## The following objects are masked from 'package:purrr':
    ## 
    ##     accumulate, when

    ## Loaded glmnet 2.0-16

``` r
library(purrr)
library(caret)
```

    ## Loading required package: lattice

    ## 
    ## Attaching package: 'caret'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     lift

``` r
library(car)
```

    ## Loading required package: carData

    ## 
    ## Attaching package: 'car'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode

    ## The following object is masked from 'package:purrr':
    ## 
    ##     some

``` r
library(rsample)
```

    ## Warning: package 'rsample' was built under R version 3.5.3

Reading in the raw data to be processed.

Selecting column of interest (text) and grouping by player\_name and the respective team. Adding the sums of sentiment analysis for each player and respective team, providing us with 40 rows in total (later end up filtering out the 41st observation).

Converting to row wise percentages, must ungroup prior. mutate\_if function will convert rows 3 through 12 (emotions) to percentage. Mutating a drafted column with all 0s, then adding 1s for the first 3 teams based on the prospect they drafted. Removing sentiment\_sum column at the end.

Saved to processed folder as "nfl.sentiment\_20190425.rds".

nfltweetsbucs\_20190425 is the raw data consisting of only the Buccaneers and respective ten players. Same functions were applied as explained above (drafted column only has ten 0s because the data frame has ten players).

``` r
## CLEANING/PROCESSING RAW DATA

nfltweets_20190425 <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/raw/nfltweets_20190425.rds")

nfltweetsbucs_20190425 <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/raw/nfltweetsbucs_20190425.rds")

nfl.sentiment_20190425 <- get_nrc_sentiment(nfltweets_20190425$text) %>% 
  bind_cols(nfltweets_20190425) %>% 
  select(-(source:profile_image_url)) %>%
  select(-(user_id:screen_name)) %>%
  select(-(text)) %>%
  group_by(team, player_name) %>%
  summarise_all(., sum)

nfl.sentiment_20190425 <- nfl.sentiment_20190425 %>%
  ungroup(.) %>%
  mutate(seni_sum = rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, function(x) x/rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, round, digits = 2) %>%
  mutate(drafted = c(0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0)) %>%
  select(-(seni_sum)) 

saveRDS(nfl.sentiment_20190425, "C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfl.sentiment_20190425.rds")

nfl.sentimentbucs_20190425 <- get_nrc_sentiment(nfltweetsbucs_20190425$text) %>% 
  bind_cols(nfltweetsbucs_20190425) %>% 
  select(-(source:profile_image_url)) %>%
  select(-(user_id:screen_name)) %>%
  select(-(text)) %>%
  group_by(team, player_name) %>%
  summarise_all(., sum)

nfl.sentimentbucs_20190425 <- nfl.sentimentbucs_20190425 %>%
  ungroup(.) %>%
  mutate(seni_sum = rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, function(x) x/rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, round, digits = 2) %>%
  mutate(drafted = c(0, 0, 0, 0, 0, 0, 0, 0, 0, 0)) %>%
  select(-(seni_sum))

saveRDS(nfl.sentimentbucs_20190425, "C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfl.sentimentbucs_20190425.rds")
```

nfltweets.list is a list containing 41 tibbles consisting of each team and respective player that may be drafted (10 available), leaving us with 40 tibbles (41st was excluded because the Buccaneers are not a part of the training data).

``` r
nfltweets.list <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/raw/nfltweets.list.rds")
```

Applying nrc sentiment analysis to every element of the list. map\_df will apply a function to each element in the list. In this case, I am having it apply the get\_nrc\_sentiment command in order get the sentiment analysis for each of the 40 tibbles inside the list. It will be assigned to the vector "nfl.sentiment\_20190425map".

``` r
nfl.sentiment_20190425map <- nfltweets.list %>%
  map_df(., function(x)get_nrc_sentiment(char_v=x$text)) 
```

Creating a data frame from "nfl.sentiment\_20190425map", whereby it will be bound into one data frame, and grouped by the player name and respective team. Columns 3 through 12 consist of the emotions from sentiment analysis, which will be summed, averaged, rounded, and then converted to row wise percentages. A column named "drafted" will be added which shows where the first four picks actually landed by using 0s and 1s. Murray was drafted by the Cardinals, Bosa was drafted by the 49ers, Williams was drafted by the Jets, and a player which was not in the list of players searched for was drafted by the Raiders (Ferrell), so they contain ten 0s in their drafted column. seni\_sum column was removed at the end.

``` r
nfltweets_df <- nfl.sentiment_20190425map %>%
  bind_cols(., nfltweets_20190425 %>%
                select(player_name, team)) %>% 
  group_by(player_name, team) %>%
  summarise_if(is.numeric, sum) %>%
  ungroup(.) %>%
  mutate(seni_sum = rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, function(x) x/rowSums(.[3:12])) %>%
  mutate_if(., is.numeric, round, digits = 2) %>%
  mutate(drafted = c(1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0)) %>%
  select(-(seni_sum))
```

nfltweets\_df is filtered to exclude the Buccaneers observation (mentioned earlier), and saved into the processed folder as "nfltweets\_df\_processed.rds".

``` r
nfltweets_df %>%
  filter(team != "Buccaneers") %>% 
  saveRDS(., "C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfltweets_df_processed.rds")
```

Creating another processed data frame whereby the 'team' and 'player\_name' columns are numeric instead of factors (necessary for building model). Completed by reading in the recently saved processed data (nfltweets\_df\_processed) and transforming the columns to numeric. Saved the resulting data frames as 'newnfl\_processed\_data' and 'newbucs\_processed\_data'.

``` r
nfl_processed_data <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfltweets_df_processed.rds")

## Converting team and player_name columns to numeric
newnfl_processed_data <- nfl_processed_data %>%
  transform(., team = as.numeric(team)) %>%
  transform(., player_name = as.numeric(player_name))

saveRDS(newnfl_processed_data, "C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/newnfl_processed_data.rds")


bucs_processed_data <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfl.sentimentbucs_20190425.rds") 

## Converting team and player_name columns to numeric
newbucs_processed_data <- bucs_processed_data %>%
  transform(., team = as.numeric(team)) %>%
  transform(., player_name = as.numeric(player_name))

saveRDS(newbucs_processed_data, "C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/newbucs_processed_data.rds")
```
