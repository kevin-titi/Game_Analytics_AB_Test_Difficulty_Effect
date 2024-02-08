# Game Analytics: A/B Testing the Impact of a Game Difficulty Variation
(There is no dataset provided. I only uploaded the code I used as well as the result. Please take a look at the attached Jupyter Notebook for detail and visualization.)

This project is an A/B test for the impact of a game difficulty variation. I have statistically tested the difference between groups of two in-game metrics, GameExp, and Quest Completion Rate. By doing so, I can test for causation whether the difficulty adjustment creates a difference for players in the game.

## Randomly Assigning Players into Two Groups
Players were randomly assigned into two smaller groups, a control and a difficulty-adjusted group. Randomness will ensure that the two groups will have statistically the same characteristics. Next, I adjusted the game difficulty only for the difficulty-adjusted group. Finally, I conducted a two-sample t-test for two variables, GameExp, and Quest Completion Rate.

![Game_Analytics_AB_Test_Difficulty_Effect - Github](https://github.com/kk-chaiyapuk/Game_Analytics_AB_Test_Difficulty_Effect/assets/82194433/cb106589-8088-47ac-a0db-b892a8949bd9)

## Testing for the statistical difference

Apparently, the adjustment seems to make the game easier as indicated by higher GameExp and Quest Completion Rate.

| Group | GameExp | Quest Completion Rate
------------- | ------------- | ------------- 
A Group (Control) | -0.9754 | 48.2%
B Group (Difficulty-adjusted) | +0.8860 | 52.4%

### GameExp: The change in difficulty made a difference
- Metrics: Average GameExp of each player
- Hypothesis: Expected change in GameExp between groups = 0.00 (No change)
- Confidence Level: 95%

Based on the central limit theorem, we assume the distribution of “the mean of each group” is normally distributed. With 70 samples, I used statsmodels to calculate statistical test results. **The change in difficulty made a difference for GameExp (p-value = 0.00007 < 0.05, statistically different).** The GameExp has been increased by 1.86 from -0.98 in a control group to 0.89 in a difficulty-adjusted group.

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
```
tstat: -4.345537266490658
pvalue: 0.00007006208793317454
df: 48.88318812366832
```

### Quest Completion Rate: The change in difficulty did NOT make a difference
- Metrics: Average quest completion rate of each player (not proportion)

We will not assume that quests taken are independent from each other and we will not treat this
as a proportion metric. Quest completion rate depends on how good each player’s status/skill in
the game is. Thus, we will aggregate the quest completion rate of each player first.

- Hypothesis: Expected change in quest completion rate being 0.00% (No change)
- Confidence Level: 95%

**The change in difficulty did NOT make a difference for Quest Completion Rate (p-value = 0.34 >= 0.05, not statistically different).** Assuming by null hypothesis, Quest Completion Rate is still 48.26%.

```python
tstat, pvalue, df = ttest_ind(hw3_data_abtest[hw3_data_abtest["Test Group"]==1]["Quest Completion Rate"],
                        hw3_data_abtest[hw3_data_abtest["Test Group"]==2]["Quest Completion Rate"],
                        alternative = 'two-sided',
                        usevar = 'unequal', # The change in difficulty may change the deviation with Quest Completion Rate
                        value = 0)

print(f"tstat: {tstat}")
print(f"pvalue: {pvalue}")
print(f"df: {df}")
```
```
tstat: -0.9603789025359427
pvalue: 0.3403256759199408
df: 66.8444658670575
```
