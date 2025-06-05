# Do 'Impossible' Recipes Have A Lower Average Rating?

Author: James Wootton

# Introduction

Dieting often makes people think of images of restrictive eating, bland meals, and uninspiring snacks. Many dieters report that "diet foods"—those specially formulated to be lower in calories, sugar, or fat—tend to be less satisfying and less flavorful than their regular counterparts. Is this just a matter of perception? or do diet foods actually taste worse? Analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com, **I will try to answer the question if people would rate diet foods lower due to being lower in calories, sugar, or fat content.** To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on food.com.

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


