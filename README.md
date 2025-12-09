# Investigating the Relationship Between Protein Content and Recipe Ratings

Author: Arushi Patra

## Overview

This data science project (for the DSC 80 class at UCSD) aims to investigate the relationship between protein content and ratings for a recipe.

## Introduction

Food is essential to daily life and production. For many, eating healthy, nutritious meals is a priority. For example, college students, including myself, try to incorporate meals with more protein in order to increase body strength. However, various groups have different preferences, and I wanted to see how people feel about higher/lower protein content in meals they cook. As I am trying to cook more and eat less processed meals in my day to day routine, I wanted to investigate if recipes with higher protein level tend to get higher recipe ratings? The dataset for this project, Recipes and Ratings, comes from [food.com](https://www.food.com/). It contains two CSV's: one for recipes (RAW_recipes.csv) and one for interactions (RAW_interactions.csv), which holds ratings and revies submitted for the recipes in RAW_recipes.csv.

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
| `'rating'`    | Rating given (Scale from 0-5) |
| `'review'`    | Review text         |


Provide an introduction to your dataset, and clearly state the one question your project is centered around. Why should readers of your website care about the dataset and your question specifically? Report the number of rows in the dataset, the names of the columns that are relevant to your question, and descriptions of those relevant columns.


## Data Cleaning and Exploratory Data Analysis


## Assessment of Missingness


## Hypothesis Testing

## Prediction Problem


## Baseline Model

My baseline model predicts the protein content of recipes by standardizing numerical nutrition features (calories and carbohydrates) and transforming recipe tags into numerical features using TF-IDF, then fitting a linear regression model to learn patterns in the data.

##  Final Model

## Fairness Analysis
