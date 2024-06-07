# Technical Report: Analysis of Conversion Rates and Average Spending Between Two A/B Test Groups

## Project Overview 
An A/B test is an experimentation technique used by businesses to compare two versions of a webpage, advertisement, or product feature to determine which one performs better. By randomly assigning customers or users to either the A or B version, the business can determine which version is more effective at achieving a particular goal.
This report provides a detailed statistical analysis of the differences in conversion rates and average amounts spent per user between two distinct test groups. Utilizing hypothesis testing and confidence interval estimation, the study aims to determine statistically significant differences between the groups, aiding strategic decision-making. The analyses were conducted using standard statistical methods under assumptions appropriate for the data characteristics.

#### A/B Test Setup
An A/B test is an experimentation technique used by businesses to compare two versions of a webpage, advertisement, or product feature to determine which one performs better. By randomly assigning customers or users to either the A or B version, the business can determine which version is more effective at achieving a particular goal.
The Growth team decides to run an A/B test that highlights key products in the food and drink category as a banner at the top of the website. The control group does not see the banner, and the test group sees it as shown below:

![image](https://github.com/JoshEdje/codac/assets/171504805/5429dae1-370a-4abc-b152-63a03a66a25a)

The setup of the A/B test is as follows:

The experiment is only being run on the mobile website.
A user visits the GloBox main page and is randomly assigned to either the control or test group. This is the join date for the user.
The page loads the banner if the user is assigned to the test group, and does not load the banner if the user is assigned to the control group.
The user subsequently may or may not purchase products from the website. It could be on the same day they join the experiment, or days later. If they do make one or more purchases, this is considered a “conversion”.

![image](https://github.com/JoshEdje/codac/assets/171504805/a9671d6c-eb7c-4486-9360-bd0644fc7563)

#### DATASET
GloBox stores its data in a relational database, which you can access through Beekeeper. 
postgres://Test:bQNxVzJL4g6u@ep-noisy-flower-846766-pooler.us-east-2.aws.neon.tech/Globox

#### Data cleaning/preparation 
in the initial data preparation phase, the following tasks were performed 
- Data loading & inspection
- Handling missing values
- Data cleaning and formatting

#### Exploratory Data analysis 
This involved exploring the sales data to answer key questions shown below 
- What are the start and end dates of the experiment?
- How many total users were in the experiment?
- How many users were in the control and treatment groups?
- What was the conversion rate of all users?
- What is the user conversion rate for the control and treatment groups?
- What is the average amount spent per user for the control and treatment groups, including users who did not convert?

#### Data analysis 
The questions above were solved using the following SQL queries.  
``` sql 
SELECT 
    MIN(join_dt) AS start_date, 
    MAX(join_dt) AS end_date
FROM 
    Groups;
```
![Screenshot 2024-06-07 192308](https://github.com/JoshEdje/codac/assets/171504805/9929efa5-62ce-465c-8a44-02e4d5a08da0)



```sql
SELECT 
    COUNT(DISTINCT uid) AS total_users
FROM 
    groups;
```

![Screenshot 2024-06-07 192337](https://github.com/JoshEdje/codac/assets/171504805/f91c783e-df16-41bb-b67f-31b3520373dd)


```sql
SELECT 
    "group", 
    COUNT(DISTINCT uid) AS user_count
FROM 
    groups
GROUP BY 
    "group";
```

![image](https://github.com/JoshEdje/codac/assets/171504805/39def958-b7d8-4427-b844-ce12d58855ee)


```sql
WITH Purchasing_Users AS (
    SELECT 
        COUNT(DISTINCT uid) AS purchasers
    FROM 
        activity
),
Total_Users AS (
    SELECT 
        COUNT(DISTINCT uid) AS total
    FROM 
        groups
)
SELECT 
    (Purchasing_Users.purchasers::DECIMAL / Total_Users.total) * 100 AS conversion_rate_percentage
FROM 
    Purchasing_Users, Total_Users;
```

![image](https://github.com/JoshEdje/codac/assets/171504805/0bb62767-bb56-491e-9872-12de05210ec5)


```sql
WITH Group_Conversions AS (
    SELECT 
        g."group",
        COUNT(DISTINCT a.uid) AS purchasers
    FROM 
        groups g
    LEFT JOIN 
        activity a ON g.uid = a.uid
    GROUP BY 
        g."group"
),
Total_Group_Users AS (
    SELECT 
        "group",
        COUNT(DISTINCT uid) AS total_users
    FROM 
        groups
    GROUP BY 
        "group"
)
SELECT 
    t."group",
    ROUND((c.purchasers::DECIMAL / t.total_users) * 100, 2) AS conversion_rate_percentage
FROM 
    Total_Group_Users t
JOIN 
    Group_Conversions c ON t."group" = c."group";
```

![image](https://github.com/JoshEdje/codac/assets/171504805/a3efe3ba-ca28-400e-8e23-bf725ab77cd7)


```sql
WITH Group_Spending AS (
    SELECT 
        g."group",
        g.uid,
        COALESCE(SUM(a.spent), 0) AS total_spent
    FROM 
        groups g
    LEFT JOIN 
        activity a ON g.uid = a.uid
    GROUP BY 
        g."group", g.uid
)
SELECT 
    "group",
    ROUND(AVG(total_spent), 2) AS avg_spent_per_user
FROM 
    Group_Spending
GROUP BY 
    "group";
```
![image](https://github.com/JoshEdje/codac/assets/171504805/3335f5f3-d19c-4d0c-82c9-b47cdb38d37c)






## Calculating The A/B Test Statistics
The results provided enabled the construction of both a hypothesis test and confidence interval comparing the two test groups for each metric of interest. Represented by the following tasks. 






### Task 1. Hypothesis Testing on Conversion Rates
#### Objective
To determine if there is a statistically significant difference in the conversion rates between the two test groups.

#### Methodology
A hypothesis test was conducted using the normal distribution at a 5% significance level. The pooled proportion method was applied to calculate the standard error due to the assumption of equal variance across groups.

#### Results
- **Pooled Proportion:** 0.0428
- **Standard Error:** 0.0018
- **Z-Score:** -3.8643
- **P-Value:** 0.00011

#### Conclusion
The p-value obtained from the test is significantly lower than the 5% significance level. This result leads to the rejection of the null hypothesis, indicating a significant difference in the conversion rates between the two test groups.

### Task 2. Confidence Interval for Difference in Conversion Rates
#### Objective
To estimate the 95% confidence interval for the difference in conversion rates between the treatment and control groups.

#### Methodology
The confidence interval was calculated using the normal distribution with unpooled proportions for the standard error, reflecting the potential difference in variances between the groups.

#### Results
- **95% Confidence Interval:** (0.0035, 0.0107)

#### Conclusion
The confidence interval does not include zero, which supports the finding that there is a meaningful difference in conversion rates between the two groups.

### Task 3. Hypothesis Testing on Average Amount Spent
#### Objective
To evaluate whether there is a difference in the average amount spent per user between the two groups.

#### Methodology
The hypothesis test was conducted using the t-distribution with a 5% significance level, under the assumption of unequal variances between the groups (Welch's t-test).

#### Results
- **T-statistic:** 2.1750
- **P-value:** 0.0296

#### Conclusion
The p-value is below the 5% threshold, which supports the rejection of the null hypothesis. This suggests that there is a statistically significant difference in the average amounts spent between the groups.

### Task 4. Confidence Interval for Difference in Average Amount Spent
#### Objective
To determine the 95% confidence interval for the difference in the average amount spent per user between the treatment and control groups.

#### Methodology
The confidence interval was estimated using the t-distribution, appropriate for data with unequal variances.

#### Results
- **95% Confidence Interval:** (-1.71, 1.74)

#### Conclusion
The confidence interval spans both negative and positive values, indicating that the difference in average amounts spent could favor either group, but still suggesting variability that includes no difference. However, the hypothesis test results favor a significant difference, highlighting the complexity and sensitivity of statistical analysis.

### Summary
The statistical tests and confidence interval analyses confirm significant differences in conversion rates and suggest differences in average spending between the two test groups. These results provide valuable insights for decision-making regarding marketing strategies and resource allocation. Further research might include deeper analysis into the causes of these differences and validation with larger sample sizes or additional metrics.

