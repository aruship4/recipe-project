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


By analyzing these columns, I can explore how protein content, nutritional values, and recipe characteristics influence how users rate recipes, providing insight into what makes a meal both nutritious and high-rated.

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

For Bivariate Analysis, I examined the relationship between protein (PDV) and average recipe ratings. From the plot, we can see big clusters around average ratings of 4 and 5, indicating that a large portion of the dataset lies here. Using this data, we will further explore the relationship between protein and average ratings to see if any correlation does exist.

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

The `avg_rating` column has a significant amount of missing values (2609 rows have an average rating missing), which is the column I used to assess missingness for the recipes dataframe.

## NMAR Analysis
I believe that the `protein` column is likely NMAR (Not Missing At Random). This is because the missingness of protein values likely depends on the actual protein content itself. For example, recipes with very low protein may be more likely to omit nutritional information altogether, especially desserts or snacks. Since the missingness may depend directly on the unobserved value of protein, this makes it plausibly NMAR. Additional data such as standardized nutrition reporting requirements or the source of the nutrition data could help determine whether this missingness could instead be explained by observed features (thereby making it MAR).

I also believe the `avg_rating` column may be NMAR (Not Missing At Random). Recipes with very poor quality or confusing instructions may be less likely to receive ratings because users may abandon the recipe or choose not to leave feedback. This would make the missingness directly related to the unobserved true rating itself. Additional data such as page view counts or user interaction logs could help explain the missingness and potentially make it MAR.

### Missingness Dependency

To better understand why some recipes are missing avg_rating, I performed two permutation tests. Each test examines whether the missingness of avg_rating depends on another observed variable.

1. Missingness of avg_rating vs. is_main_dish

The first permutation test checks whether the probability that avg_rating is missing differs between main-dish recipes and non–main-dish recipes.

**Null Hypothesis:** The missingness of average ratings does not depend on the fact that a recipe is a main-dish or not.

**Alternate Hypothesis:** The missingness of average ratings does depend on the fact that a recipe is a main-dish or not.

**Test Statistic:** The absolute difference of mean in the proportion of sugar of the distribution of the group without missing ratings and the distribution of the group without missing ratings.

**Significance Level:** 0.05

I ran a permutation test by shuffling the missingness of average rating 1000 times to collect 1000 simulated mean differences in the two distributions.

<iframe
  src="assets/missingness_ismaindish.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The red line on the graph represents the **observed statistic** of **-0.003**, meaning that meaning main dishes have slightly lower missingness than non-main dishes. Since the **p_value** that I found **(0.009)** is < 0.05, I **rejected the null hypothesis**. The missingness of `avg_rating` does depend on `is_main-dish` (whether a recipe is a main dish or not)

2. Missingness of avg_rating vs. sodium (PDV).

The second permutation test checks whether the missingness of avg_rating depends on the `sodium` column.

**Null Hypothesis:** The missingness of ratings does not depend on the sodium content.

**Alternate Hypothesis:** The missingness of ratings does depend ont the sodium content.

**Test Statistic:** The absolute difference in average sodium between recipes with missing `avg_rating` and non-missing `avg_rating`.

**Significance Level:** 0.05

<iframe
  src="assets/missingness_sodium.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The red line on the graph represents the **observed statistic** of **-0.667**, meaning that mean main dishes have slightly lower missingness than non-main dishes. Since the **p_value** that I found **(0.7696)** is > 0.05, I **failed to reject the null hypothesis**. The missingness of `avg_rating` does depend on `is_main-dish`.

### Summary of Missingness Findings


## Hypothesis Testing

To further explore the relationship between protein content and average ratings, I set up the following hypotheses: 

### Hypotheses:

Question: Do recipes with higher protein levels tend to have higher average ratings than recipes with low protein levels? 

Approach: I compared the average ratings of recipes with protein content above the median versus those at or below the median. Since main dishes generally contain more protein, I performed the test within main dishes only to reduce confounding.

**Null Hypothesis (H0):** There is no difference in average ratings between high protein and low protein recipes (mu high protein - mu low protein = 0)

**Alternative Hypotheses (H1)** There is a difference in average ratings between high protein and low protein recipes (mu high protein - mu low protein > 0)

**Test statistic:** I used the difference in mean average ratings between high-protein and low-protein recipes as our test statistic (high - low)

**Method:** I performed a permutation test by randomly shuffling the high/low protein labels 5000 times to simulate the distribution of mean differences under the null hypothesis.

**Reasoning:** I chose a permutation test to compare average ratings between high-protein and low-protein recipes because it does not assume a specific distribution for the ratings. Recipe ratings are ordinal and may not follow a normal distribution, so traditional parametric tests like a t-test might not be fully appropriate. The permutation test is non-parametric, making it robust to the skewed and potentially irregular distribution of ratings in the dataset.

Additionally, the test allows me to directly measure the difference in mean ratings between groups under the null hypothesis that protein content does not affect ratings. By randomly shuffling the high/low protein labels many times (5,000 permutations), I can generate a distribution for the mean difference and accurately assess whether the observed difference is statistically significant. This approach is suitable for observational data like this, where confounding factors may exist, and I want a method that is both flexible and rigorous.

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
- Caveat: This is an observational analysis: confounding variables (recipe type, sweetness, ingredient quality) could explain part of any observed relationship.

Since the **p-value is greater than 0.05**, we **fail to reject the null hypothesis**. This indicates that there is no evidence from our data that high-protein recipes receive higher average ratings than low-protein recipes. In our sample, the observed difference in ratings was slightly negative, suggesting that low-protein recipes had somewhat higher average ratings. However, this observation does not provide conclusive proof, and the difference could be due to random variation.

## Prediction Problem

**Prediction Problem:** Predict the protein level of recipes.
**Type of prediction problem:** regression (protein is a continuous variable)

**Response variable:** The response variable is `protein` (PDV). We chose this variable because protein is a key nutritional component that influences how a recipe fits into a diet or meal plan. Accurate prediction of protein content can help users make informed dietary choices.

**Evaluation metrics:** 
We chose **RMSE** and **R²** because they are well-suited for regression problems and provide complementary insights:

- **RMSE** measures the average magnitude of prediction errors in the same units as the response variable (protein (PDV)). This makes it **directly interpretable** and easy to communicate. We preferred RMSE over Mean Absolute Error (MAE) because RMSE penalizes larger errors more heavily, which is useful if large deviations in protein prediction are particularly undesirable.

- **R²** indicates the proportion of variance in protein content that is explained by the model. This gives a sense of **how well the model captures the underlying patterns** in the data beyond just the average error. We chose R² over metrics like adjusted R² because we are primarily comparing models with similar numbers of features and want a straightforward measure of explanatory power.

Together, RMSE and R² provide a **balanced assessment** of both prediction accuracy (RMSE) and model fit/explanatory ability (R²), making them more informative than using a single metric alone.

***Features at the time of prediction:*** Only information available before cooking is used to predict protein content. For the baseline model, this includes numerical features like **calories and carbohydrates, and text-based tags converted with TF–IDF.** Features such as user ratings, reviews, or any post-cooking metrics are excluded, because they would only be available **after the recipe is completed**. This ensures the model makes predictions based on realistic, actionable information.


## Baseline Model

For the baseline model, I used a **linear regression model** to predict the protein content of recipes. The dataset was split into training and test sets. The model used the following features:

- Calories (PDV) (quantitative - continuous numeric variable)
- Carbohydrates (PDV) (quantitative - continuous numeric variable)
- tags (text data converted into numerical features using TF–IDF)
    - Originally nominal, but after TF–IDF vectorization, they become numerical features (continuous values between 0 and 1).

The numerical features were standardized using StandardScaler so they were on comparable scales before training. For the text-based tags column, I used TF-IDF Vectorization to convert the words into numerical representations that the model could train from.

The baseline model achieved a **Train RMSE of 33.127** and a **Test RMSE of 26.898**, along with a **Train R² of 0.601** and a **Test R² of 0.642.** While the model explains a moderate amount of variance (Test R² = 0.642), the RMSE of ~27 indicates that predictions can **still be off by a substantial amount**. Thus, this baseline provides a simple, interpretable starting point but would likely need additional features or more sophisticated modeling to achieve high predictive accuracy. My current model is "good," but there is definitely room for improvement.

##  Final Model

For our final model, I used a **Random Forest Regressor** to predict the protein content of recipes. Random Forest can capture nonlinear relationships and interactions between nutritional and textual features, which a linear model might miss. This model builds on our baseline by incorporating engineered nutritional features and text-based features extracted from recipe tags.

### Features Used:

I trained the model using the following features:

- log_calories
  - I applied a log transformation to the calorie values using log(calories + 1) to reduce skew and limit the impact of extreme outliers. This is important because extremely high-calorie recipes could disproportionately influence the model, while most recipes have moderate calorie content.

- cal_to_carb_ratio: I created a calorie-to-carbohydrate ratio feature: calories / (carbohydrates + 1)
  - This captures the nutritional balance of each recipe more effectively than raw values alone. Recipes with higher calorie-to-carb ratios are more likely to contain protein-rich ingredients such as meat, eggs, or nuts.

- carbohydrates
  - I included total carbohydrate content as a direct macronutrient feature. Protein content may correlate with other macronutrients; for instance, recipes high in carbohydrates but low in protein may have different ingredients than balanced or protein-rich recipes.

- tag_str
  - I combined recipe tags into a single string and used TF-IDF vectorization to extract useful text-based features from the tags. Tags often describe ingredients or preparation methods (“chicken”, “tofu”, “beans”), which are strong indicators of protein content. Encoding this textual information allows the model to learn associations between specific tags and protein levels.

### Preprocessing and Model Pipeline

I built a pipeline that:

- Standardized all numeric features using StandardScaler
- Converted text features into numerical form using TF-IDF Vectorization
- Trained a Random Forest Regressor for prediction

### Hyperparameter Tuning

I used GridSearchCV to tune the model and selected the best hyperparameters based on minimizing RMSE:

Best Parameters:

- n_estimators = 50
- max_depth = None
- min_samples_leaf = 1

### Model Performance

I evaluated the model using RMSE and R² on both the training and test sets:

| Metric   | Training|   Test  |
|:---------|--------:|--------:|
| RMSE     |  12.20 | 22.26 |
| R²       |  0.95  | 0.75  |

The final model explains about **75%** of the variance in protein content on unseen data, indicating strong predictive performance. The gap between training and test performance suggests some overfitting, but overall the model generalizes well. The Test RMSE decreased from **26.898 → 22.26** (an improvement of ~4.64 points). Additionally, the Train R² increased from 0.601 → 0.95, indicating the model captures complex relationships in the training set.

### Interpretation

This final model significantly improves over the baseline by:
- Using **engineered nutritional ratios**
- Incorporating **text information** from recipe tags

Overall, the model provides accurate and useful predictions of recipe protein content, while demonstrating reasonable generalization beyond the training data.

## Fairness Analysis

To evaluate fairness of our final model, we examined whether the model’s performance differed between main-dish recipes and non-main dishes. It is important to ensure the model predicts fairly across these groups.

I split the test set into two groups based on the is_main_dish column:

- Main dishes (is_main_dish = True)
- Non-main dishes (is_main_dish = False)

I then computed the RMSE for each group to measure predictive performance.

**Null Hypothesis (H0):** The model’s RMSE is similar across main-dish and non-main-dish recipes; any observed difference is due to random chance. (RMSE_main - RMSE_non-main = 0)

**Alternative Hypothesis (H1):** The model’s RMSE differs between main-dish and non-main-dish recipes. (RMSE_main - RMSE_non-main != 0)

**Test Statistic:** Difference in RMSE between main-dish and non-main-dish groups (RMSE_main - RMSE_non-main).

**Significance Level:** 0.05

### Results

<iframe
  src="assets/fairness_analysis.html"
  width="800"
  height="600"
  frameborder="0"
></iframe> 

RMSE by group:

| is_main_dish | RMSE|
|:-------------|----:|
| False        |21.80|
| True         |23.28|

**Observed RMSE difference (RMSE_main - RMSE_non-main):** 1.49

**Permutation p-value:** 0.89

To run the permutation test, we first calculated the prediction errors (RMSE) for main-dish and non-main-dish recipes in the test set. The observed difference in RMSE between main and non-main dishes was 1.49 (main - non-main). To test whether this difference could have arisen by random chance, we performed a permutation test.

We randomly shuffled the is_main_dish labels **2,000 times,** recalculating the RMSE difference for each shuffled dataset. This generated a null distribution of RMSE differences under the assumption that the model is fair (i.e., group membership does not affect prediction error).

After comparing the observed RMSE difference to this null distribution, we obtained a p-value of 0.89. Since the p-value is greater than 0.05, **we fail to reject the null hypothesis.** This indicates that the model’s prediction error does not differ significantly between main-dish and non-main-dish recipes, suggesting that the model is reasonably fair with respect to recipe type.