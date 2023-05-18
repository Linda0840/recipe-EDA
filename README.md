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

We begin our data cleaning by merging the two dataframes: `raw_recipes` and `raw_iteractions`together. We perform a left merge on both dataframe's `recipe_id` (formatted as `id` in `raw_recipe`) so that the resulting `merged` dataframe will have all recipes from the `raw_recipes` dataframe with information from the `raw_interactions`. 

A more in-depth guide about merging can be found under [this Stack Overflow post](https://stackoverflow.com/questions/53645882/pandas-merging-101).

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

In this section, we performed basic individual visualization on columns that we are interested in : `minutes` and `rating` (average rating).

<iframe src="assets/Part_1_Histogram_of_Minutes.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can observe that more than 64% of the recipes can be made in under 50 minutes, and almost 90% of the recipes can be prepared in under 100 minutes.

<iframe src="assets/Part_1_Histogram_of_Average_Rating.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can see that the average ratings of recipes are overall positive, with around (TBD) percent of values ranging at a solid 5/5. The rest of the ratings are distributed at around 3-4, and there are very few ratings that are below 2.

### Bivariate analysis

In this section, we will be exploring the relationship between the two variables we are interested in: `minutes` and `rating` (average rating). 

<iframe src="assets/Part_1_Scatter_Plot.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can observe that there seems to be some correlation between `minutes` and `rating`. This observation is based on the fact that we see more ratings on the lower end of the spectrum when the recipe's time in minutes is shorter. We would like to further investigate if that is truly the case. 

### Interesting Aggregates


|   n_steps |       5 |       6 |       7 |       8 |       9 |      10 |      11 |      12 |      13 |      14 |
|----------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|
|         5 | 4.64328 | 4.61548 | 4.60238 | 4.60963 | 4.54944 | 4.57725 | 4.61091 | 4.60648 | 4.6117  | 4.50497 |
|         6 | 4.59967 | 4.64566 | 4.60331 | 4.61542 | 4.56317 | 4.58415 | 4.60216 | 4.60585 | 4.64637 | 4.61832 |
|         7 | 4.64362 | 4.63443 | 4.65464 | 4.59957 | 4.60524 | 4.58246 | 4.60984 | 4.62786 | 4.58493 | 4.66519 |
|         8 | 4.6591  | 4.61752 | 4.6323  | 4.5916  | 4.60628 | 4.61113 | 4.65268 | 4.63326 | 4.65007 | 4.6313  |
|         9 | 4.63867 | 4.63726 | 4.62117 | 4.62457 | 4.62893 | 4.59998 | 4.55509 | 4.61854 | 4.63983 | 4.61253 |
|        10 | 4.66933 | 4.60975 | 4.5827  | 4.62373 | 4.5877  | 4.5778  | 4.59206 | 4.59543 | 4.58221 | 4.6334  |
|        11 | 4.59678 | 4.67157 | 4.62597 | 4.62436 | 4.58784 | 4.61545 | 4.62939 | 4.59178 | 4.63902 | 4.65239 |
|        12 | 4.61807 | 4.60683 | 4.6429  | 4.59205 | 4.60362 | 4.61306 | 4.67451 | 4.59529 | 4.5873  | 4.60679 |
|        13 | 4.7376  | 4.63187 | 4.60628 | 4.64955 | 4.59426 | 4.62241 | 4.60985 | 4.62847 | 4.65895 | 4.57838 |
|        14 | 4.67968 | 4.57864 | 4.5237  | 4.61213 | 4.64867 | 4.543   | 4.62148 | 4.58855 | 4.62266 | 4.57949 |


For the pivot table above, we attempted to visualize `n_steps`  and `n_ingredients`, and aggregating their average ratings. The result does not show an obvious trend, but we can prelimanarily hypothesize that as number of steps and number of ingredients increase, the average rating for the recipe will decrease. This will not be the main focus of this EDA, but is a potential future area of interest.

# Part III: Assessment of Missingness

In real-life data, missingness is something that we will always encounter. We would like to investigate if there is a relationship between `rating` and `time`, in minutes. We hypothesize that the missingness of `rating` is **dependent** on `time`, in minutes. Which means that we believe the missingness is __NMAR__. For example, a potential reason why missingness in ratings could be attributed to `time`, is that the longer the recipe requires, the less likely people are going to attempt cooking them, and therefore the appearace of `NaN`s in `rating`. 

_(Note: more information about types of missingness can be found [here](https://www.ncbi.nlm.nih.gov/books/NBK493614/))_

We want to determine whether the missingness is: 
- **NMAR (Not Missing At Random),** or
- **MAR (Missing At Random).**

We first performed a __permutation testing__ to determine whether missingness of `rating` was __dependent__ on minutes.

<iframe src="assets/Part_2_Ingredients.html" width=600 height=550 frameBorder=0></iframe>

<iframe src="assets/Part_2_Minutes.html" width=600 height=550 frameBorder=0></iframe>

<iframe src="assets/Part_2_Reject_Steps.html" width=600 height=550 frameBorder=0></iframe>

We first attempted to visualize the difference in missingness among different 

<iframe src="assets/Part_2_Steps.html" width=600 height=550 frameBorder=0></iframe>


The test result yielded a p-value of __`0.037`__, given that we use __α = 0.01__ as our significance level, we __fail to reject the null hypothesis__, which is a sign that the missingness is potentially due to random chance, hence we cannot conclude that the missingness of `rating` is dependent on `time`. 

Then, we tried to investigate the realtionship between missingness in `rating` and `n_ingredients`. We also hypothesized that the missingness is dependent on `n_ingredients`, with similar reasoning mentioned above. 

<iframe src="assets/Part_2_Reject_Ingredients.html" width=600 height=550 frameBorder=0></iframe>

<iframe src="assets/Part_2_Ingredients.html" width=600 height=550 frameBorder=0></iframe>

The test result yielded a p-value of __`0.001`__, and given that we still use __α = 0.01__ as our significance level, we __reject the null hypothesis__, which means that the missingness is likely to be associated with `n_ingredients`.


# Part IV: Hypothesis Testing


