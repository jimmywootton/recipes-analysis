# Do Diet Foods Have A Lower Average Rating?

Author: James Wootton

## Introduction

Dieting often makes people think of images of restrictive eating, bland meals, and uninspiring snacks. Many dieters report that "diet foods" those specially formulated to be lower in calories, sugar, or fat—tend to be less satisfying and less flavorful than their regular counterparts. Is this just a matter of perception? Or do diet foods actually taste worse? Analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com, **I will try to answer the question if people would rate diet foods lower due to being lower in calories, sugar, or fat content.** To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com.

The first dataset, `recipe`, contains 83782 rows and 10 columns with the following information:


| Column             | Description                                                                                                                                                |
| :----------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `'name'`           | Recipe name                                                                                                                                                |
| `'id'`             | Recipe ID                                                                                                                                                  |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                  |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                          |
| `'submitted'`      | Date recipe was submitted                                                                                                                                  |
| `'tags'`           | Food.com tags for recipe                                                                                                                                    |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                  |
| `'steps'`          | Text for recipe steps, in order                                                                                                                             |
| `'description'`    | User-provided description                                                                                                                                  |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                 |
| `'n_ingredients'`  | Number of ingredients in recipe  

The second dataset, `interactions`, contains 83782 rows and 5 columns with the following information:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

1. Left Merge on id in the recipes dataset and recipe_id in the interactions dataset, which matches recipe information with review data.

2. Fill all ratings of zero with NaN values. All ratings must be between 1 and 5 stars, so a recipe rating of zero indicates a recipe with no reviews.

3. Add the Column `'average_rating'` which adds the average rating for each review, based on the mean recipe rating for all reviews of that specific recipe.

4. Add `'diet'` column. This is a boolean column that is `'True'` for recipes that either contain diet in the recipe name or contain the word diet in one of the tags. This essentially splits up our recipes into two groups, diet and non diet foods.

The resulting first five rows of the Dataframe are shown below. I have ommited some columns for readability such as the `'ingredients'` and `'description'`. Scroll right to view all columns:

| name                                  |     id | minutes | contributor_id | submitted  | calories (#) | total fat (PDV) | sugar (PDV) | sodium (PDV) | protein (PDV) | saturated fat (PDV) | carbohydrates (PDV) | diet  |
|:--------------------------------------|-------:|--------:|---------------:|:-----------|--------------:|-----------------:|------------:|-------------:|---------------:|--------------------:|---------------------:|:-----:|
| 1 brownies in the world    best ever  | 333281 |      40 |          985201 | 2008-10-27 |         138.4 |             10.0 |         50.0 |          3.0 |             3.0 |                19.0 |                  6.0 | False |
| 1 in canada chocolate chip cookies    | 453467 |      45 |         1848091 | 2011-04-11 |         595.1 |             46.0 |        211.0 |         22.0 |            13.0 |                51.0 |                 26.0 | False |
| 412 broccoli casserole                | 306168 |      40 |           50969 | 2008-05-30 |         194.8 |             20.0 |          6.0 |         32.0 |            22.0 |                36.0 |                  3.0 | False |
| 412 broccoli casserole                | 306168 |      40 |           50969 | 2008-05-30 |         194.8 |             20.0 |          6.0 |         32.0 |            22.0 |                36.0 |                  3.0 | False |
| 412 broccoli casserole                | 306168 |      40 |           50969 | 2008-05-30 |         194.8 |             20.0 |          6.0 |         32.0 |            22.0 |                36.0 |                  3.0 | False |

### Univariate Analysis

Now that the data is clean, I examined the distribution of ratings for recipes. As the plot shows below, a large majority of ratings are more than 4 stars. The distribution shows that ratings are skewed towards larger ratings.

<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

For this analysis, I examined the distribution of the ratings of recipes between diet food and non-diet foods. The graph below shows that a slightly higher percentage non-diet foods are rated 5 stars compared to diet foods.

<iframe
  src="assets/bivariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

**Null Hypothesis:** People rate all the recipes on the same scale.

**Alternative Hypothesis:** People rate diet recipes lower than non-diet recipes.

**Test:** Permutation Test

**Test Statistic:** The difference in mean rating between diet and non-diet recipes

**Significance Level:** 0.05

The reason I chose a permutation test is because I have two samples, but no information about any population distributions. I am testing whether these samples where drawn from the same population or not. I chose the different in mean ratings between these two different groups because I am trying to figure out if diet recipes have lower ratings, where positive or negative differences between these two group means informs whether one group has lower ratings than another.

My **observed statistic** is **-0.013**, indicating the mean rating of diet recipes was slightly lower than non-diet recipes. Then we shuffled the ratings for 500 times to collect 500 simulating mean differences in the two distributions as described in the test statistic. We got a p-value of 0.0. Indicating that the difference between group means was statistically significant.

<iframe
  src="assets/empirical_diff_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

I plan to **predict average rating of a recipe** which would be a **regression problem** since we can treat rating as a numerical variable, since average ratings can take on a continuous range of values between 1 and 5 stars. To solve this problem, I am going to build a linear regression model.

The response variable in this case would be average ratings, since it is good indication of the overall rating of the recipe. We previously found that diet foods are rated lower on average than non-diet foods, but not by much. By including this feature we may be able to get slightly better performance on our model.

To evaluate our model, I will use the root mean squared error, since that is the benchmark metric for linear regression model performance, as well as being mean squared error being the metric by which a linear regression model is fitted for.

## Baseline Model

For my baseline model, I will use a standard linear regression model. The features I am using for this model are `'minutes'`, `'n_steps'`, `'n_ingredients'`, and `'diet'`. All columns contain numerical values except for `'diet'`, a column containing nominal boolean values. 

I one hot encoded the `'diet'` column with 0 and 1 values, allowing our model to properly use this as a feature. 

After training my model for training data, my root mean squared error for the training data and test data was 0.64 and 0.67 respectively. However, after plotting residuals of actual vs predicted values, I found that the predicted values has extremely little variance and were centered around the mean. I suspect the reason being poor correlation between the response variable and features, as well as a response variable that is highly skewed towards larger ratings (see univariate analysis).

<iframe
  src="assets/predicted_vs_actual.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Final Model

For my final model, I will use all the same columns as in my baseline model, with some additional columns and transformations.

1. Use a function transformer to extract features from the `'nutrition'` column, yielding `calories (#)`, `total fat (PDV)`, `sugar (PDV)`, `sodium (PDV)`, `protein (PDV)`, `saturated fat (PDV)`, and `carbohydrates (PDV)`. Sugary or fatty recipes probably correlate with higher ratings.

2. One hot encode if the recipe is nutritionally dense or not. This is done by using the results from step 1, and converting calories to a percentage of daily value (based on a 2000 calorie diet). Then check if the average PDV for all the other columns is higher than the PDV for calories or not. 

I originally framed this problem as a regression problem, hoping to plot average recipe ratings that are on a continuous range. However after testing on different regression models I found that continuously over predict low ratings. Average ratings are so skewed towards values between 4 and 5 stars, a loss function used in linear regression would penalize only a small fraction of low ratings very highly, while the large majority of high ratings would have very little loss. 

To combat this problem, I decided to take the average ratings column and round it to the nearest integer, resulting in values 1, 2, 3, 4, or 5, creating classes for different rounded ratings respectively. Then I used RandomForestClassifier, in the hopes that it would capture differences  between ratings better than the regression model.

Using `'GridSearchCV'`, I searched for the best hyperparameters that yielded the highest weighted f1 score, where weights are inversely proportional to class frequency.
I used this metric in hopes penalize incorrect predictions for infrequent classes (1,2, and 3).

Hyperparameters:

1. `'n_estimators'`

2. `'max_depth'`

The hyperparameters that yielded the best weighted_f1 score were 100 estimators and no `'max_depth'`. The weight f1 score was 0.51. This isn't the same metric as my baseline model, however to make them more comparable, I found that the mean squared error of my final model was 1.21. Although this was worse performance in terms of mean squared error, my final model had improved accuracy for lower average ratings, where in my baseline model would have over-predicted all lower average ratings by several points.

<iframe
  src="assets/accuracy_by_class.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>




