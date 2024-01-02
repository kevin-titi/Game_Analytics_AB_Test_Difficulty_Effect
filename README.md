# Game Analytics: A/B Testing the Impact of a Game Difficulty Variation
(There is no dataset provided. I only uploaded the code I used as well as the result. Please take a look at the attached PDF for a full presentation.)

This project is an A/B testing for the impact of a game difficulty variation. I have statistically tested for the difference between groups of two in-game metrics, GameExp, and Quest Completion Rate. By doing so, I can test for causation whether the difficulty adjustment creates a difference for players in the game.

## Randomly Assigning Players into Two Groups
Players were randomly assigned into two smaller groups, a control and a difficulty-adjusted group. Randomness will ensure that the two groups will have statistically the same characteristics. Next, I adjusted the game difficulty only for the difficulty-adjusted group. Finally, I conducted a two-sample t-test for two variables, GameExp, and Quest Completion Rate.

![Game_Analytics_AB_Test_Difficulty_Effect - Github](https://github.com/kk-chaiyapuk/Game_Analytics_AB_Test_Difficulty_Effect/assets/82194433/cb106589-8088-47ac-a0db-b892a8949bd9)

## Testing for the statistically difference

Apparently, the adjustment seems to make the game easier as indicated by higher GameExp and Quest Completion Rate.

| Group | GameExp | Quest Completion Rate
------------- | ------------- | ------------- 
A Group (Control) | -0.9754 | 48.2%
B Group (Difficulty-adjusted) | +0.8860 | 52.4%

### GameExp
```python
tstat, pvalue, df = ttest_ind(hw3_data_abtest[hw3_data_abtest["Test Group"]==1]["GameExp"],
                        hw3_data_abtest[hw3_data_abtest["Test Group"]==2]["GameExp"],
                        alternative = 'two-sided',
                        usevar = 'unequal', # I assumed that the change in difficulty may change the deviation with GameExp
                        value = 0)

print(f"tstat: {tstat}")
print(f"pvalue: {pvalue}")
print(f"df: {df}")
```
tstat: -4.345537266490658

pvalue: 7.006208793317454e-05

df: 48.88318812366832
