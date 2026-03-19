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

To explore the relationship between time investment for a recipe and the rating the columns that are most relevant are `minutes`: The total number of minutes required to prepare the meal, serving as our primary measure of "cooking time". Also we created a new column `mean_rating` which is the mean rating of every unique recipes, because most of the recipes have multiple ratings.

## Data Cleaning and Exploratory Data Analysis

## Assessment of Missingness

## Hypothesis Testing

## Baseline Model

## Final Model

## Fairness Analysis
