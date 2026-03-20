---
layout: home
---

This website explores the relationship between cooking time and the mean recipe rating.






## Introduction

In today’s fast-paced world, time is our most valuable asset. With the convenience of eating out and food delivery services just a tap away, the traditional act of home cooking often feels like a luxury we can't afford. In such a busy and stressful environment, our bodies need high-quality fuel to keep us going, yet we frequently find ourselves choosing between a long, complex recipe and the ease of a quick meal. This often raises a dilemma: are we able to create short efficient meals that are just as good as the ones that take hours to make? In this project we will be exploring the relationship between cooking time and recipe's rating to determine if efficiency in the kitchen comes at the cost of a quality meal. This project explores a dataset of recipes and ratings containing data since 2008 from https://www.food.com/. The data was originally scraped and used for a paper on recommender systems.

The first dataset, `recipes`, contains 83,782 rows, 1 row for every unique recipe, 
with 12 columns recording the following information:

| Column | Description |
|--------|-------------|
| `name` | Recipe name |
| `id` | Recipe ID |
| `minutes` | Minutes to prepare recipe |
| `contributor_id` | User ID who submitted this recipe |
| `submitted` | Date recipe was submitted |
| `tags` | Food.com tags for recipe |
| `nutrition` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" |
| `n_steps` | Number of steps in recipe |
| `steps` | Text for recipe steps, in order |
| `description` | User-provided description |
| `ingredients` | Text for recipe ingredients |
| `n_ingredients` | Number of ingredients in recipe |



The second dataset, `interactions`, contains 731,927 rows and 5 columns where each row contains 
a review from a user on a specific recipe. The columns it includes are:

| Column | Description |
|--------|-------------|
| `user_id` | User ID |
| `recipe_id` | Recipe ID |
| `date` | Date of interaction |
| `rating` | Rating given |
| `review` | Review text |

To explore the relationship between time investment for a recipe and the rating the columns that are most relevant are `minutes`: The total number of minutes required to prepare the meal, serving as our primary measure of "cooking time" and `n_steps`: The number of steps in a recipe, which provides additional context on the complexity and effort required. Also we created a new column `mean_rating` which is the mean rating of every unique recipes, because most of the recipes have multiple ratings. I also created a column called, `time_category` which categorizes recipes depending on the time it takes to prepare the recipe: ('0-15', '16-30', '31-60', '61-120', '120+'). 




## Data Cleaning and Exploratory Data Analysis

To prepare the Food.com dataset for an investigation into cooking times and ratings, I performed several key cleaning steps to ensure the data was accurate and usable:

1. Merged Recipes and Ratings: I performed a left merge of the recipes dataset with the ratings dataset. This ensures that every recipe is kept in the final DataFrame, even those that have not yet received a rating, which is crucial for maintaining an unbiased look at all available cooking options.

2. Handled "Zero" Ratings: I replaced all ratings of 0 with np.nan. Users can leave a review without a star rating, which the system records as a 0. Since the actual scale is 1–5, these 0s do not represent "zero stars" but rather missing data. Treating them as NaN prevents them from artificially dragging down the average ratings of recipes

3. Calculated Average Ratings: I calculated the mean and median rating for each unique recipe and merged these back into the main dataset. This allows me to analyze each recipe’s overall "quality" as a single data point.
   
4. Extracted Nutrition Data: The nutrition column originally contained strings that looked like lists (e.g., "[150.2, 5.0, ...]"). I processed these strings into individual numerical columns: calories, total_fat, sugar, sodium, protein, saturated_fat, and carbohydrates.

5. Removed Outliers: I filtered out the top 1% of entries for minutes and all nutrition columns. In a real-world setting, recipes claiming to take thousands of minutes or containing impossible amounts of sugar are likely data entry errors that would skew the results of the "average" quick meal.

**OUR CLEAN DF HAS _ ROWS AND _ COLUMNS** DO THIS AND ADD DF



**Mean Rating Distribution**
For univariate analysis graph I desicded to do a distribution of `mean_ratings`....
<iframe src="mean_rating_distribution.html" 
        width="800" 
        height="450" 
        frameborder="0">
</iframe>




**Time Category Box Plot**
For the bivariate analysis graph I decided to do a box plot of `time_categories`.....
<iframe src="box_plot_fig2.html" 
        width="800" 
        height="450" 
        frameborder="0">
</iframe>



**Pivot Table**

The pivot table below shows the average rating, median rating, average calories, and count of recipies for each time category. Several interesting patterns I noticed was for shorter recipes (0 - 15 minutes) tend to receive higher ratings mean rating (4.71) while the longest recipes (120+ minutes) receive the lowest rating (4.62). This suggest a slight negative relationship between cooking time and rating. Secondly, average calories steadly increase with cooking time, this makes sense because longer recipes tend to be more elaborate meal. Finally, the 31-60 minute category contains the most recipes (69,381), shaped like an asymmetric bell curve. This indicates that the most common cooking time range is between (31-60) on Food.com.

| time_category   |   mean_rating |   median_rating |   avg_calories |   count |
|:----------------|--------------:|----------------:|---------------:|--------:|
| 0-15            |          4.71 |            4.91 |         301.62 |   48435 |
| 16-30           |          4.68 |            4.86 |         369.7  |   58155 |
| 31-60           |          4.67 |            4.86 |         432.15 |   69381 |
| 61-120          |          4.68 |            4.89 |         564.28 |   31818 |
| 120+            |          4.62 |            4.8  |         547.6  |   23872 |





## Assessment of Missingness

**MNAR Analysis**: One of the columns that I believe is MNAR is the `rating` because the probability of that value being missing is related to the rating value itself. Users who thought the recipe was mediocre or thought the recipe was nothing special would be less likely to leave a review compared to users who either loved or hated the recipe. Additionally, time-consuming or very difficult recipes may go unrated simply because fewer people are attempting those recipes and even fewer would go and leave a review. Addintional data I would need to make the missingness MAR is the time since the recipe was released because typically newer recipes have less rating simply because less time has passed since release.

To potentially transition the missingness of `rating` from MNAR to MAR, I would want obtain the **time since the recipe was released** which would be the `submitted` column. Newer recipes might have fewer ratings simply because they have had less exposure time, and accounting for this facotr could help explain the missingness of `rating` through other observed variables.



### Missingness Dependency

To further investigate the missingess of `rating`, I performed a permutation test using the fast-permutation method to see if its missingness depends on other columns in the dataset.
1. **DOES Depend:** `n_steps`
- **Null Hypothesis:** The Distribution of `n_steps` is the same whether or not the `rating` is missing.
- **Alternative Hypothesis:** The distribution of `n_steps` is different when the `rating` is missing compared to when it is not.
- **Test Statistic:** Absolute difference in group means
- **Significance Level:** 0.05

<iframe src="missingness_n_steps.html" 
        width="800" 
        height="450" 
        frameborder="0">
</iframe>

  **Result:** With the observe statistic of 1.3390 and a p-value of 0.00, I reject te null hypothesis. The missingness of `rating` **does not** on the number of steps in a recipe. We can conclude that the complexity of a recipe as measured by steps influences whether or not a user provides a rating.



2. **Does NOT Depend:** `minutes`
- **Null Hypothesis:** The distribution of minutes is the same whether or not the rating is missing
- **Alternative Hypothesis:** The distribution of minutes is different when the rating is missing compared to when it is not
- **Test Statistic:** Absolute difference in group means
- **Significance Level:** 0.05

<iframe src="missingness_minutes.html" 
        width="800" 
        height="450" 
        frameborder="0">
</iframe>


- **Result:** With an observed statistic of 51.4620 and a p-value of 0.1170, I fail to reject the null hypothesis. The missingness of rating does not depend on the cooking time in minutes. At a 5% significance level, there is no evidence that the length of time a recipe takes to prepare affects the likelihood of it being rated



## Hypothesis Testing

**Null Hypothesis:** The average rating for short recipes (≤30 minutes) and long recipes (>30 minutes) come from the same distribution. Any observed difference in means is due to random chance. 

**Alternative Hypothesis:** Short recipes (≤30 minutes) receive higher ratings than long recipes on average.
- **Test Statistic:** The difference in mean rating (Short - Long)
- **Significance Level:** 0.05

I choose 30 minutes as the dividing line because it is a standard benchmark for a "quick" meal. A permutation test was the most appropriate method because it allows us to compare the distribution of two groups without assuming that the rating follow a specific shape. The **difference in means** directly answers whether one group of recipes is, on average, "better" then the others according to user feedback. 

<iframe src="hypothesis_test.html" 
        width="800" 
        height="450" 
        frameborder="0">
</iframe>

The result after running 3,000 permutations, the test produced a Observed Difference of 0.0339 and a p-value of 0.0. Since the p-value is less than 0.05, I reject the null hypothesis. This result suggest that the difference in ratings between short and long recipes is statistically significant and not likely due to random chance alone. The observed difference indicates that **short recipes tend to have slightly higher average ratings**, while we cannot conclude that shorter cooking times _cause_ higer ratings, this provides evidence that you do not need to spend hours in the kitchen to achieve a highly-rated, satisfying meal.


## Baseline Model

In this stage of the project, I developed a baseline regression model to predict the **average rating** of a recipe. I used a **Linear Regression** model implemented by sklearn and split the data by training set and saving 20% for test set. The features I decided to use for this model `n_steps`, `n_indgredients`, `minutes`, and `calories`. 

The features I choose are all **quantitative** and since they exist on very different scales, I applied a `StandardScaler`. This transformation centers the data around a mean of zero with a standard deviation of one, ensuring that no single feature dominates the model simply because of it's magnitude.

###Performance###
The model's performance was evaluated using **Root Mean Squared Error (RMSE)** and the R^2 score. As a result the Training RMSE was 0.4978 and R^2 was 0.0002, while the test RMSE was 0.4961 and R^2 was 0.0001. Currently, showing that this baseline model is **not very efficient** at predicting recipe ratings. While the RSME is relatively low (predictions are on average, within 0.5 stars of actual rating), the R^2 **score is near zero**. Such a low R^2 indicates that this model explains almost none of the variance in ratings. Since mean ratings are tightly clustered near 4-5 stars, it makes it difficult for simple linear regression with basic recipe attributes to predict a meaningful differences.

This suggest that basic metrics like cooking time and step count are not primary drivers of user satsfaction. To improce the model for the step, I will need to engineer more complex features or non-linear relationships that better capture why people love certain recipes.

## Final Model

For the Final Model I transitioned from simple Linear Regression to a **Random Forest Regressor**. This algorithm is better suited for capturing non-linear relationship between recipe characteristics and user ratings. I engineered **three new features** to better represent the "complexity" and "feel" of a recipe: 

1. Complexity Score (`n_steps` * `n_ingredients`): I created an interaction feature by multiplying the number of steps by the number of ingredients. A recipe with many steps and many ingredients is fundamentally more complex than one that has only one of those traits. This captures the synergy between these two variables.
2. Log-Transformed Minutes: Since cooking times are heavily **right-skewed** (most recipes are short, but a few take a very long time), I used a `FunctionTransformer` to apply a log transformation. This compresses the long tail of the data, allowing the model to treat proportional differences (e.g., 30 vs. 60 minutes) more equally.
3. Quantile-Transformed Calories: Nutritional data is often skewed by extreme outliers. I used a    `QuantileTransformer` to map the distribution of calories to a normal distribution. This prevents the model from being distorted by recipes with unusually high caloric counts.
4. For remaining quantitative features, `n_steps`, `n_indgredients`, `complexity_score` I used `StandardScaler`.

Finally organizing them into into a `ColumnTransformer` and then a single `Pipeline`.

###Hyperparameter Tuning###
To find the optimal configuration for my Random Forest, I used `GridSearchCV` with 5-fold cross-validation. I specifically chose to tune the following hyperparameters:

- `max_depth`: To balance the model between underfitting (too shallow) and overfitting (too deep).
- `n_estimators`: To determine the minimum number of trees needed to provide stable and accurate predictions.
The search identified the best parameters as: `{'max_depth': None, 'n_estimators': 200}`

The final model showed substantial improvement over the baseline, improving the test RMSE by 0.1385 from 0.4961 to 0.3576. But, most importantly, R^2 **score** increased from nearly zero in the baseline to **0.4767** in the final model. Now the model can explain nearly **48% of the variance** in recipe ratings. 

## Fairness Analysis
