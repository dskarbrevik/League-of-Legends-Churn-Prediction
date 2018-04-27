# Riot Churn Prediction

Two part project to model prediction of "churn" in new players. Uses <a href="https://developer.riotgames.com/">Riot's API</a> as data source and the <a href="http://cassiopeia.readthedocs.io">Cassiopeia</a> library to access that API.

More specifically, Riot's League of Legends (while free to play) requires you reach "level 3" as a player before facing human opponents (the real meat of the game). There may be a wide variety of reasons why a new player would stop playing before reaching level 3 (e.g. smurf account, doesn't see appeal of game, frustrated by game's learning curve, etc.). However there is no obvious label for things like smurf/not-smurf and such so I'm starting with a naive view that most players that don't get to level 3 just got bored/fed-up with the game for one reason or another. 

If I can predict which players are going to give up on the game before reaching level 3 based on their performance in their first match, it may be worth incentivizing that player in some way before they give up (either encouraging them to play again or providing some additional game info where they seem to be weakest).

**So that's my goal: predict, from the performance of a player's first match, if that player is going to make it to level 3.**

Or put more simply... "will this player get past the bot matches".

## Part 1: <a href="https://nbviewer.jupyter.org/github/dskarbrevik/Riot_Churn_Prediction/blob/master/Riot%20Churn%20Predictor%20%5BPart%201%20-%20Data%20Collection%5D.ipynb">Data Collection</a>

As stated above, I used the Cassiopeia Python library to access Riot's API. Unfortunately, I did not see any easy way to get summoner ids for new (low level) players. So my strategy was to make a new account, play one of the "tutorial" bot matches, and use the match history from my human teammates to branch out and build a list of new player ids.

## Part 2: <a href="https://nbviewer.jupyter.org/github/dskarbrevik/Riot_Churn_Prediction/blob/master/Riot%20Churn%20Predictor%20%5BPart%202%20-%20Data%20Cleaning%20and%20Modeling%5D.ipynb">Data Cleaning and Modeling</a>

Here I do basic data manipulation to prepare the dataset for basic ML models (remove duplicate rows, OHE categorical variables, etc.). I then use a few simple models (logistic regression, random forests) getting an accuracy score of around 74%. Lastly, I train a simple neural network on the dataset (still in progress!). 
