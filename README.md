# A Preliminary EDA on Recipes from food.com
Authors: Linda Zhang (xiz115@ucsd.edu), Kay Qu(kqu@ucsd.edu)

# About this project
this is a paired EDA project which is originally assigned by UCSD's DSC80 offering during SP23. 
we will be focusing on  cleaning, exploreing, and visualizing data scraped from [food.com](food.com). 
More information about the project's guidelines can be found [here](https://dsc80.com/project3/recipes-and-ratings/).

# Part I: Introduction

## Research Question

We would like to investigate the relationship between Time (in minutues) to prepare a recipe and whether it correlates with the average rating that it receives. We hypothesize that ... (TBD). We will be implementing permutation testing techniques to investigate this question. 

## About our data
our data is derived from [food.com](https://www.food.com), originally scraped and used by them. The raw csv files can be found [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

Below are the breif description of the raw data that we are going to perform cleaning on: 

### Recipes (83782 rows, 12 columns)

| Column           | Description                                          |
|------------------|------------------------------------------------------|
| 'name'           | Name of the recipe                                          |
| 'id'             | Recipe's ID                                            |
| 'minutes'        | Time (in minutues) to prepare recipe                            |
| 'contributor_id' | Recipe contributor's ID                     |
| 'submitted'      | Date when the recipe was submitted                            |
| 'tags'           | Categorical tags for recipe                             |
| 'nutrition'      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps'        | Number of steps in recipe                            |
| 'steps'          | Description for 'n_steps', in order                       |
| 'description'    | additional description provided by user                             |

### Ratings (731927 rows, 5 columns)

| Column       | Description           |
|--------------|-----------------------|
| 'user_id'    | User's ID               |
| 'recipe_id'  | Recipe's ID             |
| 'date'       | Date when the rating was posted   |
| 'rating'     | rating, scaling from 1-5          |
| 'review'     | user's review message about the recipe           |

# Part II: Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning

We begin our data cleaning by merging the two dataframes: `raw_recipes` and `raw_iteractions`together. We perform a left merge on both dataframe's `recipe_id` (formatted as `id` in `raw_recipe`) so that the resulting `merged` dataframe will have all recipes from the `raw_recipes` dataframe with information from the `raw_interactions`. A more in-depth guide about merging can be found under [this Stack Overflow post](https://stackoverflow.com/questions/53645882/pandas-merging-101).

Then we replace all illegal ratings (e.g. 0) with `Nan`. This step is to prevent numerical missing data from affecting certain aggregation methods, such as when looking for average rating using `.mean()`. Since aggregation methods ignores `NaN` as default, we can safely proceed to the next step. 

Next, since we are interested in finding trends for the average ratings amongst our recipes, we would like to also have a column with  `average_rating` data. We perform a left merge again, resulting in a dataframe called `recipes` with additional column that contains information about `rating`.

The cleaned DataFrame is displayed below (columns have been separated due to webpage formatting concerns):
<iframe src="assets/recipes_trincated_head.html" width=600 height=600 frameBorder=0 index=False></iframe>


| name                              |     id |   minutes |   contributor_id | submitted   |
|:----------------------------------|-------:|----------:|-----------------:|:------------|
| 1 brownies in the world bes...    | 333281 |        40 |           985201 | 2008-10-27  |
| 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  |
| millionaire pound cake            | 286009 |       120 |           461724 | 2008-02-12  |
| 2000 meatloaf                     | 475785 |        90 |          2202916 | 2012-03-06  |

| tags                              | nutrition                         |   n_steps | steps                             |
|:----------------------------------|:----------------------------------|----------:|:----------------------------------|
| ['60-minutes-or-less', 'time-t... | [138.4, 10.0, 50.0, 3.0, 3.0, ... |        10 | ['heat the oven to 350f and ar... |
| ['60-minutes-or-less', 'time-t... | [595.1, 46.0, 211.0, 22.0, 13.... |        12 | ['pre-heat oven the 350 degree... |
| ['60-minutes-or-less', 'time-t... | [194.8, 20.0, 6.0, 32.0, 22.0,... |         6 | ['preheat oven to 350 degrees'... |
| ['time-to-make', 'course', 'cu... | [878.3, 63.0, 326.0, 13.0, 20.... |         7 | ['freheat the oven to 300 degr... |
| ['time-to-make', 'course', 'ma... | [267.0, 30.0, 12.0, 12.0, 29.0... |        17 | ['pan fry bacon , and set asid... |

| description                       | ingredients                       |   n_ingredients |   rating |
|:----------------------------------|:----------------------------------|----------------:|---------:|
| these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 |        4 |
| this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 |        5 |
| since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |        5 |
| why a millionaire pound cake? ... | ['butter', 'sugar', 'eggs', 'a... |               7 |        5 |
| ready, set, cook! special edit... | ['meatloaf mixture', 'unsmoked... |              13 |        5 |


### Univariate Analysis

In this section, we performed basic visualization on columns that we are interested in : `minutes` and `rating` (average rating).

<iframe src="assets/Part_1_Histogram_of_Minutes.html" width=450 height=450 frameBorder=0></iframe>

From this chart, we can observe that more than 64% of the recipes can be made in under 50 minutes, and almost 90% of the recipes can be prepared in under 100 minutes.

<iframe src="assets/Part_1_Histogram_of_Average_Rating.html" width=450 height=450 frameBorder=0></iframe>

