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
 
 #2020
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

I had just taken a course in machine learning and  wanted to see if Neural Networks could handle this. The big question was: what would the predictive labels be? Binary: Champions vs Not? Categorical: Champions vs Finalists vs Semi-Finalists vs Quarter Finalists vs Qualifying vs Not? Numerical regression based on playoffs standings? Each of these had a number of negatives.

Any categorical data would be woefully imbalanced, with far more teams not winning than winning the whole thing. Numerical data sounds nice, but there's no definitive ranking of the teams in the playoffs after 1 and 2. I definitely didn't want to go through each bracket for 10 years determining which teams lost to the winning teams early onto come up with placings. We're already dealing with hundreds of lines in a spreadsheet, let's leave the manual editing to a minimum and give each team a 1 if they won the Stanley Cup and a 0 if they did not. As expected, this is going to go very poorly.

Stick around for NN results, Logistic Regression results, ensemble results, and data from 2021!
