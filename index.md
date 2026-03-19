---
layout: home
---

# Investigating the Relationship Between Cooking Time and Recipe Ratings

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

To explore the relationship between time investment for a recipe and the rating the columns that are most relevant are `minutes`: The total number of minutes required to prepare the meal, serving as our primary measure of "cooking time" and `n_steps`: The number of steps in a recipe, which provides additional context on the complexity and effort required. Also we created a new column `mean_rating` which is the mean rating of every unique recipes, because most of the recipes have multiple ratings. I also created a column called, `time_category` which categorizes recipes depending on the time it takes to prepare the recipe: '0-15', '16-30', '31-60', '61-120', '120+'. 

## Data Cleaning and Exploratory Data Analysis

To prepare the Food.com dataset for an investigation into cooking times and ratings, I performed several key cleaning steps to ensure the data was accurate and usable:
Merged Recipes and Ratings: I performed a left merge of the recipes dataset with the ratings dataset
1. This ensures that every recipe is kept in the final DataFrame, even those that have not yet received a rating, which is crucial for maintaining an unbiased look at all available cooking options.

3. Handled "Zero" Ratings: I replaced all ratings of 0 with np.nan. Users can leave a review without a star rating, which the system records as a 0. Since the actual scale is 1–5, these 0s do not represent "zero stars" but rather missing data. Treating them as NaN prevents them from artificially dragging down the average ratings of recipes

4. Calculated Average Ratings: I calculated the mean and median rating for each unique recipe and merged these back into the main dataset. This allows me to analyze each recipe’s overall "quality" as a single data point.
   
6. Extracted Nutrition Data: The nutrition column originally contained strings that looked like lists (e.g., "[150.2, 5.0, ...]"). I processed these strings into individual numerical columns: calories, total_fat, sugar, sodium, protein, saturated_fat, and carbohydrates.

7. Removed Outliers: I filtered out the top 1% of entries for minutes and all nutrition columns. In a real-world setting, recipes claiming to take thousands of minutes or containing impossible amounts of sugar are likely data entry errors that would skew the results of the "average" quick meal.


**Pivot Table**
'|    | time_category   |   mean_rating |   median_rating |   avg_calories |   count |\n|---:|:----------------|--------------:|----------------:|---------------:|--------:|\n|  0 | 0-15            |          4.71 |            4.91 |         301.62 |   48435 |\n|  1 | 16-30           |          4.68 |            4.86 |         369.7  |   58155 |\n|  2 | 31-60           |          4.67 |            4.86 |         432.15 |   69381 |\n|  3 | 61-120          |          4.68 |            4.89 |         564.28 |   31818 |\n|  4 | 120+            |          4.62 |            4.8  |         547.6  |   23872 |'


## Assessment of Missingness

## Hypothesis Testing

## Baseline Model

## Final Model

## Fairness Analysis
