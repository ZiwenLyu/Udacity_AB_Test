# Udacity_AB_Test
## Experiment Overview
During this experiment, Udacity courses featured two options on the course overview page: "start free trial" and "access course materials." The student will be enrolled in a free trial for the paid version of the course after clicking "start free trial," and then they will be prompted to provide their credit card information. They will be automatically charged after 14 days unless they cancel first. The student still can watch the videos and do the quizzes for free if they choose to access the course materials, but they won't get coaching assistance, a validated certificate, or the opportunity to submit their final project for review.

In the experiment, Udacity tried out a new feature where, after clicking "start free trial," the student was prompted to indicate how much time they could commit to the course. The student would go through the checkout procedure as usual if they indicated that they worked five or more hours per week. A notice advising that Udacity courses often require a greater time commitment for effective completion would show up if they stated less than 5 hours per week. It would also offer that the student might like to access the course materials for free. The student would then have the choice to either access the course contents for free or to sign up for the free trial again.

The hypothesis is that setting a time expectation upfront will diminish the number of frustrated students who leave the free trial without enough time. So Udacity will provide better user experience and improve the coach’s capacity to instruct students.

## Experiment Design
### Funnel Illustration

### Unit of Diversion
The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

### Metric Choice
#### Invariant Metrics: 
Number of cookies, number of clicks, and click-through-probability will be three invariant metrics. We don’t expect a change of them in the control and the experiment groups.

Number of cookies: That is, number of unique cookies to view the course overview page. (dmin=3000)
Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is triggered). (dmin=240)
Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)

#### Evaluation Metrics:

Gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
Retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)


Gross conversion, retention, and net conversion are the evaluation metrics, and we expect to see their significant differences between the control and the experiment groups. The gross conversion of the experiment group is expected to be lower than the control group, because if the screener works, students who cannot commit 5 hours or more will be diverted to the access to course materials. While we expect the retention and net conversion to be higher than the control group, as we already filter out the part of students who are likely to leave.

### Hypotheses

The null hypothesis is that the screener has no effect and doesn’t increase the conversion of free trial to payment.
The alternative hypothesis is that the screener has an effect and improves the conversion of free trial to payment.
H0: Gross Conversion(exp) = Gross Conversion(cont)
H1:Gross Conversion(exp) ≠ Gross Conversion(cont)
H0: Retention(exp) = Retention(cont)
H1:Retention(exp) ≠ Retention(cont)
H0: Net Conversion(exp) = Net Conversion(cont)
H1:Net Conversion(exp) ≠ Net Conversion(cont)

### Analytical Estimate of Standard Deviation of Evaluation Metrics
Since the evaluation metrics all are probabilities, we can assume that they are binomial distributions. Besides, the units of analysis are the same as the units of diversion, which means their analytical estimates are comparable to their empirical variability. Their standard errors are as follow:




### Sizing
Number of Samples vs. Power
There are two methods to size the sample and track multiple metrics while not increase false positive probability:
Assume the independence of each metric
αoverall = 1 - (1- αindividual)^n
Bonferroni Correction
αindividual = αoverall / n
However, since three evaluation metrics are correlated and move together, it will be too conservative to use Bonferroni correction. So we will compute the appropriate number of samples by using standard errors to ensure the size and the power of metrics. (The online calculator)
Gross Conversion:
Baseline Conversion: 20.625%
Minimum Detectable Effect: 1%
Alpha: 5%
Beta: 20% -Sensitivity (1 - Beta): 80%
Sample Size: 25,835*2 = 51,670 clicks
Pageviews needed:  sample clicks/(clicks/pageviews) = 645,875 
Retention:
Baseline Conversion: 53%
Minimum Detectable Effect: 1%
Alpha: 5%
Beta: 20%
Sensitivity (1 - Beta): 80%
Sample size: 39,115*2 = 78,230 enrollments
Pageviews needed: sample enrollments/(enrollments/pageviews) = 4,741,212
Net Conversion:
Baseline Conversion: 10.9313%
Minimum Detectable Effect: 0.75%
Alpha: 5% -Beta: 20%
Sensitivity (1 - Beta): 80%
Sample size: 27,413*2 = 54,826 clicks
Pageviews needed: sample clicks/(clicks/pageviews) = 685,325
Given our calculations, gross convention requires 645,875 pageviews, retention needs 4,741,212 pageviews, and net conversion needs 685,325 pageviews. In order to effectively test three hypotheses, the number of samples is 4,741,212.

### Duration vs. Exposure

If we divert 100% of the traffic to our experiment, given 40,000 pageviews per day,  it will take pageviews-per-day/pageviews-required = 119 days to fulfill our sample size.
 
Clearly, that's too risky for Udacity. If the experiment is harmful to user experience, the long-period experiment will incur great frustration among students and cause huge business loss.Besides, this also presents an opportunity risk as it's impossible to run other experiments during the 4 months.
Since the net conversion metric can also measure the effectiveness of the free trial screener function after the 14-day boundary, we choose to discard the retention metric and reduce our sample size. Therefore, we only need 685,325 pageviews in total.

