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
5. Merged the recipes and interactions datasets and calculated average rating per recipe `avg rating` 
  -  I merged the two data sets on `id` from the recipes dataset and `recipe_id` from the interactions dataset to get each rating for each recipe. Then, I took the average of all the ratings for each recipe since one recipe can hold multiple ratings from various users. This allows a more general understanding of the ratings for each recipe.
6. Added `is_main_dish` to the merged dataframe
  - `is_main_dish` is a boolean column checking if the recipe contains the tag 'main_dish'. Main dishes usually have more protein than other types of foods, such as snacks and dessert since they contain protein sources like meat, fish, tofu, beans, eggs, etc. This addition gives another way to compare recipes with more protein to recipes with less protein.

Here is the head of the cleaned dataframe with 83782 rows and 21 columns. For the website, I included the columns relevant for data analysis:

| name                                 |     id | submitted           |   calories |   protein |   avg_rating | is_main_dish   |
|:-------------------------------------|-------:|:--------------------|-----------:|----------:|-------------:|:---------------|
| 1 brownies in the world    best ever | 333281 | 2008-10-27 00:00:00 |      138.4 |         3 |            4 | False          |
| 1 in canada chocolate chip cookies   | 453467 | 2011-04-11 00:00:00 |      595.1 |        13 |            5 | False          |
| 412 broccoli casserole               | 306168 | 2008-05-30 00:00:00 |      194.8 |        22 |            5 | False          |
| millionaire pound cake               | 286009 | 2008-02-12 00:00:00 |      878.3 |        20 |            5 | False          |
| 2000 meatloaf                        | 475785 | 2012-03-06 00:00:00 |      267   |        29 |            5 | True           |


### Univariate Analysis

For Univariate Analysis, I examined the distribution of the protein (PDV) in a recipe when the Protein (PDV) is less than 150. When I intially plotted the protein distribution without the filter, the plot did not show much of a trend and bunched up the data into one singular bar. Thus, I added the filter to see
any trend in the distribution. The plot shows that the distribution is skewed right, revealing that most recipes from the dataset have a low Protein (PDV). 

<iframe
  src="assets/protein_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

Embed at least one grouped table or pivot table in your website and explain its significance.

For Bivariate Analysis, I examined the relationship between protein (PDV) and average recipe ratings. From the plot, we can see as Protein (PDV) increases, average recipe ratings incerase as well. However, this graph has various outliers when the average rating is 1, 2, 4, and 5, which is important to observe
since outliers can impact our data analysis. Thus, these outliers need to be filtered out, which will be done for the basemodel.


<iframe
  src="assets/protein_vs_avg.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

I created a grouped table comparing protein content between main-dish recipes and non-main dishes. I create a dataframe called `protein_by_type` that shows the count, mean, median, and standard deviation for is_main_dish. The results showed that main dishes tend to have higher average protein than non-main dishes, which aligns with my expectations that meals like entrées include more protein-rich ingredients such as meat or legumes. This table helped validate that my derived variable for “main dish” was meaningful and relevant to the dataset.

| is_main_dish   |   count |    mean |   median |     std |
|:---------------|--------:|--------:|---------:|--------:|
| False          |   58577 | 21.6066 |       11 | 42.9628 |
| True           |   25205 | 59.9219 |       54 | 57.8012 |


## Assessment of Missingness


## NMAR Analysis
I believe that the `protein` column is likely NMAR (Not Missing At Random). This is because the missingness of protein values likely depends on the actual protein content itself. For example, recipes with very low protein may be more likely to omit nutritional information altogether, especially desserts or snacks. Since the missingness may depend directly on the unobserved value of protein, this makes it plausibly NMAR. Additional data such as standardized nutrition reporting requirements or the source of the nutrition data could help determine whether this missingness could instead be explained by observed features (thereby making it MAR).

I also believe the `avg_rating` column may be NMAR (Not Missing At Random). Recipes with very poor quality or confusing instructions may be less likely to receive ratings because users may abandon the recipe or choose not to leave feedback. This would make the missingness directly related to the unobserved true rating itself. Additional data such as page view counts or user interaction logs could help explain the missingness and potentially make it MAR.

### Missingness Dependency

I conducted 2 permutations tests:
- To see if the missingness of avg_rating is dependent on whether a recipe is a main dish or not. 
- To see if 
The missingness of avg_rating appears to depend slightly on whether a recipe is a main dish. The observed difference in missingness between main dishes and non-main dishes is very small (-0.0033), but the permutation test gives a p-value of 0.009, suggesting that this difference is statistically significant. This indicates that main dish recipes are slightly less likely to have missing average ratings than non-main dishes.


## Hypothesis Testing

Question: Do recipes with higher protein levels tend to have higher average ratings than recipes with low protein levels?

Approach: We compared the average ratings of recipes with protein content above the median versus those at or below the median. Since main dishes generally contain more protein, we performed the test within main dishes only to reduce confounding.




### Hypotheses:

Null (H0) = There is no difference in average ratings between high protein and low protein recipes (mu high protein - mu low protein = 0)

Alternative (H1) = There is a dfference in average ratings between high protein and low protein recipes (mu high protein - mu low protein > 0)

Test statistic: We used the difference in mean average ratings between high-protein and low-protein recipes as our test statistic (high - low)

Method: We performed a permutation test by randomly shuffling the high/low protein labels 5000 times to simulate the distribution of mean differences under the null hypothesis.

### Results

<iframe
  src="assets/file-hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed difference in mean ratings was -0.025375283811557736, indicated by the red line in the plot below. The resulting p-value from the permutation test was 1.0

If p_value < 0.05: “We reject H0 at α = 0.05. The data provide evidence that high-protein recipes get higher average ratings than low-protein recipes.”

If p_value >= 0.05: “We fail to reject H0 at α = 0.05. The data do not provide strong evidence that high-protein recipes get higher average ratings than low-protein recipes.”
Caveat: This is an observational analysis: confounding variables (recipe type, sweetness, ingredient quality) could explain part of any observed relationship.

Since the p-value is **greater** than 0.05, we **fail to reject** the null hypothesis. This suggests that high-protein recipes do not receive higher average ratings compared to low-protein recipes.

Visualization

We created a box plot comparing the distribution of average ratings for high-protein vs. low-protein recipes. The plot shows the spread of ratings and any noticeable differences in median ratings between the two groups.


## Prediction Problem

Prediction Problem: Predict the protein level of recipes.
Type of prediction problem: regression (protein is a continuous variable)


## Baseline Model

My baseline model predicts the protein content of recipes by standardizing numerical nutrition features (calories and carbohydrates) and transforming recipe tags into numerical features using TF-IDF, then fitting a linear regression model to learn patterns in the data.


##  Final Model

## Fairness Analysis
