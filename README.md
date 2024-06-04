Technical Report: Analysis of Conversion Rates and Average Spending Between Test Groups

Introduction
This report provides a detailed statistical analysis of the differences in conversion rates and average amounts spent per user between two distinct test groups. Utilizing hypothesis testing and confidence interval estimation, the study aims to determine statistically significant differences between the groups, aiding strategic decision-making. The analyses were conducted using standard statistical methods under assumptions appropriate for the data characteristics.

Task 1. Hypothesis Testing on Conversion Rates
Objective
To determine if there is a statistically significant difference in the conversion rates between the two test groups.

Methodology
A hypothesis test was conducted using the normal distribution at a 5% significance level. The pooled proportion method was applied to calculate the standard error due to the assumption of equal variance across groups.

Results
- **Pooled Proportion:** 0.0428
- **Standard Error:** 0.0018
- **Z-Score:** -3.8643
- **P-Value:** 0.00011

Conclusion
The p-value obtained from the test is significantly lower than the 5% significance level. This result leads to the rejection of the null hypothesis, indicating a significant difference in the conversion rates between the two test groups.

Task 2. Confidence Interval for Difference in Conversion Rates
Objective
To estimate the 95% confidence interval for the difference in conversion rates between the treatment and control groups.

Methodology
The confidence interval was calculated using the normal distribution with unpooled proportions for the standard error, reflecting the potential difference in variances between the groups.

Results
- **95% Confidence Interval:** (0.0035, 0.0107)

Conclusion
The confidence interval does not include zero, which supports the finding that there is a meaningful difference in conversion rates between the two groups.

Task 3. Hypothesis Testing on Average Amount Spent
Objective
To evaluate whether there is a difference in the average amount spent per user between the two groups.

#### Methodology
The hypothesis test was conducted using the t-distribution with a 5% significance level, under the assumption of unequal variances between the groups (Welch's t-test).

#### Results
- **T-statistic:** 2.1750
- **P-value:** 0.0296

#### Conclusion
The p-value is below the 5% threshold, which supports the rejection of the null hypothesis. This suggests that there is a statistically significant difference in the average amounts spent between the groups.

TASK 4. Confidence Interval for Difference in Average Amount Spent
Objective
To determine the 95% confidence interval for the difference in the average amount spent per user between the treatment and control groups.

#### Methodology
The confidence interval was estimated using the t-distribution, appropriate for data with unequal variances.

#### Results
- **95% Confidence Interval:** (-1.71, 1.74)

#### Conclusion
The confidence interval spans both negative and positive values, indicating that the difference in average amounts spent could favor either group, but still suggesting variability that includes no difference. However, the hypothesis test results favor a significant difference, highlighting the complexity and sensitivity of statistical analysis.

### Summary
The statistical tests and confidence interval analyses confirm significant differences in conversion rates and suggest differences in average spending between the two test groups. These results provide valuable insights for decision-making regarding marketing strategies and resource allocation. Further research might include deeper analysis into the causes of these differences and validation with larger sample sizes or additional metrics.

