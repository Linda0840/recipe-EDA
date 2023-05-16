# A Comprehensive Analysis on Recipes on food.com
Authors: Linda Zhang (xiz115@ucsd.edu), Kay Qu(kqu@ucsd.edu)

## About this project: 
this is a paired EDA project which is originally assigned by UCSD's DSC80 offering during SP23. 
we will be focusing on  cleaning, exploreing, and visualizing data scraped from food.com.

## Introduction:

### About our data:
our data is derived from [food.com](https://www.food.com), originally scraped and used by them. 

the raw csv files can be found [here](https://drive.google.com/file/d/1kIbMz6jlhleiZ9_3QthmUnifoSds_2EI/view).



#### Recipes

| Column           | Description                                          |
|------------------|------------------------------------------------------|
| 'name'           | Name of the recipe                                          |
| 'id'             | Recipe's ID                                            |
| 'minutes'        | Time (in minutues) to prepare recipe                            |
| 'contributor_id' | recipe's contributor's ID                     |
| 'submitted'      | Date when the recipe was submitted                            |
| 'tags'           | Categorical tags for recipe                             |
| 'nutrition'      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps'        | Number of steps in recipe                            |
| 'steps'          | Text for 'n_steps', in order                       |
| 'description'    | additional description provided by user                             |

#### Ratings

| Column       | Description           |
|--------------|-----------------------|
| 'user_id'    | User ID               |
| 'recipe_id'  | Recipe ID             |
| 'date'       | Date of interaction   |
| 'rating'     | Rating given          |
| 'review'     | Review text           |
