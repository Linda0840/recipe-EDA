# Chop Chop! An Analysis on food recipe preparation times and ratings
Authors: Xiaoyan Zhang (xiz115@ucsd.edu), Kay Qu(kqu@ucsd.edu)

# About this project
This is a paired EDA project which is originally assigned by UCSD's DSC80 offering during SP23. 
we will be focusing on cleaning, exploreing, and visualizing data scraped from [food.com](food.com). 
More information about the project's guidelines can be found [here](https://dsc80.com/project3/recipes-and-ratings/).

# Part I: Introduction

## Research Question

We would like to investigate the relationship between Time (in minutues) to prepare a recipe and whether it correlates with the average rating that the recipe receives. We hypothesize that the longer time a recipe requires, the less rating it will receive. We will be implementing hypothesis testing techniques to investigate this question. 

This is a question worth investigating because through analyzing the relationship between cooking time and ratings, we can potentially look into whether this trend (or the lack thereof) is related to years when the users provided their ratings. We can then preform meta-analysis on whether users are becoming less fond of lengthy recipes as time passes. This can be a future research topic that is worth investigating. 

## About our data
Our data is derived from [food.com](https://www.food.com), originally scraped and used by them. The raw csv files can be found [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).

Below are the breif description of the raw data that we are going to perform cleaning on: 

### raw_recipes (83782 rows, 12 columns)

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

### raw_ratings (731927 rows, 5 columns)

| Column       | Description           |
|--------------|-----------------------|
| 'user_id'    | User's ID               |
| 'recipe_id'  | Recipe's ID             |
| 'date'       | Date when the rating was posted   |
| 'rating'     | rating, scaling from 1-5          |
| 'review'     | user's review message about the recipe           |

# Part II: Cleaning and EDA (Exploratory Data Analysis)

## Data Cleaning

We begin our data cleaning by merging the two dataframes: `raw_recipes` and `raw_iteractions`together. We perform a left merge on both dataframe's `recipe_id` (formatted as `id` in `raw_recipe`) so that the resulting `merged` dataframe will have all recipes from the `raw_recipes` dataframe with information from the `raw_interactions`. 

A more in-depth guide about merging can be found under [this Stack Overflow post](https://stackoverflow.com/questions/53645882/pandas-merging-101).

Then we replace all illegal ratings (e.g. 0) with `Nan`. This step is to prevent numerical missing data from affecting certain aggregation methods, such as when looking for average rating using `.mean()`. Since aggregation methods ignores `NaN` as default, we can safely proceed to the next step. 

Next, since we are interested in finding trends for the average ratings amongst our recipes, we would like to also have a column with  `average_rating` data. We perform a left merge again on `raw_recipes` with `average_rating`, resulting in a dataframe called `recipes` with additional column that contains information about `rating`.

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


## Univariate Analysis

In this section, we performed basic individual visualization on columns that we are interested in : `minutes` and `rating` (average rating).

<iframe src="assets/Part_1_Histogram_of_Minutes.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can observe that more than 64% of the `recipes` can be made in under 50 minutes, and almost 90% of the `recipes` can be prepared in under 100 minutes.

<iframe src="assets/Part_1_Histogram_of_Average_Rating.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can see that the average `ratings` of `recipes` are overall positive, with around 64% of values ranging at a solid 5/5. The rest of the `ratings` are distributed at around 3-4, and there are very few ratings that are below 2.

## Bivariate analysis <a id="biv-analysis"></a>

In this section, we will be exploring the relationship between the two variables we are interested in: `minutes` and `rating` (average rating). 

<iframe src="assets/Part_1_Scatter_Plot.html" width=500 height=550 frameBorder=0></iframe>

From the chart above, we can observe that there seems to be some correlation between `minutes` and `rating`. This observation is based on the fact that we see more ratings on the higher end of the spectrum when the recipe's time in minutes is shorter. We would like to further investigate if that is truly the case. 

## Interesting Aggregates

<iframe src="assets/Part_1_Pivot.html" width=600 height=400 frameBorder=0 margin-bottom=0 padding- bottom=0></iframe>


For the pivot table above, we attempted to visualize `n_steps`  and `n_ingredients`, and aggregating their average ratings. The result does not show an obvious trend, but we can prelimanarily hypothesize that as number of steps and number of ingredients increase, the average rating for the recipe will decrease. This will not be the main focus of this EDA, but is a potential future area of interest.

# Part III: Assessment of Missingness

## NMAR Analysis

### Why NMAR?

In real-life data, missingness is something that we will always encounter. We would like to investigate if there is a relationship between `rating` and `time`, in minutes. Through cleaning and examining data, we obsereved a few columns with missing values, such as `rating`, `review`, `description`. One potential column where missingness of type NMAR could occur is in the `rating` column. 

**NMAR** occurs when the missingness is _dependent_ on certain unknown traits from themselves. We would have to use some domain knowledge to determine whether there is a possibility of **NMAR**. 

 Online environment is usually polarizing and people tend to express extreme opinions on things that they feel strongly about[(see: Group Polarization)](https://en.wikipedia.org/wiki/Group_polarization). In terms of ratings, online users might be more inclined to leave a review when they either feel like the recipe is exceptionally good (perhaps giving it as 5/5), but there will also be folks who would leave a (1/5) if the recipe does not suit their taste. 
 
 Therefore, the extremeness of the ratings themselves could be correlated with their missingness, with more missing data amongst ratings with a more moderate score (e.g. 3). 

_(Note: more complete information about types of missingness can be found [here](https://www.ncbi.nlm.nih.gov/books/NBK493614/))_

### Could It Be Other Missingness?

However, we can test for MAR for the column we've selected as well. While **NMAR** can only be deduced by reasoning, we can determine whether a column's data is either:
- **MCAR (Missing Completely at Random)**, or
-**MAR (Missing at Random)**.

For example, while we cannot prove or disprove that the column `rating` has some inherent underlying information that affects its missingness, we can test its dependencies with other columns. 

If we are able to discover a dependent relationship between `rating` and other columns present in the dataset, we can then argue that our column is **MAR**.

## Missingness Dependency

We want to determine whether the missingness is: 
- **MCAR (Missing Completely At Random),** or
- **MAR (Missing At Random).**,

and this can be achieved by performing column dependency tests.

## Rating Missingness by minutes

We first performed a __permutation testing__ to determine whether missingness of `rating` was __dependent__ on minutes.

<iframe src="assets/Part_2_Fail_To_Reject.html" width=600 height=500 frameBorder=0></iframe>

From the graph, we can see that there are some visible difference in the distribution of minutes when comparing missing and non-missing data. We performed a permutation testing by using __difference in means__ as test statistic to determine if these two groups could be from the same population: 

<iframe src="assets/Part_2_Minutes.html" width=600 height=550 frameBorder=0></iframe>


The observed test statistic is about 117.34 (difference in 'minutes' means between missing and non-missing 'rating' is 117.34 minutes). The test result yielded a p-value of __`0.037`__, given that we use __α = 0.01__ as our significance level, we __fail to reject the null hypothesis__, which is a sign that the missingness is potentially due to random chance (belong to the same population), hence we cannot conclude that the missingness of `rating` is dependent on `time`. 

## Rating Missingness by n_ingredients

Then, we tried to investigate the realtionship between missingness in `rating` and `n_ingredients`. We also hypothesized that the missingness is dependent on `n_ingredients`, with similar reasoning mentioned above. 

<iframe src="assets/Part_2_Reject_Ingredients.html" width=600 height=550 frameBorder=0></iframe>

From the graph, we can still see that there are some visible difference in the distribution of minutes when comparing missing and non-missing data. We performed a permutation testing  by using __difference in means__ as test statistic to determine if these two groups are from the same population: 

<iframe src="assets/Part_2_Ingredients.html" width=600 height=550 frameBorder=0></iframe>

The observed test statistic is about 0.254 (difference in means of 'n_ingredients' between missing and non-missing 'rating' is 0.254 ingredients). The test result yielded a p-value of __`0.001`__, and given that we still use __α = 0.01__ as our significance level, we __reject the null hypothesis__, which means that the missingness in `rating`is likely to be dependent on `n_ingredients`.

## Rating Missingness by n_steps

Additionally, we attempted to visualize the difference in missingness among different `n_steps`:

<iframe src="assets/Part_2_Reject_Steps.html" width=600 height=550 frameBorder=0></iframe>

Then we performed a permutation testing by using __difference in means__ to determine if these two groups could be from the same population: 

<iframe src="assets/Part_2_Steps.html" width=600 height=550 frameBorder=0></iframe>

The observed test statistic is about 1.492 (difference in means of 'n_steps' between missing and non-missing 'rating' is 1.492 steps). The test result yielded a p-value of __`0.00`__, and given that we still use __α = 0.01__ as our significance level, we __reject the null hypothesis__, which means that the missingness is likely to be dependent on `n_steps`.



# Part IV: Hypothesis Testing

## Setting up Relevant Hypothesis

In this section, we will finally investigate the question we came up at the beginning of this report: 

**_Do recipes with longer cooking time receive lower ratings?_**

In order to investigate this question, we need to first statistically define our independent variable: `cooking time`.

We used a quartiled approach and split our data in to four quadrants by `time` . We define our first quartile to be group of **short cooking time** and fourth quartile to be group of **long cooking time**. We chose this partitioning condition because from the previous figure on the [Bivariate Distribution](#bivariate-analysis-), a large portion of scatter points are clustered in the left-up corner of the plot with relatively short cooking time yet high rating scores.

Now, we are ready to begin by setting up our null hypothesis (no change):

- **The difference of average rating between q1 and q4 (q1-q4) is 0  (difference = 0)**

Then we state our alternative hypthesis:

- **The difference of average rating between q1 and q4 (q1-q4) is positive  (difference > 0)**

## Performing the Test

The best way to answer this question is still by performing a permutation testing to determine if q1 and q4 come from the same population. We will be using **signed difference in means of averages** as test statistic. We use **signed** difference since our question of interest is one-sided (whether observed is larger than expected, so we need statistics that inform the direction). 

<iframe src="assets/Part_3.html" width=600 height=550 frameBorder=0></iframe>


The test result yielded a p-value of __`0.00`__, and given that we still use __α = 0.01__ as our significance level, we __reject the null hypothesis__, which means that recipes with lower cooking time could be **more likely** to have a higher rating compared to those with longer cooking time. 

However, it is important to note that we **cannot guarantee** for sure that our conclusion is correct, since we are performing a hypothesis testing. We can only conclude that it is **likely** that the recipes with lower cooking time have higher ratings.
