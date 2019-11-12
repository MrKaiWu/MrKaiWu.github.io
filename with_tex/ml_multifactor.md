## Machine Learning for Multifactor Stock Selection

This project is part of an exploratory research during my internship at one securities company in China. For ethical considerations, I will not go into great details. Instead I am going to briefly decribe the task and list several empirical findings that I personally think are quite interesting. This project is mostly done by myself and this article only represents my personal point of view.

### Task
I was given a set of stock factors that were proved to be efficient in China A-share market. My task was to explore whether machine learning algorithms could incorprate all these factors into stock selection in a way that constantly outperformed conventional methods. It was basically a multifactor stock selection problem that employed machine learning to add a flavor of non-linearity.

### Approach
- Basic framework: The stock selection was performed on a walk-forward basis. At each rebalance date, a new machine learning model was trained to incorporate the latest data. The model was then used to score the entire universe of stocks, and the top ranked stocks and their corresponding weights were saved. Finally, the stocks and weights were fed into a portfolio backtesting program. 
- Machine Learning configurations:
  - Learning algorithm: I used LightGBM as the library for boosted trees algorithm. I also used vanilla feed-forward neural networks and neural networks with LSTM layers. I implemented the neural networks in PyTorch.
  - Learning objective: The most typical learning objectives are regression and classification. Both fit into my task and are also supported by all three algorithms. LightGBM also supports "learning to rank", which LambdaRank uses the normalized discounted cumulative gain as the loss function instead of the negative log likelihood and mean square error, and is typically applied in information retrieval. 
  - Labeling: the labelling scheme I chose depended on the learning objective. For a regression task, the label for a stock was its future return. For a classification task, typically the best-performed and worst performed stocks were labeled as positive and negative, and the stocks in between were omitted to increase the signal-to-noise ratio. For a ranking task, stocks were first sorted by their future returns and then divided into several groups.
  - Dataset: The dataset as the input to the learning algorithm was the features(stock factors) data aligned with the label data. Features were available daily, but there was a resampling involved. For example, if when labeling the future return used was five-day return, then the data were included every five days. Regarding the choice of dataset, I identified two factors that had material impact on the outcome. The first was the length of time window, or the periods of historical data. While a longer window guaranteed sufficient training data and reduced overfitting, a shorter window enabled the model to better respond to potential market regime shift. The second was the subsample used, which was more relevant to classification tasks. To illustrate, we can keep the top 20% and the bottom 20% performed stocks each day. We can also keep the top 20% performed stocks and randomly select another 20% of stocks.
  - Training and validating: When training, a validation set was used for performance evaluation, as well as hyperparameters tuning when necessary. While hyperparameter does matter a lot, in my project it was not the main focus unless I found the loss in validation set barely declined.
- Benchmark:
  - I was given more than 70 stock factors, and the most simple linear model tended to perform poorly with so many independent variables. Hence I added the L1 regularization to introduce some sparsity. All else equal, I used linear model instead of machine learning algorithms to repeat the train-validation-predict process, and used the corresponding backtest performance as a benchmark. 
  - That being said, the backtesting module used several major equity indexes as benchmark.
  
 


### Findings
