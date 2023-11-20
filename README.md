# -Recipes-Research-Project
This project, conducted for DSC80, involves data cleaning, exploratory data analysis, evaluation of missing data, and the implementation of permutation tests. The content on this website serves as a presentation of our research outcomes. <br>
**Name(s)**: Kristina Wu and Yishan Cai <br>

## Introduction
The pursuit of culinary delights has become a passion for many, with cooking evolving into an act of self-gratification. In Project 3, we delve deep into a comprehensive dataset of recipes and their corresponding user interactions from food.com. These provide an opportunity to dissect and understand the broader patterns shaping culinary practices and to pinpoint the elements that contribute to a recipe's popularity.<br>

Our dataset, sourced from food.com, dates back to 2008 and consists of two primary segments: recipes and interactions. The recipe segment is detailed, encompassing attributes such as the recipe's name, ID, preparation time, contributor's ID, submission date, tags, nutritional information, number of steps involved, the steps themselves, and a description. The interaction segment, on the other hand, captures user engagement through user ID, recipe ID, date of interaction, ratings, and reviews.<br>

A glance at the dataset reveals 83,782 unique recipes and 731,927 interactions, reflecting the extensive community engagement on the platform. By computing an average rating for each recipe, we transform these interactions into measurable insights, painting a clear picture of each recipe's enduring popularity and reception over time.<br>

This project will unfold in stages, starting with a thorough data cleaning process followed by an exploratory data analysis to lay bare the fundamental structure and interconnections within the dataset. As we progress, we will address the issue of data missingness with a focus on NMAR (Not Missing at Random) analysis to understand the intricacies behind any absent data.<br>

In the missingness analysis, there are three columns in the merged dataset that contain missing values, which are `description`, `review`, and `rating`. In the NMAR analysis, our attention is directed towards the `review` column, and provides some plausible explanation regarding the absence of certain reviews. For missingness dependency analysis, we examine the relationship between the `rating` missingness and the number of cooking steps (`n_steps`) as well as the duration of cooking (`minutes`) via dependency test.<br>

Furthermore, we will probe into the research question surrounding consumer preferences and trends, especially in the context of the health-conscious choices reflected in low-calorie recipes. By evaluating whether recipes with lower caloric content are favored with higher ratings, we aim to shed light on whether health considerations weigh in on user preferences. This understanding is paramount for recipe creators and food-oriented enterprises as they navigate the currents of user and consumer needs and health trends, shaping their offerings to align with the tastes and well-being of their audience.<br>

---

## Cleaning and EDA
### Data Cleaning
#### Cleaning Process
1. Checking data type <br>
|                | 0      |
|:---------------|:-------|
| name           | object |
| id             | int64  |
| minutes        | int64  |
| contributor_id | int64  |
| submitted      | object |
| tags           | object |
| nutrition      | object |
| n_steps        | int64  |
| steps          | object |
| description    | object |
| ingredients    | object |
| n_ingredients  | int64  |


print(raw_recipes.dtypes.to_markdown())

2. Convert time information `submitted` and `date` into datetime
3. Convert the value of `tags`, `steps`, and `ingredients` columns into list of strings
4. Create individual columns for each value in the `nutrition` column, titled as `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`
5. Left merge the recipes and interactions datasets together.
6. Fill all ratings of 0 with np.nan. #missing justification.
7. Find the average rating per recipe, as a Series, named as `avg_rating` and assign this series back to the recipes dataset.

#### Cleaning Result
1. cleaned recipe dataframe
    - shape
    (83782, 19)
    - columns
    [`name`, `recipe_id`, `minutes`, `contributor_id`, `submitted`, `tags`, `n_steps`, `steps`, `description`, `ingredients`, `n_ingredients`, `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, `carbohydrates`, `avg_rating`]
    - datatype
    |                | 0              |\n|:---------------|:---------------|\n| name           | object         |\n| recipe_id      | int64          |\n| minutes        | int64          |\n| contributor_id | int64          |\n| submitted      | datetime64[ns] |\n| tags           | object         |\n| n_steps        | int64          |\n| steps          | object         |\n| description    | object         |\n| ingredients    | object         |\n| n_ingredients  | int64          |\n| calories       | float64        |\n| total fat      | float64        |\n| sugar          | float64        |\n| sodium         | float64        |\n| protein        | float64        |\n| saturated fat  | float64        |\n| carbohydrates  | float64        |\n| avg_rating     | float64        |
    - the head of `recipe` DataFrame with important columns:











<iframe src="assets/Distribution_of_Number_of_Steps.html" width=800 height=600 frameBorder=0></iframe>
