# Do Diet Foods Have A Lower Average Rating?

Author: James Wootton

# Introduction

Dieting often makes people think of images of restrictive eating, bland meals, and uninspiring snacks. Many dieters report that "diet foods"—those specially formulated to be lower in calories, sugar, or fat—tend to be less satisfying and less flavorful than their regular counterparts. Is this just a matter of perception? or do diet foods actually taste worse? Analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com, **I will try to answer the question if people would rate diet foods lower due to being lower in calories, sugar, or fat content.** To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com.

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

# Data Cleaning and Exploratory Data Analysis

1. Left Merge on id in the recipes dataset and recipe_id in the interactions dataset, which matches recipe information with review data.

2. Fill all ratings of zero with NaN values. All ratings must be between 1 and 5 stars, so a recipe rating of zero indicates a recipe with no reviews.

3. Add the Column `'average_rating'` which adds the average rating for each review, based on the mean recipe rating for all reviews of that specific recipe.

4. Converted `'tags'`, `'ingredients'`, `'nutrition'` columns from strings to lists. `'tags'`, and `'ingredients'` columns get converted to a list of strings, displaying the respective recipe tags and ingredients used in that recipe. `'nutrition'` gets converted into a list of floats, corresponding to the relevent nutrition numerical values.








