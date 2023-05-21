# Analysis On First Blood's Role in Winning Games at the Pro Play Level
Quantifying the advantage given by taking the first kill in a game.

## Introduction
In the game League of Legends, a 5 versus 5 Multiplayer Online Battle Arena 
(MOBA) game, the very first kill of the game is called first blood. Though it is
only the first kill out of many, first blood has the potential to have a huge
impact on the game because it can allow a player to snowball against their lane 
opponent (opposing player that is the same position as them), and lead their 
team to a win.

As such, it would be interesting to investigate the following: "Are Players who
Draw 'First Blood' More Likely to Win their Lane, and Consequently the Game? 

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

The second step of cleaning was to remove the lines of data that contained the 
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

The last step taken was to query only the columns used in the analyses to allow 
for easier analysis and quicker viewing. This resulted in a cleaned dataset 
ready for analysis.
'print(player_data.head().to_markdown(index=False))'
Figure 1: Preview of the cleaned data.

### Univariate Analysis
#### The Importance of First Blood
In order to answer the question above, one of the most important things to 
first understand is the importance of a single kill. In pro play, kills are 
extremely hard to come by. The progression of number of kills during a game 
hardly changes, which means that a single kill earlyn on in the game often tips 
the balance of the scale in one direction or the other.

<iframe src="assets/killsat10bar.html" width=800 height=600 frameBorder=0></iframe>
Figure 2: The distribution of kills at the 10 minute mark.

<iframe src="assets/killsbar.html" width=800 height=600 frameBorder=0></iframe>
Figure 3: The distribution of kills at the end of a game.

Examining the figures, we can see that around 40% of all players had end the 
game with having 1 kill and below. Having a kill, let alone the very first one 
of the game is extremely impactful.

### Bivariate Analysis
#### The Advantage of Drawing First Blood
So what exactly does a player gain by drawing first blood in a match? When a 
player gets a kill in league of legends, the player is rewarded with a certain
amount of xp and gold as calculated by the game. When a player draws first blood
they gain a large advantage in terms of xp and gold over their opponent which 
allows them to snowball, or push their lead, even more against the opposing
team.

<iframe src="assets/killshist.html" width=800 height=600 frameBorder=0></iframe>
Figure 4: The distribution of kills at the end of a gameconditioned on whether 
or not a player drew first blood.

<iframe src="assets/earnedgoldhist.html" width=800 height=600 frameBorder=0></iframe>
Figure 5: The distribution of gold earned conditioned on whether or not a player 
drew first blood.

The difference in distribution between the two is marked, especially for the 
gold earned distribution. Players who draw the first blood of the game typically 
end the game ahead in terms of gold earned, meaning that they are able to 
utilize their early lead to widen the gap between them and their lane opponent.

### Interesting Aggregates
#### Assessing the Advantage of First Blood Among Different Positions
Taking a quick detour from our question, it would be interesting to take a look
at how different positions are able to utilize the advantage of drawing first 
blood. As it turns out, positions that are in solo lanes are able to capitalize
on the advantage drawing first blood and are able to extend it further, while
the jungler and support often benefit less from this. 

#### Average Gold Difference at 15 Minutes
'print(table_golddiffat15.to_markdown(index=True))'
Figure 6: Average gold difference at 15 minutes by position played, conditioned 
on whether or not a player drew first blood.

#### Average xp Difference at 15 Minutes
'print(table_xpdiffat15.to_markdown(index=True))'
Figure 7: Average xp difference at 15 minutes by position played, conditioned 
on whether or not a player drew first blood.

#### Average Kills 
'print(table_kills.to_markdown(index=True))'
Figure 8: Average kills at the end of the game by position played, conditioned 
on whether or not a player drew first blood.

In particular, top, mid, and bot laners that drew first blood are, on average, 2 
kills ahead of their counterparts that did not draw first blood, which means 
that in addition to the first blood kill, they are able to capitalize on their 
early advantage and early another kill.

## Assessment of Missingness
### NMAR Analysis
After a somewhat thorough investigation of the dataset, there is sufficient 
reason to believe that "pentakills" column may be NMAR. The missingess of the 
pentakills column is markedly different for several other columns, including 
league affiliation, the games status as a playoff, and other columns related to 
data about the description of the game, instead of actual data collected from 
the game. Since the data is independently aggregated by a single person, it 
would be helpful to have data about the date and person from which it was 
collected. 

### Missingness Dependency
Futhering the investigation of "pentakills" missingness, it would be a good idea
to perform a permutation test on one of the columns mentioned above. In this 
particular case, the missingness of "pentakills" data and the game's designation 
as a playoff were compared using a permutation test.

<iframe src="assets/playoffsmissing.html" width=800 height=600 frameBorder=0></iframe>
Figure 9: Missingness of "pentakills" data, conditioned on whether or not the 
game was classified as a playoff or non-playoff game.

<iframe src="assets/playoffsmissingtvd.html" width=800 height=600 frameBorder=0></iframe>
Figure 10: The distribution of permutated test statistics versus the oberserved
test statistic for shuffled playoff labels. In this case, the chose test 
statistic was the TVD.

With an observed p-value of 0.00, we can reject the notion that the missingness
of "pentakills" data is MAR (or dependent) on "playoffs". This means that there 
is a good chance that the missingness of "pentakills" data is inherently tied to
the value of the "playoffs" column. This makes sense because data for playoffs
is more likely to be complete, given that playoffs are more important than
non-playoff games. More attention is paid to playoff games, so they might have
more complete data collected for them.

## Hypothesis Test
### Test Parameters
In order to investigate whether or not players that draw first blood are more
likely to win their lane and their game, we have:
- Null Hypothesis: Players that draw first blood are even in xp and gold difference compared to players that did not draw first blood. Players that draw first blood are equally likely to win games as they are to lose games.
- Alternative Hypothesis: Players that draw first blood will have a positive xp and gold difference compared to players that did not draw first blood. Players that draw first blood are more likely to win games than to lose games.
- Test Statistic: Total Variation Distance.
- Significance Level: Rejection at the 1% or 0.01 significance level.

#### xpdiffat15
<iframe src="assets/xpdiffat15hist.html" width=800 height=600 frameBorder=0></iframe>
Figure 11: The distribution of xp differences at 15 minutes conditioned on 
whether or not a player drew first blood.

<iframe src="assets/xpdiffat15tvd.html" width=800 height=600 frameBorder=0></iframe>
Figure 12: The distribution of permutated test statistics versus the oberserved
test statistic for shuffled xpdiffat15 labels. In this case, the chose test 
statistic was the TVD.

The p-value obtained for this particular permutation test was 0.00.

#### golddiffat15
<iframe src="assets/golddiffat15hist.html" width=800 height=600 frameBorder=0></iframe>
Figure 13: The distribution of gold differences at 15 minutes conditioned on 
whether or not a player drew first blood.

<iframe src="assets/golddiffat15tvd.html" width=800 height=600 frameBorder=0></iframe>
Figure 14: The distribution of permutated test statistics versus the oberserved
test statistic for shuffled golddiffat15 labels. In this case, the chose test 
statistic was the TVD.

The p-value obtained for this particular permutation test was 0.00.

#### result
<iframe src="assets/winratebar.html" width=800 height=600 frameBorder=0></iframe>
Figure 15: The winrate of games conditioned on whether or not a player drew 
first blood.

<iframe src="assets/winratetvd.html" width=800 height=600 frameBorder=0></iframe>
Figure 16: The distribution of permutated test statistics versus the oberserved
test statistic for shuffled result labels. In this case, the chose test 
statistic was the TVD.

The p-value obtained for this particular permutation test was 0.00.

### Conclusion
At the 1% significance level, we can reject the notion that players that draw
first blood will be equal in terms of xp and gold diff compared to players that
did not draw first blood. We can also reject the notion that players that draw
first blood will be equally likely to win or lose games compared to players that
did not draw first blood.

It seems safe to say that drawing first blood plays a role by giving a tangible 
advantage that propagates through the game and gives them a better than 50/50 
chance of winning games. Our final analysis shows that players that draw first 
blood often end up ahead in terms of xp and gold, and as such are more likely to 
win games. The advantage of first blood is extremely important in a high-stakes 
low-margin environment like that of a League of Legend pro play game, and can 
often decide the fate of games early on.