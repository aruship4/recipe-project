# Investigating the Relationship Between Protein Content and Recipe Ratings

Author: Arushi Patra

## Overview

This data science project (for the DSC 80 class at UCSD) aims to investigate the relationship between protein content and ratings for a recipe.

## Introduction

Food is essential to daily life, and for many people, eating healthy and nutritious meals is a priority. As a college student, I try to incorporate meals with higher protein to support body strength, but different groups have varying preferences. I wanted to explore how recipe protein content influences user ratings. Specifically, do recipes with higher protein levels tend to receive higher ratings? The dataset for this project, Recipes and Ratings, comes from [food.com](https://www.food.com/). It contains two CSV's: one for recipes (RAW_recipes.csv) and one for interactions (RAW_interactions.csv), which holds ratings and revies submitted for the recipes in RAW_recipes.csv.

The first dataset, `recipe`, contains 83782 rows (83782 unique recipes), with 12 columns below.

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset, `interactions`, contains 731927 rows. Each row holds a review from the user on a specific recipe. The columns it contains are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction (YYYY-MM-DD) |
| `'rating'`    | Rating given (Scale from 1-5) |
| `'review'`    | Review text         |


By analyzing these columns, we can explore how protein content, nutritional values, and recipe characteristics influence how users rate recipes, providing insight into what makes a meal both nutritious and high-rated.

## Data Cleaning and Exploratory Data Analysis

In order to best prepare my data for thoroough data analysis, I cleaned my data sets by taking the following steps:

1. Filled ratings with 0 with NaN
  - A rating of 0 means there is no rating associated with the recipe (recipe ratings range from 1-5). Thus, I replaced ratings of 0 with NaN. Treating these as missing ensures that the average ratings accurately reflect only the ratings that were actually provided.
2. Convert date strings to date time objects
  - Dates are stored as objects in the dataset, so I converted them to date time objects in order to perform proper analysis over time. 
3. Converted columns with lists as strings to actual lists
  - The `nutrition` columns stored the PDV values as a string, and the `tags` column stored tags as a string. For each one, I converted the entire string into an actual list in order to extract each tag and the Protein level for each recipe.
4. Split nutrition into separate columns
  - I split nutrition, which is stored as a list in the data set, into separate columns in order to specifically focus on the protein value.
5. merged the recipes and interactions datasetsCalculated average rating per recipe `avg rating` 
  -  I merged the two data sets on `id` from the recipes dataset and `recipe_id` from the interactions dataset to get each rating for each recipe. Then, I took the average of all the ratings for each recipe since one recipe can hold multiple ratings from various users. This allows a more general understanding of the ratings for each recipe.


Here is the head of the cleaned dataframe with 83782 rows and 19 columns. For the sake of the website, I included the columns relevant for data analysis:

| name                                 | submitted           |   calories |   protein |   avg_rating |
|:-------------------------------------|:--------------------|-----------:|----------:|-------------:|
| 1 brownies in the world    best ever | 2008-10-27 00:00:00 |      138.4 |         3 |            4 |
| 1 in canada chocolate chip cookies   | 2011-04-11 00:00:00 |      595.1 |        13 |            5 |
| 412 broccoli casserole               | 2008-05-30 00:00:00 |      194.8 |        22 |            5 |
| millionaire pound cake               | 2008-02-12 00:00:00 |      878.3 |        20 |            5 |
| 2000 meatloaf                        | 2012-03-06 00:00:00 |      267   |        29 |            5 |

Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that's fine. Feel free to embed more than one univariate visualization in your website if you'd like, but make sure that each embedded plot is accompanied by a description.)

Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present. (Your notebook will likely have more visualizations than your website, and that's fine. Feel free to embed more than one bivariate visualization in your website if you'd like, but make sure that each embedded plot is accompanied by a description.)

Embed at least one grouped table or pivot table in your website and explain its significance.



## Assessment of Missingness


## Hypothesis Testing

## Prediction Problem


## Baseline Model

My baseline model predicts the protein content of recipes by standardizing numerical nutrition features (calories and carbohydrates) and transforming recipe tags into numerical features using TF-IDF, then fitting a linear regression model to learn patterns in the data.

##  Final Model

## Fairness Analysis
