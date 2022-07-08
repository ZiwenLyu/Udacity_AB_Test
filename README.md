# Udacity_AB_Test
## Experiment Overview
During this experiment, Udacity courses featured two options on the course overview page: "start free trial" and "access course materials." The student will be enrolled in a free trial for the paid version of the course after clicking "start free trial," and then they will be prompted to provide their credit card information. They will be automatically charged after 14 days unless they cancel first. The student still can watch the videos and do the quizzes for free if they choose to access the course materials, but they won't get coaching assistance, a validated certificate, or the opportunity to submit their final project for review.

![standard_errors](https://github.com/ZiwenLyu/Udacity_AB_Test/blob/main/screenshots/Final%20Project_%20Experiment%20Screenshot.png)

In the experiment, Udacity tried out a new feature where, after clicking "start free trial," the student was prompted to indicate how much time they could commit to the course. The student would go through the checkout procedure as usual if they indicated that they worked five or more hours per week. A notice advising that Udacity courses often require a greater time commitment for effective completion would show up if they stated less than 5 hours per week. It would also offer that the student might like to access the course materials for free. The student would then have the choice to either access the course contents for free or to sign up for the free trial again.

The hypothesis is that setting a time expectation upfront will diminish the number of frustrated students who leave the free trial without enough time. So Udacity will provide better user experience and improve the coach’s capacity to instruct students.

## Experiment Design
### Funnel Illustration
![funnel](https://github.com/ZiwenLyu/Udacity_AB_Test/blob/main/screenshots/funnel.png)

### Unit of Diversion
The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

### Metric Choice
#### Invariant Metrics: 
Number of cookies, number of clicks, and click-through-probability will be three invariant metrics. We don’t expect a change of them in the control and the experiment groups.

- **Number of cookies**: That is, number of unique cookies to view the course overview page. (dmin=3000)
- **Number of clicks**: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is triggered). (dmin=240)
- **Click-through-probability**: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)

#### Evaluation Metrics:

- **Gross conversion**: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)

- **Retention**: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)

- **Net conversion**: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)


Gross conversion, retention, and net conversion are the evaluation metrics, and we expect to see their significant differences between the control and the experiment groups. The gross conversion of the experiment group is expected to be lower than the control group, because if the screener works, students who cannot commit 5 hours or more will be diverted to the access to course materials. While we expect the retention and net conversion to be higher than the control group, as we already filter out the part of students who are likely to leave.

### Hypotheses

The null hypothesis is that the screener has no effect and doesn’t increase the conversion of free trial to payment.
The alternative hypothesis is that the screener has an effect and improves the conversion of free trial to payment.
- H0: Gross Conversion(exp) = Gross Conversion(cont)
- H1:Gross Conversion(exp) ≠ Gross Conversion(cont)
- H0: Retention(exp) = Retention(cont)
- H1:Retention(exp) ≠ Retention(cont)
- H0: Net Conversion(exp) = Net Conversion(cont)
- H1:Net Conversion(exp) ≠ Net Conversion(cont)

### Analytical Estimate of Standard Deviation of Evaluation Metrics
Since the evaluation metrics all are probabilities, we can assume that they are binomial distributions. Besides, the units of analysis are the same as the units of diversion, which means their analytical estimates are comparable to their empirical variability. Their standard errors are as follow:

| Metrics | Standard Errors |
| -------- | --------- |
| gross_convetion | 0.0202 |
| retention | 0.0549 |
| net_convetion | 0.0156 |

### Sizing
#### Number of Samples vs. Power
There are two methods to size the sample and track multiple metrics while not increase false positive probability:

Assume the independence of each metric:

- αoverall = 1 - (1- αindividual)^n

Bonferroni Correction: 
- αindividual = αoverall / n

However, since three evaluation metrics are correlated and move together, it will be too conservative to use Bonferroni correction. So we will compute the appropriate number of samples by using standard errors to ensure the size and the power of metrics. (The online calculator)

**Gross Conversion**:
- Baseline Conversion: 20.625%
- Minimum Detectable Effect: 1%
- Alpha: 5%
- Beta: 20% -Sensitivity (1 - Beta): 80%
- Sample Size: 25,835*2 = 51,670 clicks
- Pageviews needed:  sample clicks/(clicks/pageviews) = 645,875 

**Retention**:
- Baseline Conversion: 53%
- Minimum Detectable Effect: 1%
- Alpha: 5%
- Beta: 20%
- Sensitivity (1 - Beta): 80%
- Sample size: 39,115*2 = 78,230 enrollments
- Pageviews needed: sample enrollments/(enrollments/pageviews) = 4,741,212

**Net Conversion**:
- Baseline Conversion: 10.9313%
- Minimum Detectable Effect: 0.75%
- Alpha: 5% -Beta: 20%
- Sensitivity (1 - Beta): 80%
- Sample size: 27,413*2 = 54,826 clicks
- Pageviews needed: sample clicks/(clicks/pageviews) = 685,325

Given our calculations, gross convention requires 645,875 pageviews, retention needs 4,741,212 pageviews, and net conversion needs 685,325 pageviews. In order to effectively test three hypotheses, the number of samples is 4,741,212.

### Duration vs. Exposure

If we divert 100% of the traffic to our experiment, given 40,000 pageviews per day,  it will take pageviews-per-day/pageviews-required = 119 days to fulfill our sample size.
 
Clearly, that's too risky for Udacity. If the experiment is harmful to user experience, the long-period experiment will incur great frustration among students and cause huge business loss.Besides, this also presents an opportunity risk as it's impossible to run other experiments during the 4 months.
Since the net conversion metric can also measure the effectiveness of the free trial screener function after the 14-day boundary, we choose to discard the retention metric and reduce our sample size. Therefore, we only need 685,325 pageviews in total. And given the relatively low risk of this new feature, ideally we plan to divert 75% of the traffic per day and the duration is 23 days.

## Experiment Analysis
### Sanity Checks
To ensure the test run properly, we will conduct the sanity check on three invariant metrics (the number of cookies, the number of clicks, and the click-through probability). For invariant metrics we expect equal diversion into the experiment and control group. The sanity check results are followed:

|  Metrics | The number of pageviews  | The number of clicks | The click-through probability |
|------------- | ------------- | ------------- |------------- |
| Obeserved Value | 0.5006 | 0.5004 | 0.0001 |
| CI lower bound | 0.4988 | 0.4959 | -0.0013 |
| CI upper bound | 0.5012 | 0.5041 | 0.0013 |
| Result | Pass | Pass | Pass |


### Result Analysis
Accroding to the data from Udacity, although in total we have 690,203 pageviews, we missed the last 14 days' enrollment and payment. Because the payment of students who tried for free within last 14 days was out of the tracking duration, so we will only have 23 days' data and 423,525 pageviews in total.

#### Effect Size Tests
We computed the confidence interval around the observed differences and compare with the practical differences (dmin) to see if the experiment generates significant results statistically and practically.

|  Metrics | Gross Conversion  | Net Conversion | 
|------------- | ------------- | ------------- |
| Obeserved differences | -0.0206 | -0.0048 |
| Minimal practical differences | 0.01 | 0.0075 |
| CI lower bound | -0.0291 | -0.0116 | 
| CI upper bound | -0.0120 | 0.0019 | 
| Statistically Significant | True | False |
| Practically Significant | True | False |

#### Sign Tests
In order to double check the analytical result, we conducted a traditional sign test using the day-by-day data, measuring the p-value of the sign test whether the result is statistically significant.

|  Metrics | Gross Conversion  | Net Conversion | 
|------------- | ------------- | ------------- |
| P-value | 0.0025 | 0.6776 |
| α | 0.05 | 0.05 |
| Statistically Significant | True | False |

Gross conversion is both statistically and practically significant, while the net conversion is neither statistically and practically significant. The sign test result matches the result we drew from the effect size test.

## Summary
The experiment is conducted to test the new feature of free trial screener on Udacity. Students were recorded as unique cookies and divided into the control and experiment groups. The experiment group got a survey to enter their weekly study time after they clicked starting the free trial, and if they only could commit less than 5 hours per week, the Udacity would pop up a message to suggest them access to free course materials instead of choosing the free trial. Number of pageviews, number of free trial clicks (before the survey and commit time reminder), and the click-through probability are invariant metrics. Gross conversion (enrollments/clicks) and net conversion (payments/clicks) are served as evaluation metrics.

Recall the hypotheses:
- H0: Gross Conversion(exp) = Gross Conversion(cont)
- H1:Gross Conversion(exp) ≠ Gross Conversion(cont)
- H0: Net Conversion(exp) = Net Conversion(cont)
- H1:Net Conversion(exp) ≠ Net Conversion(cont)

As expected the observed gross conversion is 2.06% lower than the control group; Besides, the result indicates statistical and practical significance. The observed net conversion in the experiment group is 0.48% suprisingly lower than the control group, and the result is neither statsically nor practically sginificant to the business. Therefore: the H0:Gross Conversion(exp) = Gross Conversion(cont) is rejected, while the H0: Net Conversion(exp) = Net Conversion(cont) cannot be rejected.

### Recommendation
From the analysis we can see that, the free trial screener is able to effectively filter out parts of students who may not commit to the study, which explains the sliding gross conversion. Maybe the change clearly set expections upfront and reduce the number of frustrated students, however, this new feature is less likely to relatively increase payments after the 14-day trial. Though the data from Udacity which didn't meet the required test size may skew the result to some degree, considering the cost in updating the new feature and barely payments growth, I suggest do not launch this new function. 

### Follow-Up Experiment: How to Reduce Early Cancellations
Since Udacity would like to lessen early cancellations after the 14-day trial, we need to figure out other factors may cause this kind of behavior besides the insufficient time to commit to study. 

In order to understand why many students churn during the 14-day trial, I recommend to conduct follow-up qualitative experiments to dig into this question. Udacity may organize focus groups first to get a feel about what other factors contributing to the early cancellations. Then, Udacity may launch surveys to students who churn before the 14-day boundary and let them rank factors leading to unsubscriptions, as well as asking questions about their experience of their learning, the time pressure, the coach supports, the difficult level, etc. Some top answers could better explain the churn problem than the study time issue.

Then, combining with the result from the qualitative research, Udacity may modify their features and design another test. For instance, most students reported the 14-day trial is too long as they lost interests in learning or they finished courses within 14 days, so the new feature may shorten the free trial period to 7 days. 

To set up the new experiment, enrolled users will be equally diverted into the control and experiment groups. The unit of diversion will be user IDs. The evaluation metric will be retention, which is the number of students remain after 14 days (at least pay once) divided by the number of students who enrolled the free trial. 
So the new hypothesis is:

- H0: Retention(exp) = Retention(cont)
- H1: Retention(exp) ≠ Retention(cont)

In our expectation, reducing the free trial duration will lead to a higher payment conversion. If the observed retention is both statistically and practically significant and indicates a positive change, the new feature could be gradually popularized to users.

