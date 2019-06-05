---
layout: post
title:      "Feature Selection - Module 1 Project"
date:       2019-06-05 01:46:58 +0000
permalink:  feature_selection_-_module_1_project
---


Feature selection is probably what I spent the most brainpower on when I was doing my Module 1 on linear regression. Picking the right data to use is the basis of a good model; “garbage in, garbage out”, as the saying goes. I ended up with an R-squared of 87.6% so I am confident I was at least on the right track with the features I chose. I will go over how I approached feature selection and my thought process and and hopefully this will give you some ideas and direction for your own project. 

Right at the beginning, I identified features that obviously would be of no use for a linear regression model and got rid of those. “ Id” and “date” were the first to go. Then I went on to dig into the data. “view” and “sqft_basement” got dropped because they were both missing data and I didn’t think either would be useful enough to keep by trying to figure out a way to fill in the missing data or dropping rows. “yr_renovated” got dropped for the same reason. 

Then I made a correlation heatmap. “sqft_above” and “sqft_living” were highly correlated so I dropped “sqft_above” to avoid biasing my model. Judging by their descriptions, they were pretty much the same data anyway. I picked “sqft_above” to drop because “sqft_living” was more strongly correlated to the target feature “price”. 

I tried recursive feature elimination to look for candidate features to drop. It ranked “floors” as the least fit so I tried dropping it. That didn’t end up improving my model and actually slightly lowered my r-squared so I kept “floors”.  I also tried dropping “waterfront” because it was mostly zeros. That, too, ended up hurting my model so I kept it in. (I set “waterfront” to be categorical.)

Near the beginning of the process I had set aside “lat”, long”, and “zipcode” to examine last as they would have to be one-hot encoded to use. I couldn’t come up with a good way to bin “lat” and “long” that that seemed like it would work well in a linear regression model so I ended up dropping them. My main reasoning was that “zipcode” gave me a way to factor location into my model more easily so I didn’t need to use them. (Also, using “lat” and “long” along with “zipcode” would “double count” for location and bias the model.) I one-hot encoded “zipcode” and dropped one of the columns as you are supposed to do.

I felt that I could go on another week or so tweaking my model’s performance. But at a point I had to call it “done” so that I could move on. (One of the pitfalls of being a self-paced student is with having no hard deadlines perfectionism can creep in.) But if I had to do it again, there are a couple of things I would try differently. Like dropping “sqft_lot15” and “year”, because I don’t think they did much for my model. Also, though this veers into data cleaning, it isn’t always obvious if a feature should be treated as continuous or categorical. There were a few features that contained discrete data. I struggled a bit in trying to decide if I should treat them as continuous or categorical variables. I ended up using them as continuous variables. My reasoning was that the numbers encoded data, they weren’t a name or Boolean. (For example, two floors are more than one floor.) But maybe some of them would have performed better as categorical.

That was my process for feature selection. I made use of established workflows and tools, and used my best judgment. 


