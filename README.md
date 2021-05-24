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

Stick around for data from 2021!
