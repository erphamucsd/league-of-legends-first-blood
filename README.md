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
