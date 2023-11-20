# -Recipes-Research-Project
This project, conducted for DSC80, involves data cleaning, exploratory data analysis, evaluation of missing data, and the implementation of permutation tests. The content on this website serves as a presentation of our research outcomes. <br>
<br>
**Names**: Kristina Wu and Yishan Cai <br>
<br>

---

## Introduction
The pursuit of culinary delights has become a passion for many, with cooking evolving into an act of self-gratification. In Project 3, we delve deep into a comprehensive dataset of recipes and their corresponding user interactions from food.com. These provide an opportunity to dissect and understand the broader patterns shaping culinary practices and to pinpoint the elements that contribute to a recipe's popularity.<br>

Our dataset, sourced from food.com, dates back to 2008 and consists of two primary segments: `recipes` and `interactions`. The `recipe` segment is detailed, encompassing attributes such as the recipe's name, ID, preparation time, contributor's ID, submission date, tags, nutritional information, number of steps involved, the steps themselves, and a description. The `interactions` segment, on the other hand, captures user engagement through user ID, recipe ID, date of interaction, ratings, and reviews.<br>

A glance at the dataset reveals 83,782 unique recipes and 731,927 interactions, reflecting the extensive community engagement on the platform. By computing an average rating for each recipe, we transform these interactions into measurable insights, painting a clear picture of each recipe's enduring popularity and reception over time.<br>

This project will unfold in stages, starting with a thorough data cleaning process followed by an exploratory data analysis to lay bare the fundamental structure and interconnections within the dataset. As we progress, we will address the issue of data missingness with a focus on NMAR (Not Missing at Random) analysis to understand the intricacies behind any absent data.<br>

In the missingness analysis, there are three columns in the merged dataset that contain missing values, which are `description`, `review`, and `rating`. In the NMAR analysis, our attention is directed towards the `review` column, and provides some plausible explanation regarding the absence of certain reviews. For missingness dependency analysis, we examine the relationship between the `rating` missingness and the number of cooking steps (`n_steps`) as well as the duration of cooking (`minutes`) via dependency test.<br>

Furthermore, we will probe into the research question surrounding consumer preferences and trends, especially in the context of the health-conscious choices reflected in low-calorie recipes. By evaluating whether recipes with lower caloric content are favored with higher ratings, we aim to shed light on whether health considerations weigh in on user preferences. This understanding is paramount for recipe creators and food-oriented enterprises as they navigate the currents of user and consumer needs and health trends, shaping their offerings to align with the tastes and well-being of their audience.<br>

---

## Data Cleaning
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

---

## Exploratory Data Analysis
### Univariate Analysis
In the univariate analysis, we would analyze the distribution of number of steps and the distribution of number of recipe submission.<br>

<iframe src="assets/Distribution_of_Number_of_Steps.html" width=800 height=600 frameBorder=0></iframe> <br>
This show that the distribution could be approximate as a right skewed gaussian distribution. By the shape of this distribution, most recipes have a lower number of steps, with a quick drop-off as the number of steps increases, indicating a concentration of simpler recipes with fewer steps. We would say this graph is centered approximately at 8. The peak of the histogram is in the lower range of the step count, emphasizing that the majority of the recipes are relatively easy to follow and likely cater to individuals seeking quick and straightforward meal preparation. The long tail to the right suggests that while there are recipes with many steps, they are less common. The graph suggests a user-friendly approach by food.com, focusing on less complex recipes that align with the fast-paced lifestyles of many users, while still offering options for those who enjoy the intricacy and challenge of more elaborate recipes.<br>

<iframe src="assets/Distribution_of_Number_of_Recipe_Submission.html" width=800 height=600 frameBorder=0></iframe> <br>
It is evident that there was a high level of activity in the initial years, with a particularly notable peak at the beginning. This could be indicative of an initial surge of enthusiasm when the platform was newly established or gaining popularity. Moving through the timeline, a gradual decline in submissions becomes apparent, interspersed with occasional spikes. These peaks may align with seasonal culinary events or holidays when users are more likely to share their creations. The dips, conversely, could signify quieter periods or a broader trend of diminishing submissions as the platform matured.<br>
<br>

### Bivariate Analysis
<iframe src="assets/bivariate_scatter_plot.html" width=800 height=600 frameBorder=0></iframe> <br>
This visualization reveals no obvious trend relating the number of steps to the time of submission, suggesting that the complexity of recipes—as measured by the number of steps—has remained diverse throughout the years. Recipes with a broad range of complexity are submitted in every month with no clear pattern of increase or decrease over time. The scatter plot is densest at the lower end of the x-axis, indicating that recipes with fewer steps are more commonly submitted. This density gradually thins out as the number of steps increases, implying that less complex recipes are more prevalent on the platform. <br>

<iframe src="assets/bivariate_line_plot.html" width=800 height=600 frameBorder=0></iframe> <br>
The line chart reveals a generally stable trend with several noticeable peaks. These outliers could be due to the submission of particularly complex recipes which influence the average at that time. Apart from the outliers, the average cooking time remains relatively consistent month-to-month around 70mins, which might indicate a steady preference for recipes that fit within a typical time commitment for daily cooking. This consistency implies that the food.com community values recipes that are practical and manageable in terms of preparation time.<br>
<br>

### Interesting Aggregates
In the aggregates analysis, we will study the sodium with the variety of ingredients. <br>
The pivot table for the sodium and ingredients: <br>

|    |   ('n_ingredients', '') |   ('mean', 'sodium') |   ('median', 'sodium') |   ('min', 'sodium') |   ('max', 'sodium') |
|---:|------------------------:|---------------------:|-----------------------:|--------------------:|--------------------:|
|  0 |                       1 |              26.1429 |                    0.5 |                   0 |                 236 |
|  1 |                       2 |              53.0428 |                    1   |                   0 |               10677 |
|  2 |                       3 |              23.3958 |                    3   |                   0 |                4715 |
|  3 |                       4 |              18.2441 |                    4   |                   0 |                3540 |
|  4 |                       5 |              25.4564 |                    7   |                   0 |               29338 |

<iframe src="assets/agg_plot_all.html" width=800 height=600 frameBorder=0></iframe> <br>
Given the wide range of sodium content across different ingredients, we focus on the median to assess the relationship between sodium content and the diversity of ingredients. The median serves as a robust measure to capture the central tendency of sodium levels in recipes, regardless of the individual sodium variability of each ingredient. By examining the median, we can gain insights into the overall sodium profile as it relates to the variety of ingredients included in the recipes.<br>


<iframe src="assets/agg_plot_median.html" width=800 height=600 frameBorder=0></iframe> <br>
This line chart suggests that as the number of ingredients in a recipe increases, there is a general trend of rising sodium content. However, the relationship is not strictly linear, as indicated by the fluctuations in the line. These variations could point to the diversity of recipe types—those with more ingredients do not necessarily have to be higher in sodium, depending on the nature of the ingredients used.<br>


---

## Assessment of Missingness
### NMAR Analysis
We believe the missingness of the review is Not Missing At Random (NMAR). This missingness might due to several reasons. Firstly, reviews are often driven by extremely positive or negative experiences. If people have a neutral or indifferent experience, they may not feel strongly compelled to share their thoughts on this recipe. Secondly, Moreover, in today's fast-paced world, people may feel that they don't have the time to write a thoughtful review. Busy schedules and other priorities can lead individuals to skip the review process. Also, some people might not feel confident in their ability to provide a well-informed review, especially if they are not experts in cooking. <br>
By adding a personal cooking level evaluation for every person who use this recipe, we could know their confidence level of cooking, and thus explain the missingness of reviews (thereby making it MAR).<br>
<br>

### Missingness Dependency
Discuss and testing the missingness of the rating whether depends on n_steps, the number of cooking steps, and minutes, the cooking duration. <br>

1. n_steps and rating <br>

    H0: The distribution of 'n_steps' when 'rating' is missing is the same as the distribution of 'n_steps' when 'rating' is not missing.<br>
    H1: The distribution of 'n_steps' when 'rating' is missing is different from the distribution of 'n_steps' when 'rating' is not missing.<br>
    Test statistic: Absolute difference in 'n_steps' means between two groups. <br>
    Significant level: 0.05


<iframe src="assets/missingness_n_steps.html" width=800 height=600 frameBorder=0></iframe> <br>
We use permutation test to shuffle the missingness of rating 1000 times and get 1000 simulating results about the absolute difference. <br>

<iframe src="assets/empirical_n_steps.html" width=800 height=600 frameBorder=0></iframe> <br>
We reject the null hypothesis that the 'rating' is independent on the 'n_steps', because the p-val is 0.0 < 0.05. Based on our test result, we may conclude that the missingness of the rating is MAR the rating is dependent on the number of cooking steps. <br>


2. minutes and rating <br>

    H0: The distribution of 'minutes' when 'rating' is missing is the same as the distribution of 'minutes' when 'rating' is not missing.<br>
    H1: The distribution of 'minutes' when 'rating' is missing is different from the distribution of 'minutes' when 'rating' is not missing.<br>
    Test statistic: Absolute difference in 'minutes' means between two groups. <br>
    Significant level: 0.05

<iframe src="assets/missingness_minutes.html" width=800 height=600 frameBorder=0></iframe> <br>
We use permutation test to shuffle the missingness of rating 1000 times and get 1000 simulating results about the absolute difference. <br>

<iframe src="assets/empirical_minutes.html" width=800 height=600 frameBorder=0></iframe> <br>
We fail to reject the null hypothesis that the 'rating' is independent on the 'minutes', because the p-val is  0.13 > 0.05. Based on our test result, we may conclude that the missingness of the rating is MCAR, that the rating is not dependent on cooking duration.  <br>


---

## Hypothesis Testing
### Permutation Test
The research question we intend to examine is whether recipes with different caloric contents are rated differently by users.<br>

For this analysis, we will categorize recipes based on their caloric content. We define a recipe as "high-calorie" if it contains calories greater than the median calorie count of all recipes. We will utilize a permutation test to assess the differences in average ratings between high-calorie and low-calorie recipes.<br>

**Null Hypothesis (H0)**: people are rating all recipes, irrespective of their caloric content, on the same scale.<br>
**Alternative Hypothesis (H1)**: high-calorie recipes receive lower ratings. This is based on the assumption that users may perceive high-calorie recipes as less healthy and therefore may rate them lower as part of a trend towards health consciousness.<br>
**Test statistic**: Difference in average ratings.<br>
**Significant level**: 0.05 <br>

We opt for a one-sided test because it is consistent with our assumption that higher caloric content has a one-directional impact on recipe ratings, potentially leading to lower ratings for high-calorie recipes compared to their low-calorie counterparts.<br>


<iframe src="assets/hypothesis_test.html" width=800 height=600 frameBorder=0></iframe> <br>
The plot below shows the empirical distribution of our test statistics in 10000 permutations, the red line indicates the observed test statistics.<br>


### Conclusion
Since the p-value of 0.0305 is less than the significance level of 0.05, we have sufficient evidence to **reject** the null hypothesis. This finding suggests that there is a **statistically significant** difference in how users rate recipes based on their caloric content. The lower p-value points towards the alternative hypothesis that recipes with higher calories may receive different ratings compared to those with lower calories.<br>

This result seems plausible as caloric content often reflects broader dietary preferences and health considerations which are increasingly important to many users. While the flavor of a dish may not be directly tied to calorie count, perceptions of healthiness and dietary impact might influence user ratings. Furthermore, this outcome could indicate a user preference trend where lower-calorie recipes are favored for their perceived health benefits.<br>
<br>

