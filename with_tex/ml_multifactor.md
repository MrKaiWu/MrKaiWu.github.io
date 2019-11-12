## Machine Learning for Multifactor Stock Selection

This project is part of an exploratory research during my internship at one securities company in China. For ethical considerations, I will not go into great details. Instead I am going to briefly decribe the task and list several empirical findings that I personally think are quite interesting. This project is mostly done by myself and this article only represents my personal point of view.

### Task
I was given a set of stock factors that were proved to be efficient in China A-share market. My task was to explore whether machine learning algorithms could incoprate all these factors into stock selection in a way that constantly outperformed conventional methods. It was basically a multifactor stock selection problem that employed machine learning to add a flavor of non-linearity.

### Approach
- The basic framework: The stock selection was performed on a walk-forward basis. At each rebalance date, a new machine learning model was trained to incorporate the latest data. The model was then used to score the entire universe of stocks, and the top ranked stocks and their corresponding weights were saved. Finally, the stocks and weights were fed into a portfolio backtesting program. 
- Labeling: the labelling scheme I chose depended on the learning objective. For a regression task, the label for a stock was its future return. For a classification task, typically the best-performed and worst performed stocks were labeled as positive and negative, and the stocks in between were omitted to increase the signal-to-noise ratio. For a ranking task, stocks are first sorted by future return and then divided into several groups.
- Training and validating: When training, a validation set was used for performance evaluation, as well as hyperparameters tuning when necessary. I used a Time-series-friendly cross-validation rather than k-fold.
- 
- 

### Findings
