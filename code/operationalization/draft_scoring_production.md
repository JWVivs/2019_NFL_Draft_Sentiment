draft\_scoring\_production
================
John Viviani
April 26, 2019

Reading in the model and the data which will be predicted (newbucs\_processed\_data).

``` r
nfl.model <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/models/nfl.model.rds")

newbucs_processed_data <- readRDS("C:/Users/John.JVivs/Documents/COLLEGE/GRAD SCHOOL/GDAT510/JohnV_FinalProject/data/processed/newbucs_processed_data.rds")
```

Function that will retrieve probabilities of being drafted for each of the prospects to the Buccaneers. Getting rid of 'drafted', 'team', and 'player\_name' columns. model.matrix is creating a matrix from the data frame provided by expanding factors to a set of dummy variables. predict() is making predictions based off of the nfl.model I provided, while using the lambda value that gives the minimum mean cross-validated error. Type is set to 'response' since I need a numeric result.

Generated probability of 7.5% for each prospect. Ideally, it would generate probabilities across all of the prospects, and the prospect with the highest probability that was not already drafted would be the player that the model predicts the Buccaneers will draft.

``` r
preds <- newbucs_processed_data %>% 
  select(-one_of("drafted", "team", "player_name")) %>% 
  model.matrix(~ ., data = .) %>% 
  predict(nfl.model, newx =  ., s = "lambda.min", type = "response")

## Buccaneers data frame whereby each prospect is denoted by a number (1 through 10)
preds
```

    ##        1
    ## 1  0.075
    ## 2  0.075
    ## 3  0.075
    ## 4  0.075
    ## 5  0.075
    ## 6  0.075
    ## 7  0.075
    ## 8  0.075
    ## 9  0.075
    ## 10 0.075
