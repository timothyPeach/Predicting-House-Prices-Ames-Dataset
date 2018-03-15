# Predicting-House-Prices-Ames-Dataset

So at General Assembly's Data Science Immersive program, one of our projects was to make an interpretable model that predicted
house prices using the Ames Iowa data available on kaggle and explain that model we made.  In that project, there was a
tradeoff we were trying to hit the sweet spot on regarding how interpretable is it versus how good is it at predicting.

Well, in this repo, I'll just be shooting for the most predictive black box that I can make without running anything overnight
or using AWS.

Here's the general workflow that ended up happening here:

1) Imported Data

2) Dealt With Missing Values
- most cases were trivial, but the column "LotFrontage" needed to be imputed, and this was done VERY WELL just by training a
RandomForestRegressor, and then using that RandomForestRegressor's feature_importances method to reduce the number of features 
and then predict "LotFrontage's" ~260 missing values.  Only 10% of the original correlation to the target "SalePrice" was lost
in the imputed values.  Very successful.

3) Cut Out Obviously Useless Columns

4) Cut More Columns
-Ran a RandomForestRegressor 100 times and accepted columns that had an importance of at least 1% even just in one of these
100 trials.

5) Predicted With Basic Model
-The average performance of a not-finely-tuned RandomForestRegressor was about 0.83 R-squared.

6) Analyzed How Model Was Predicting
-The distribution was definitely skewed toward lower prices, and so predicting log(price) and then reconverting your predictions
back at the end with exp() is much better.

7) Predicted log(price) With Basic Model
~0.85 R-squared with basic model

8) Predicted log(price) With TPOT
-TPOT is a library designed to automate everything after Data Cleaning and before Model Evaluation.  TPOT uses a genetic algorithm
to run through tons of combinations of everything in the book.  GridSearching different models and parameters, doing PCA, you name it.
~0.92 R-squared in Train/Test/Split after running TPOT for only 10 minutes.

9) Closing Brief Analysis of Model Performance / Saved Model
-The dataset is really small in terms of len(DataFrame) (only ~1450 usable rows) and the model still absolutely crushed it in
the predictions.  Where it especially shines is with lower house prices, but doesn't do so well with the higher prices.  This
isn't surprising though, seeing how as price increased, there were far fewer houses with those prices for the model to
learn from.

All in all it went well.  There wasn't much EDA, although there was some.  I think, at least if the goal is to have a black box
afterwards, we have to be honest with ourselves about what people have to do and what people can just automate.  It's also
worth noting that this was literally just me sitting down all of yesterday and doing this (just one day),
imputing one column really well, cutting some columns out, and running TPOT for 10 minutes, and the model is 
absolutely crushing it.

The VP at Demyst said to our class regarding automating black box creation, they have a program that they can just run for a 
few hours and it will basically always score within the top 2-3 percent of a kaggle competition.

In the coming days, just to get something up here, maybe I will do something with more EDA (white box.)
