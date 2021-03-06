eda
================
John Viviani
April 28, 2019

-   [Cardinals Sentiment](#cardinals-sentiment)
-   [49ers Sentiment](#ers-sentiment)
-   [Jets Sentiment](#jets-sentiment)
-   [Raiders Sentiment](#raiders-sentiment)

Reading in the processed data for EDA.

``` r
nfl_processed_data <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/nfltweets_df_processed.rds")
```

From the processed NFL data, I filtered for each team in order to plot the sentiment analysis for each team and respective player. The x-axis contains the ten player names, whereas the y-axis measures the value of the sentiment being measured. I removed the 'team' and 'drafted' columns by using select(-()), and used gather(emotion, values, -player\_name) in order to generate a sentiment value (y-axis) that is measured across each player (x-axis). A key was created based off of the available emotions from sentiment analysis. ggplot() was used to plot the data, while geom\_line() connected the observations in order of player\_name. In order to make the x-axis legible, I used theme(axis.text.x = element\_text(angle = 90, hjust = 1)) in order to rotate the text on the x-axis to a 90 degree angle. labs() allowed me to edit the names of the axes, and ggtitle() was used to create the title for each plot.

Cardinals Sentiment
-------------------

The Cardinals have Kyler Murray, Quinnen Williams, and Rashan Gary at the same positive sentiment of 0.26. Murray seems appropriate to be one of the higher placed players here since the Cardinals have been interested in him at quarterback.

``` r
cardinals <- nfl_processed_data %>%
  filter(team =="Cardinals")

cardinalssentiment <- cardinals %>%
  select(-(team)) %>%
  select(-(drafted)) %>%
  gather(emotion, values, -player_name) %>%
  ggplot(data = .) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) + 
  geom_line(aes(x = player_name, y = values, group = emotion, color = emotion)) + 
  labs(x = "Players", y = "Value") + 
  ggtitle("2019 NFL Draft: Cardinals Sentiment")

plot(cardinalssentiment)
```

![](eda_files/figure-markdown_github/unnamed-chunk-2-1.png)

49ers Sentiment
---------------

Ed Oliver had the highest positive sentiment for the 49ers (0.27), whereas Nick Bosa and Kyler Murray had the lowest (0.20). These were also the lowest overall positive sentiment values in the dataset. It's interesting that Bosa was the least positive considering he was a favorite to be selected to the 49ers. Not as surprising to see Murray alongside him since the 49ers aren't in the market for a quarterback.

``` r
niners <- nfl_processed_data %>%
  filter(team =="49ers")

ninerssentiment <- niners %>%
  select(-(team)) %>%
  select(-(drafted)) %>%
  gather(emotion, values, -player_name) %>%
  ggplot(data = .) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) + 
  geom_line(aes(x = player_name, y = values, group = emotion, color = emotion)) + 
  labs(x = "Players", y = "Value") + 
  ggtitle("2019 NFL Draft: 49ers Sentiment")

plot(ninerssentiment)
```

![](eda_files/figure-markdown_github/unnamed-chunk-3-1.png)

Jets Sentiment
--------------

Ed Oliver had the highest positive sentiment for the Jets (0.27), which is understandable since the Jets are in the market for a defensive tackle. Ironically, Quinnen Williams had a lower positive sentiment (0.23), despite being the higher rated prospect and eventual draft pick of the Jets.

``` r
jets <- nfl_processed_data %>%
  filter(team =="Jets")

jetssentiment <- jets %>%
  select(-(team)) %>%
  select(-(drafted)) %>%
  gather(emotion, values, -player_name) %>%
  ggplot(data = .) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) + 
  geom_line(aes(x = player_name, y = values, group = emotion, color = emotion)) + 
  labs(x = "Players", y = "Value") + 
  ggtitle("2019 NFL Draft: Jets Sentiment")

plot(jetssentiment)
```

![](eda_files/figure-markdown_github/unnamed-chunk-4-1.png)

Raiders Sentiment
-----------------

The Raiders were an interesting scenario because they didn't draft any of the ten prospects listed (opted for Clelin Ferrell). However, Ed Oliver once again had the highest rated positive sentiment (0.32), which isn't too surprising since the Raiders needed to address their defensive line. This was also the highest overall positive sentiment value in the dataset. Dwayne Haskins was behind Oliver at 0.30 positive sentiment, which may be attributed to the rumors across Twitter nearing draft day that he may be selected.

``` r
raiders <- nfl_processed_data %>%
  filter(team =="Raiders")

raiderssentiment <- raiders %>%
  select(-(team)) %>%
  select(-(drafted)) %>%
  gather(emotion, values, -player_name) %>%
  ggplot(data = .) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) + 
  geom_line(aes(x = player_name, y = values, group = emotion, color = emotion)) + 
  labs(x = "Players", y = "Value") + 
  ggtitle("2019 NFL Draft: Raiders Sentiment")

plot(raiderssentiment)
```

![](eda_files/figure-markdown_github/unnamed-chunk-5-1.png)
