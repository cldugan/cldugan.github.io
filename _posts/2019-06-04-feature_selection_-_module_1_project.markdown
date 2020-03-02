---
layout: post
title:      "Feature Selection - Module 1 Project"
date:       2019-06-04 21:47:01 -0400
permalink:  feature_selection_-_module_1_project
---


When doing my Module 1 project on linear regression, I spent a lot of time considering feature selection. Picking the right data to use is the basis of a good model; “garbage in, garbage out”, as the saying goes. My model ended up with an adjusted R-squared of 87.6% so I am confident I was at least on the right track with the features I chose. I will go over how I approached feature selection and my thought process and and hopefully this will give you some ideas and direction for your own project. 

The dataset used for this project is a modified version of the King County House Sales dataset from Kaggle. Right at the beginning, I identified features that obviously would be of no use for a linear regression model and got rid of those. The features “ Id” and “date” fell into this category. Then it was time to dig into the data. I checked the dataset for nans or placeholders. “yr_renovated”, "view” and “sqft_basement” all had a lot of missing data. My choice then was to either drop rows with missing data (my least favorite choice), replace the data, or delete the columns. For all three of these features I decided to drop the columns because I didn’t think they would be useful enough to keep by trying to figure out a way to fill in the missing data, and with dropping rows I would have lost lots of other data.  The feature "waterfront" was also missing some data. It wasn't missing a ton of data and I thought this feature probably had a large effect on the price so I decided to fill in the missing data with the mode.

Then I made a correlation heatmap. See below for example code and heatmap. I used a mask to block the reduntant half of the heatmap, I think it is easier to read this way. “sqft_above” and “sqft_living” were highly correlated with each other so I dropped “sqft_above” to avoid biasing my model. Judging by their descriptions, they were pretty much the same data anyway. I picked “sqft_above” to drop because “sqft_living” was more strongly correlated to the target feature “price”.  I made scatter plots with best-fit lines of each continuous feature with the target “price” to check the linear relationships. I felt everything looked good enough at that point to move on with modeling.

```
# These are the libraries used for the heatmap
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# plotting heatmap of variables for correlation and collinearity
plt.figure(figsize=(12,10))
corr = abs(data_pred.corr())
mask = np.zeros_like(corr)
mask[np.triu_indices_from(mask)] = True
with sns.axes_style("white"):
    ax = sns.heatmap(corr, mask=mask, annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap');
```

![Imgur](https://i.imgur.com/uK1kV6H.jpg)

After running my first linear regression model I tried recursive feature elimination to look for candidate features to drop. It ranked “floors” as the least fit so I tried dropping it. That didn’t end up improving my model and actually slightly lowered my r-squared so I kept “floors”.  I also tried dropping “waterfront” because it was mostly zeros. That, too, ended up hurting my model so I kept it in. (I set “waterfront” to be categorical.) 
Code for recursive feature elimination:

```
# Import libraries needed for recursive feature elimination
from sklearn.linear_model import LinearRegression
from sklearn.feature_selection import RFE

# Running recursive feature elimination to look for candidate variables to drop to see if I can improve model.

predictors = data_df.drop(['price', 'waterfront'], axis=1)

linreg = LinearRegression()
selector = RFE(linreg, n_features_to_select = 1)
selector = selector.fit(predictors, data_df["price"])
list(zip(predictors.columns,selector.ranking_))
```
The output is a list of features and their rank.  Features with lower numbers are more fit:
```
[('bedrooms', 7),
 ('bathrooms', 3),
 ('sqft_living', 2),
 ('sqft_lot', 8),
 ('floors', 10),
 ('condition', 9),
 ('grade', 1),
 ('yr_built', 4),
 ('sqft_living15', 5),
 ('sqft_lot15', 6)]
 ```
Though this veers into the topic of data cleaning, it isn’t always obvious if a feature should be treated as continuous or categorical. There were a few features that contained discrete data. I struggled a bit in trying to decide if I should treat them as continuous or categorical variables. I ended up treating all of them as continuous variables. My reasoning was that the numbers encoded data, they weren’t a name or a category. (For example, two floors are more than one floor.)

Near the beginning of the process I had set aside “lat”, long”, and “zipcode” to examine last as they would have to be one-hot encoded to use as categorical data. I couldn’t come up with a good way to bin “lat” and “long” that that seemed like it would work well in a linear regression model so I ended up dropping them. My main reasoning was that “zipcode” gave me a way to easily factor location into my model so I didn’t need to use them. (Also, using “lat” and “long” along with “zipcode” would “double count” for location and bias the model.) I one-hot encoded “zipcode” into dummy variables and dropped one of the columns.

After I added the "zipcode" dummy variables to my model, I checked to make sure all my features had a p-value of less than 0.05 (Super important!). I ended up with 11 features plus 70 “zipcode” categories. I really learned a lot during this process and I feel like I am one step closer to becoming a data scientist. 

