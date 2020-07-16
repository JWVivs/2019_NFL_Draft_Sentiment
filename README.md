# Project Proposal
### John Viviani
**Statement of Work**

Develop a tool capable of predicting which prospect an NFL team will draft in the 2019 NFL Draft. This tool will use Twitter sentiment analysis through Twitter's API as a means of building a predictive model. The model will be updated regularly leading up to the draft day (April 25th) to account for new tweets which may influence where prospects are predicted to get drafted.

All work will be completed using the statistical programming language R.

This tool will be currently limited to the first 5 picks in the 2019 NFL Draft, as accurate prediction of the first round is generally difficult, along with the fact that I am not accounting for potential trades during the draft.

**Problem Statement**

Mock drafts remain as an enticing tool for NFL analysts to try and accurately predict the outcome of the NFL draft. If twitter sentiment analysis yields success in predicting where a prospect may land, this would provide analysts with a tool to better improve their mock drafts, which in turn would grant them a larger following for their success. This could be potentially used by NFL owners as a means of better preparing their draft board if the model predicts a player of interest will get drafted by another team.

**Techical Objectives**

*Environment:* R via R Studio

*Data Sources:* Twitter

*Workflow:* Microsoft Team DS

The final output will be an r-script or markdown that uses a prebuilt model to predict which prospect the Buccaneers (5th overall pick) will select in the draft.

**Technical Challenges**

The performance of the model.

**Technical Approach**

The model will employ the use of Twitter sentiment analysis to predict which prospect the Buccaneers will draft.
The first 4 picks of the 2019 NFL Draft will serve as the training data to predict who the 5th team (Buccaneers) will pick.
