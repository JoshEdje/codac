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

