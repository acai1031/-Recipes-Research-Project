# -Recipes-Research-Project
This project, conducted for DSC80, involves data cleaning, exploratory data analysis, evaluation of missing data, and the implementation of permutation tests. The content on this website serves as a presentation of our research outcomes. <br>
<br>
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
### Cleaning Process
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

2. Convert time information `submitted` and `date` into datetime
3. Convert the value of `tags`, `steps`, and `ingredients` columns into list of strings
4. Create individual columns for each value in the `nutrition` column, titled as `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`
5. Left merge the recipes and interactions datasets together.
6. Fill all ratings of 0 with np.nan. #missing justification.
7. Find the average rating per recipe, as a Series, named as `avg_rating` and assign this series back to the recipes dataset.

### Cleaning Result
1. cleaned recipe dataframe `recipe`
    - shape <br>
    (83782, 19)
    - datatype <br>

    |                | 0              |
    |:---------------|:---------------|
    | name           | object         |
    | recipe_id      | int64          |
    | minutes        | int64          |
    | contributor_id | int64          |
    | submitted      | datetime64[ns] |
    | tags           | object         |
    | n_steps        | int64          |
    | steps          | object         |
    | description    | object         |
    | ingredients    | object         |
    | n_ingredients  | int64          |
    | calories       | float64        |
    | total fat      | float64        |
    | sugar          | float64        |
    | sodium         | float64        |
    | protein        | float64        |
    | saturated fat  | float64        |
    | carbohydrates  | float64        |
    | avg_rating     | float64        |

   - the head of `recipe` DataFrame with important columns: <br>

| name                                 |   recipe_id |   minutes | submitted           |   n_steps |   avg_rating |
|:-------------------------------------|------------:|----------:|:--------------------|----------:|-------------:|
| 1 brownies in the world    best ever |      333281 |        40 | 2008-10-27 00:00:00 |        10 |            4 |
| 1 in canada chocolate chip cookies   |      453467 |        45 | 2011-04-11 00:00:00 |        12 |            5 |
| 412 broccoli casserole               |      306168 |        40 | 2008-05-30 00:00:00 |         6 |            5 |

2. cleaned merged dataframe `df`
    - shape <br>
    (234429, 23)
    - the head of `df` DataFrame with important columns: <br>

| name                                 |   recipe_id | review                                                                                                                                                                                                                                                                                                                                           |   rating |   avg_rating |
|:-------------------------------------|------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------:|-------------:|
| 1 brownies in the world    best ever |      333281 | These were pretty good, but took forever to bake.  I would send it ended up being almost an hour!  Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut.  They did taste quite rich, though!  Made for My 3 Chefs.                                                                                   |        4 |            4 |
| 1 in canada chocolate chip cookies   |      453467 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |        5 |            5 |
| 412 broccoli casserole               |      306168 | This was one of the best broccoli casseroles that I have ever made.  I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                  |        5 |            5 |
|                                      |             | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |          |              |
|                                      |             | Thanks so much for sharing your recipe shapeweaver. It was wonderful!  Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                          |          |              |













<iframe src="assets/Distribution_of_Number_of_Steps.html" width=800 height=600 frameBorder=0></iframe>
