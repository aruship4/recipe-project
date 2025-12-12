# Exploring the Relationship Between Protein Content and Recipe Ratings

Author: Arushi Patra

## Overview

This data science project (for the DSC 80 class at UCSD) aims to determine if any relationship exists between protein content and ratings for a recipe.

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

In order to best prepare my data for thorough data analysis, I cleaned my data sets by taking the following steps:

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

For Bivariate Analysis, I examined the relationship between protein (PDV) and average recipe ratings. From the plot, we can see as Protein (PDV) increases, average recipe ratings increase as well. However, this graph has various outliers when the average rating is 1, 2, 4, and 5, which is important to observe
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


To better understand why some recipes are missing avg_rating, I performed two permutation tests. Each test examines whether the missingness of avg_rating depends on another observed variable.

1. Missingness of avg_rating vs. is_main_dish

The first permutation test checks whether the probability that avg_rating is missing differs between main-dish recipes and non–main-dish recipes.

<iframe
  src="assets/missingness_diff_main_dish.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Observed difference: -0.0033
(meaning main dishes have slightly lower missingness than non-main dishes)


Interpretation of the plot:
The permutation distribution is centered around 0, as expected under the null hypothesis of independence. The observed difference lies slightly in the left tail.

Conclusion:
Although the observed difference is extremely small in absolute size, the permutation distribution shows that this difference is unlikely to occur by random chance, producing a small p-value.

Interpretation:
There is weak but statistically detectable evidence that main-dish recipes are slightly less likely to have missing average ratings than non-main dishes. The effect is small and not practically meaningful, but it suggests that missingness is not completely random with respect to whether a recipe is a main dish.


2. Missingness of avg_rating vs. recipe duration (minutes > 60)


<iframe
  src="assets/missingness_minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
## Hypothesis Testing


The second permutation test checks whether the missingness of avg_rating differs between long recipes (more than 60 minutes) and shorter recipes.

Observed difference: 0.0136
(meaning long recipes have a noticeably higher rate of missing ratings)

Interpretation of the plot:
The permutation distribution is tightly centered near 0. The observed value (≈ 0.014) lies far in the right tail, well outside the range of typical permuted differences.

Conclusion:
This produces a very small p-value, providing strong evidence that missingness of avg_rating depends on recipe length.

Interpretation:
Recipes that take longer than an hour to prepare appear substantially more likely to be missing average ratings. This suggests that users may be less likely to rate or review long recipes—possibly because they choose quicker dishes more often or because longer recipes are attempted less frequently.

Summary of Missingness Findings

Missingness of avg_rating is not independent of at least two observed variables (main dish status and recipe duration). The dependency with recipe duration is much stronger and more practically meaningful. These results support the idea that the avg_rating variable is MAR (Missing At Random) with respect to at least some observed features—meaning the missingness can be partially explained by variables included in the dataset. This suggests that imputations or modeling approaches that condition on recipe attributes (like duration) may be appropriate, whereas approaches assuming MCAR would not be justified.

### Hypotheses:

Question: Do recipes with higher protein levels tend to have higher average ratings than recipes with low protein levels? 

Approach: We compared the average ratings of recipes with protein content above the median versus those at or below the median. Since main dishes generally contain more protein, we performed the test within main dishes only to reduce confounding.

Null (H0) = There is no difference in average ratings between high protein and low protein recipes (mu high protein - mu low protein = 0)

Alternative (H1) = There is a difference in average ratings between high protein and low protein recipes (mu high protein - mu low protein > 0)

Test statistic: We used the difference in mean average ratings between high-protein and low-protein recipes as our test statistic (high - low)

Method: We performed a permutation test by randomly shuffling the high/low protein labels 5000 times to simulate the distribution of mean differences under the null hypothesis.

### Results

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed difference in mean ratings was -0.025375283811557736, indicated by the red line in the plot below. The resulting p-value from the permutation test was 1.0

If p_value < 0.05: “We reject H0 at α = 0.05. The data provide evidence that high-protein recipes get higher average ratings than low-protein recipes.”

If p_value >= 0.05: “We fail to reject H0 at α = 0.05. The data do not provide strong evidence that high-protein recipes get higher average ratings than low-protein recipes.”
Caveat: This is an observational analysis: confounding variables (recipe type, sweetness, ingredient quality) could explain part of any observed relationship.

Since the p-value is **greater** than 0.05, we **fail to reject** the null hypothesis. This suggests that high-protein recipes do not receive higher average ratings compared to low-protein recipes.


## Prediction Problem

Prediction Problem: Predict the protein level of recipes.
Type of prediction problem: regression (protein is a continuous variable)


## Baseline Model


For the baseline model, I used a linear regression model to predict the protein content of recipes. The dataset was split into training and test sets. The model used the following features:

calories (numerical)

carbohydrates (numerical)

tags (text data converted into numerical features using TF–IDF)

The numerical features were standardized using StandardScaler so they were on comparable scales before training. For the text-based tags column, I used TF-IDF Vectorization to convert the words into numerical representations that the model could learn from.

The baseline model achieved a Train RMSE of 33.127221394648664 and a Test RMSE of 26.89842012868557, along with a Train R² of 0.6010357558770942 and a Test R² of 0.641604648153275. These results indicate that the model captures some patterns in the data, but still leaves room for improvement in accurately predicting protein content.

The purpose of this baseline model was to establish a simple, interpretable starting point before adding more sophisticated features and modeling techniques.

##  Final Model


Final Model

For our final model, I used a Random Forest Regressor to predict the protein content of recipes. This model builds on our baseline by incorporating engineered nutritional features and text-based features extracted from recipe tags.

Features Used

(I) trained the model using the following features:

log_calories
- We applied a log transformation to the calorie values using log(calories + 1) to reduce skew and limit the impact of extreme outliers.

cal_to_carb_ratio: I created a calorie-to-carbohydrate ratio feature: calories / (carbohydrates + 1)
- This captures the nutritional balance of each recipe more effectively than raw values alone.

carbohydrates
- I included total carbohydrate content as a direct macronutrient feature.

tag_str
- I combined recipe tags into a single string and used TF-IDF vectorization to extract useful text-based features from the tags.

### Preprocessing and Model Pipeline

I built a pipeline that:

Standardized all numeric features using StandardScaler

Converted text features into numerical form using TF-IDF Vectorization

Trained a Random Forest Regressor for prediction

### Hyperparameter Tuning

We used GridSearchCV to tune the model and selected the best hyperparameters based on minimizing RMSE:

Best Parameters:

n_estimators = 50

max_depth = None

min_samples_leaf = 1

Model Performance

We evaluated the model using RMSE and R² on both the training and test sets:

| Metric   | Training|   Test  |
|:---------|--------:|--------:|
| RMSE     |  12.20 | 22.26 |
| R²       |  0.95  | 0.75  |

The final model explains about 75% of the variance in protein content on unseen data, indicating strong predictive performance. The gap between training and test performance suggests some overfitting, but overall the model generalizes well.

### Interpretation

This final model significantly improves over the baseline by:
- Using engineered nutritional ratios
- Incorporating text information from recipe tags
- Applying a non-linear ensemble learning method

Overall, the model provides accurate and useful predictions of recipe protein content, while demonstrating reasonable generalization beyond the training data.

## Fairness Analysis

To evaluate fairness of our final model, we examined whether the model’s performance differed between main-dish recipes and non-main dishes. Main dishes tend to have higher protein content, so it is important to ensure the model predicts fairly across these groups.


We split the test set into two groups based on the is_main_dish column:

- Main dishes (is_main_dish = True)

- Non-main dishes (is_main_dish = False)

We then computed the RMSE for each group to measure predictive performance.

Null Hypothesis (H0): The model’s RMSE is similar across main-dish and non-main-dish recipes; any observed difference is due to random chance.

Alternative Hypothesis (H1): The model’s RMSE differs between main-dish and non-main-dish recipes.

Test Statistic: Difference in RMSE between main-dish and non-main-dish groups (RMSE_main - RMSE_non-main).

Significance Level: 0.05

### Results

<iframe
  src="assets/fairness_analysis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe> 

RMSE by group:


| is_main_dish | RMSE|
|:---------|--------:|
| RMSE     |  21.80  |
| R²       |  23.28  |

Observed RMSE difference (main - non-main): 1.49

Permutation p-value: 0.88

Since the p-value (0.88) is greater than 0.05, we fail to reject the null hypothesis. This indicates there is no significant difference in model performance between main-dish and non-main-dish recipes, suggesting the model is reasonably fair with respect to recipe type.