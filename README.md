# league-of-legends-first-blood
This is a project on 2022 League of Legends pro play data for DSC 80 at UCSD.

## Introduction
In the game League of Legends, a 5 versus 5 Multiplayer Online Battle Arena 
(MOBA) game, the very first kill of the game is called first blood. Though it is
only the first kill out of many, first blood has the potential to have a huge
impact on the game because it can allow a player to snowball against their lane 
opponent (opposing player that is the same position as them), and lead their 
team to a win.

As such, it would be interesting to investigate the following: "Are Players who
Draw 'First Blood' More Likely to Win Lane, and Consequently Game? 

By looking more carefully at the difference in gold, xp, and kills at various 
points in the game, we can gain a better sense of how players fare after drawing
first blood versus those that have not drawn first blood.

All analyses are made on an esports dataset of 2022 pro play data with 149400 
rows of data and 123 different columns of data, with a particular focus on these
columns:

| Column Name | Description |
|-------------|-------------|
| firstbloodkill | Indicates whether or not a player was the one to draw first blood. |
| position | Indicates what role a player played during that game. |
| result | Indicates whether or not a player won the game. |
| killsat10 | The number of kills that a player has at the 10 minute mark. | 
| killsat15 | The number of kills that a player has at the 15 minute mark. |
| kills | The number of kills that a player has by the end of the game. |
| xpdiffat10 | The difference in xp between a player and their lane opponent at the 10 minute mark. |
| xpdiffat15 | The difference in xp between a player and their lane opponent at the 15 minute mark. |
| golddiffat15 | The difference in gold between a player and their lane opponent at the 10 minute mark. |
| golddiffat15 | The difference in gold between a player and their lane opponent at the 15 minute mark. |
| earnedgold | The amount of gold that a player earned without including the passive income. |

## Cleaning and EDA
### Data Cleaning
In order to prepare the data for analysis, several steps were taken to clean the
data properly. The first step was to convert all of the 0/1 columns to boolean
columns for easier querying. This was especially useful since the firstbloodkill
column was originally a 0/1 column. Changing this column and all other similar
columns allows for quick querying. In order to preserve the NaN values, these 
steps were taken:
1.  Define a function to return True, False, or NaN depending on the input given.
2.  For each column, do an inplace assignment with the defined function applied to the original column.
3.  Final result should be a column with all 1s as True, all 0s as False, and NaNs unmodified.

The final step of cleaning was to remove the lines of data that contained the 
summary for each team. The data was generated in such a way that the first 10 
rows of data are for each individual player that played in the given match, and 
the last two rows are the respective summary data for both teams. This 
information was not particularly useful for my analysis, and not removing them 
would have introduced several NaN values that simply are not needed for 
analysis. By removing the summary rows, we can also drop several other columns
that contain data only for the summary rows. In order to do this:
1.  Identify columns in which the value is NaN for only summary rows, and a regular value for player rows. In this case, "playername" was used.
2.  Querying the data for all rows in which "playername" is not a NaN value.
3.  Drop columns for which every single value is a NaN value within the queried DataFrame.

### Univariate Analysis
<iframe src="assets/killsat10bar.html" width=800 height=600 frameBorder=0></iframe>

## Assessment of Missingness

## Hypothesis Test