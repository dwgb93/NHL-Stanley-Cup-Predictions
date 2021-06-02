# NHL-Stanley-Cup-Predictions
Using ML to predict Stanley Cup winners. 

All data was collected from: http://www.nhl.com/stats/teams, going back to 2010. I could have gone back further, I suppose, but I don't know if that will provide a noticable benefit. You're welcome to try though! Pull requests are open.

# 2019
This all started when my dad asked me to use regular season data to try and predict the Stanley Cup winner for a hockey pool. This was before I had taken an machine learning class, so the best idea seemed to be normalizing/standardizing the data in each category, then using hand-selected weights to predict playoff success. We chose to equally weight Goals Against Average (GAA) and the ratio of Takeaways to Giveaways (TA/GA), but you can also change the weights for Goals (G), Goals Against (GA), Goals per Game Played (G/GP), and Goal Differential (DIFF = G - GA), which will automatically update the weights throughout the spreadsheet.

Each column of data is standardized (0 to 1) and normalized ((data-mean)/stdev), which produces essentially (but not exactly) the same results.

## 2019 Results (with default weights on GAA and TA/GA)

| Rank | Team | Notes |
|------|------|-------|
| 1 | Vegas | |
| 2 | St. Louis | Stanley Cup Champions |
| 3 | Minnesota | |
| 4 | Carolina | Semi-Finals |
| 5 | Boston | Lost in Finals |
| 6 | Columbus | Quarter Finals |
| 7 | New York I | Quarter Finals |
| 7 | Arizona | |
| 9 | Dallas | Quarter Finals |
| 10 | Colorado | Quarter Finals |
| 11 | Tampa | |
| 12 | Nashville | |
| 13 | Pittsburgh | |
| 14 | Vancouver | | 
| 15 | Calgary | | 
| 15 | San Jose | Semi-Finals |
| 17 | Winnipeg | |
| 18 | Toronto | | 
| 19 | Washington | |
| 20 | New Jersey | |
| 21 | Montreal | |
| 22 | Anaheim | |
| 23 | Buffalo | |
| 24 | Edmonton | |
| 25 | Chicago | |
| 26 | New York R | |
| 27 | Los Angeles | |
| 28 | Florida | |
| 29 | Philadelphia | |
| 30 | Detroit | |
| 31 | Ottawa | |
 
 *Note: Arizona/New York Islanders and Calgary/San Jose's ranks were swapped depending on if standardization or normalization was used.*
 Overall, not the worst results in the world. 7 of the top 10 predictions would go on to the quarter finals or better, with the eventual winner being ranked 2nd! The biggest surprise was Minnesota being ranked 3rd despite not even qualifying for the playoffs.
 
 # 2020
 Same idea, I wanted to see if we could predict the Stanley Cup champions from regular season data, but now I had a little more experience under my belt.
 
 ## Baseline 1: Exactly the same as last year
 
| Rank | Team | Notes |
|------|------|-------|
| 1 | Colorado | Quarter Finals|
| 2 | St. Louis | | 
| 3 | Tampa | Stanley Cup Champion|
| 3 | Vegas | Semi-Finals |
| 5 | Boston | Quarter-Finals|
| 6 | Arizona | | 
| 7 | Columbus | | 
| 8 | Carolina | |
| 9 | Dallas | Lost in Finals | 
| 9 | Minnesota | |
| 11 | Pittsburgh | | 
| 12 | Philadelphia | Quarter Finals | 
| 12 | San Jose | | 
| 14 | Vancouver | Quarter Finals |
| 15 | Chicago | | 
| 16 | Winnipeg | | 
| 16 | Nashville | |
| 18 | Calgary | |
| 19 | Edmonton | |
| 20 | Washington | | 
| 21 | New York I | Semi-Finals | 
| 22 | Buffalo | | 
| 22 | Florida | |
| 24 | New Jersey | |
| 25 | Toronto | | 
| 26 | New York R | |
| 27 | Los Angeles  | |
| 28 | Montreal | |
| 29 | Anaheim | |
| 30 | Ottawa | | 
| 31 | Detroit | |

Once again, not the best in the world - in fact, it's a little worse - probably in part because of the messed up season and weird Qualifying Round of the playoffs. But we can do better.

I had just taken a course in machine learning and wanted to see if Neural Networks could handle this. The big question was: what would the predictive labels be? Binary: Champions vs Not? Categorical: Champions vs Finalists vs Semi-Finalists vs Quarter Finalists vs Qualifying vs Not? Numerical regression based on playoffs standings? Each of these had a number of negatives.

Any categorical data would be woefully imbalanced, with far more teams not winning than winning the whole thing. Numerical data sounds nice, but there's no definitive ranking of the teams in the playoffs after 1 and 2. I definitely didn't want to go through each bracket for 10 years determining which teams lost to the winning teams early onto come up with placings. We're already dealing with hundreds of lines in a spreadsheet, let's leave the manual editing to a minimum and give each team a 1 if they won the Stanley Cup and a 0 if they did not. As expected, this is going to go very poorly.

## Baseline 2: Neural Network
| Rank | Team | Notes |
|-----|------|-------|
| 1 | Colorado | Quarter Finals | 
| 2 | Carolina | | 
| 3 | Philadelphia | Quarter Finals | 
| 3 | Columbus | | 
| 5 | Pittsburgh| | 
| 6 | Florida | | 
| 7 | Los Angeles | | 
| 8 | Dallas | Lost in Finals |  
| 9 | Chicago | |
| 10 | St. Louis | |
| 11 | New York I | Semi-Finals | 
| 12 | Arizona | |  
| 13 | Vegas | Semi-Finals | 
| 14 | Anaheim | | 
| 15 | New Jersey | | 
| 16 | Nashville | | 
| 17 | Tampa | Stanley Cup Champion |
| 18 | Ottawa | |
| 19 | Winnipeg | |
| 20 | New York R | | 
| 21 | Washington | | 
| 22 | Boston | Quarter Finals | 
| 22 | San Jose | |
| 24 | Calgary | |
| 25 | Edmenton | | 
| 26 | Montreal | |
| 27 | Minnesota  | |
| 28 | Vancouver | Quarter Finals |
| 29 | Detroit | |
| 30 | Buffalo | | 
| 31 | Toronto | |

As expected: not great. I tried a few different paramter configurations - different numbers of layers, neurons, activations, Dropout - but nothing magically made the results better or more confident. In fact, with softmax activation, each team was given between a 3 and 4% probability of winning the Cup - which meant nearly 97% accuracy in the model, but no solid results. So I decided to try something else.

## Baseline 3: Logistic Regression
| Rank | Team | Notes |
|-----|------|-------|
| 1 | Boston | Quarter Finals| 
| 2 | Columbus | | 
| 3 | Colorado | Quarter-Finals | 
| 3 | Vegas | Semi-Finals | 
| 5 | Carolina | | 
| 6 | Philadelphia | Quarter-Finals | 
| 7 | St. Louis | | 
| 8 | Arizona | |
| 9 | Dallas | Lost in Finals |  
| 10 | Pittsburgh | |
| 11 | Montreal | | 
| 12 | Tampa | Stanley Cup Champions |  
| 13 | Nashville | | 
| 14 | Los Angeles | | 
| 15 | Washington | | 
| 16 | Chicago | | 
| 17 | Winnipeg | |
| 18 | New York I | Semi-Finals |
| 19 | Toronto | |
| 20 | Florida | | 
| 21 | Calgary | | 
| 22 | Vancouver | Quarter Finals | 
| 22 | New York R | |
| 24 | Minnesota | |
| 25 | Edmonton | | 
| 26 | New Jersey | |
| 27 | Buffalo  | |
| 28 | San Jose | |
| 29 | Anaheim | |
| 30 | Ottawa | | 
| 31 | Detroit | |

Again, not bad! Not great, but (maybe) better than before. This was using sci-kit learn's Logistic Regression function, with the L1 norm, again just predicting whether a team would win or not. Lastly, I had read that the single best predictor of playoff success from the regular season was Goals For, which was lead by Tampa (surprise surprise). So we combined everything together into an ensemble model, using the crappy predictions from each method combined to produce one final prediction.

## Final Ensemble
| Rank | Team | Notes |
|-----|------|-------|
| 1 | Colorado | Quarter Finals |  
| 2 | Carolina | | 
| 3 | Philadelphia | Quarter Finals |  
| 3 | St. Louis | | 
| 5 | Pittsburgh | | 
| 6 | Vegas | Semi-Finals | 
| 7 | Boston | Quarter Finals | 
| 8 | Columbus | |
| 9 | Tampa | Stanley Cup Champions | 
| 10 | Dallas | Lost in Finals |
| 11 | Florida | | 
| 12 | Arizona | |   
| 13 | Chicago | | 
| 14 | Nashvillle | | 
| 15 | Washington | | 
| 16 | Los Angeles | | 
| 17 | Winnipeg | |
| 18 | New York I | Semi-Finals | 
| 19 | New York R | |
| 20 | Minnesota | | 
| 20 | Montreal | | 
| 22 | Vancouver | Quarter Finals |  
| 22 | Toronto | |
| 24 | Calgary | |
| 25 | Edmonton | | 
| 26 | New Jersey | |
| 27 | Anaheim  | |
| 28 | San Jose | |
| 29 | Ottawa | |
| 30 | Buffalo | | 
| 31 | Detroit | |

When put together, it's actually a little better. 6 of the top 10 made it to the Quarter Finals or beyond. One thing to note is that all of the algorithms were consistently able to pick the biggest losers. The teams that did the worst in the regular season (cough *Detroit* cough) consistently ended up at the bottom of the pile, no matter which algorithm was used. So we're definitely onto something.

Come to think of it, *why* are we using the data from all 31 teams anyway?

# 2021
Time to get serious.

A lot of the decisions made previously were straddling the line between arbitrary and dumb. So let's think things through this year.

First, including all 31 teams in the analysis is useless when only 16 of them make it to the finals. The other 15 teams just serve to confound the model and add unnecessary complexity. Since we already know who's going to the finals, let's trim the rest.

Second, this is a very noisy sport, in multiple senses of the word. Data can only go so far in predicting a winner, because upsets happen surprisingly often. Having to choose between "wins Stanley Cup" or "anything else" doesn't account for dozens of possibilities that include late overtime wins, underdog victories, or poor play. Instead, we represent the data to predict in two different ways: regressiion and classification - with more nuance than before.

Regression: Instead of predicting "wins finals" (1) vs "anything else" (0), we use the percentage of series won. The team that wins the finals wins 100% of their series, even when they don't win every game. Quarter finalists win their first series but not their 2nd (0.5). Semi-finalists win their first two then lose in the third (0.66...) etc. However, I'm apparently bad at math, so used 0, 0.25, 0.5, 0.75, and lastly 1 for the Stanley Cup Champions. Close enough, I guess.

Classification: In the same vein, we can classify teams into one of 5 categories (a, b, c, d, e) based on what round they got eliminated in. They are still unbalanced, with 8 a's, 4 b's, 2 c's, 1 d (loses in finals), and 1 e (champions), but it's a lot better than before.

Using our new system, we apply the same three models from previous years.

## Baseline 1: Linear Regression
This time we use what we learned from last year with the largest weight (0.5) on Goals For, with equal weights (0.25) on Goals Against and Takeaways/Giveaways. Only the data from the 16 qualifying teams are used in the calculation of the averages for normalization.

## Baseline 2: Neural Network
Like last year, we use 2 hidden layers with 64 neurons each, ReLU activation, Dropout, and the Adam optimizer. We run for 100 epochs, but only save the model with the best validation accuracy. This time, we're regressing to the series data in [0,1] with MSE instead of the quantized {0,1} lose/win data. While this technically reduces accuracy (as measured by the percentage of teams correctly classified), it increases predictive power, as the network is better able to predict which teams perform better, even if they don't win the whole thing.

## Baseline 3: Logistic Regression
This time, we're using multiclass logistic regression, attempting to predict how far a team will progress - a through e. This time, we use the L2 norm with limited-memory BFGS instead of liblinear. Interestingly, since this model gives the odds for each category, in addition to predicing winners, it also predicts which teams are going to get eliminated in the first round, quarters, semi's, etc. 

So we can create a list of the biggest losers, who are most likely to be eliminated in round 1: Florida, Winnipeg, Nashville, Minnesota, New York I, Vegas, Montreal, Edmonton.

## Baseline Results
Linear Model     Neural Network    Logistic Regression
| Rank | Team | Rank | Team  | Rank | Team  |
|-----|------|-------|-----|------|-------|
| 1 | Boston | 1 | Boston | 1 | Boston |   
| 2 | Tampa	| 2 | St. Louis | 2 | Colorado |  
| 3 | Colorado | 3 | Carolina | 3 | Vegas |  
| 4 | Washington | 4 | New York I | 4 | Nashville |  
| 5 | St. Louis | 5 | Colorado | 5 | St. Louis |  
| 6 | Pittsburgh	| 6 | Washington | 6 | Carolina |  
| 6 | Toronto	| 7 | Tampa | 7 | Tampa | 
| 8 | Vegas | 8 | Pittsburgh | 8 | New York I | 
| 8 | Carolina	| 9 | Vegas | 9 | Toronto |  
| 10 | Winnipeg	| 10 | Minnesota | 10 | Washington | 
| 11 | Florida	| 11 | Toronto | 11 | Winnipeg |  
| 12 | Minnesota	| 12 | Edmonton | 12 | Pittsburgh |    
| 13 | Nashville	| 13 | Nashville | 13 | Montreal |  
| 14 | Edmonton	| 14 | Winnipeg | 14 | Florida |  
| 15 | Montreal | 15 | Florida	| 15 | Minnesota |  
| 16 | New York I	| 16 | Montreal	| 16 | Edmonton |  

Overall, there's a lot more agreement between the results, which is promising. Let's put it all together using the same ensemble method as last year.

## Final Ensemble
| Rank | Team | Notes (TBA) |
|-----|-----|-----|
| 1 | Boston | |  
| 2 | Colorado | | 
| 3 | St. Louis | | 
| 4 | Tampa | | 
| 5 | Carolina | | 
| 6 | Vegas | | 
| 7 | Washington | |
| 8 | Pittsburgh | |
| 9 | Toronto | | 
| 10 | New York I | |
| 11 | Nashville | | 
| 12 | Winnipeg | |   
| 13 | Minnesota | | 
| 14 | Florida | | 
| 15 | Edmonton | | 
| 16 | Montreal | | 

Now that's a model! At this point, I have no idea how it did, as I created it before the playoffs even started, but I can't wait to find out. One thing I haven't done in previous years is made a proper bracket. Generally, for any matchup, you'd expect the higher ranked (predicted) team to win. Given it's a best of 7 situation, the more games they play, the more likely they are to converge to the expected result. Using this method, let's make a bracket.

## Final Bracket
![My image](https://github.com/dwgb93/NHL-Stanley-Cup-Predictions/blob/main/2021/2021%20Stanley%20Cup%20Predictions.png?raw=True)

I didn't realize how the semi-finals worked. So it will be the winner of North Division vs West Division (seed 4 vs seed 1) and the winner of East Division vs Central Division (seed 3 vs seed 2), based on regular season standings. That means (if everythings works out), we'll have the top two model playing for the finals instead of in the semi-finals.




