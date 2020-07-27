---
layout: post
title:  "Optimizing COVID-19 Pooled Testing"
date:   2020-07-27 8:00:00 -0800
tags: news
---
We had our first experience with COVID testing in early July when my wife needed to get tested before starting a new job. As an asymptomatic, non-exposed person, she was able to get a test through an appointment with One Medical, but it took 7 days to get the antibody test results and 10 days to get the virus test results. Obviously, a 10-day wait is not ideal, and even [longer wait times](https://www.buzzfeednews.com/article/davidmack/coronavirus-testing-delays-backlog) are [becoming common](https://khn.org/news/states-search-for-ways-to-deal-with-covid-19-testing-backlogs/).

In an attempt to relieve test backlogs, the [FDA announced in June](https://www.fda.gov/news-events/press-announcements/coronavirus-covid-19-update-facilitating-diagnostic-test-availability-asymptomatic-testing-and) that they would help test developers validate protocols for testing pooled samples, and [announced last week](https://www.fda.gov/news-events/press-announcements/coronavirus-covid-19-update-fda-issues-first-emergency-authorization-sample-pooling-diagnostic) that the Quest Diagnostics COVID-19 test has been  the first to be approved for use on pooled samples.

Unfortunately, [some reporting](https://www.marketwatch.com/story/fda-approves-faster-quest-covid-19-test-for-pooled-sample-use-2020-07-19) is saying that pooling is not effective when the test positivity rate is above 10%, a threshold being surpassed by [a large number](https://coronavirus.jhu.edu/testing/individual-states) of states. Arizona seems to be on track to even pass 25%.

Thankfully, that belief seems to be incorrect. As I will explain below, pooling remains effective even at a 25% test positivity rate, and there are strategies available to improve the effectiveness of pooling.

## How Does Pooling Work?
The idea behind test pooling is pretty simple. Instead of testing each sample individually, multiple samples are mixed together and tested in aggregate. For example, a lab might combine 10 samples together and then perform a single test on the pooled sample. If the pool tests negative, all samples are negative. If the pool tests positive, then all individual samples are tested. This strategy is effective when the test is highly sensitive, which the COVID-19 PCR tests are.

We can see there's clearly an opportunity for optimization here. What is the correct number of samples to pool to minimize the total number of tests needed to identify all positive samples? If the pool size is too small, then a lot of small pools could have been aggregated into larger pools and still tested negative. If it's too large, then too many pools will test positive and all the individual samples will need to be tested again.

Assuming samples are assigned to pools randomly, it's straightforward to calculate the probability that a pool will test negative, given we know the test positivity rate **p** and number of samples per pool **k**:

<div align="center">
<img src="/assets/images/covidTest/prob.gif" alt="good" width="278" height="21"/>
</div>

It's then easy to calculate the total number of tests needed when k > 1:
<div align="center">
<img src="/assets/images/covidTest/total0.gif" alt="good" width="383" height="38"/>
</div>

This simplifies to:
<div align="center">
<img src="/assets/images/covidTest/total1.gif" alt="good" width="293" height="38"/>
</div>

## Optimizing the Pool Size 
Unfortunately, the derivative of the above equation is transcendental, so the solutions don't have a nice closed form. However, we can simply plot the function for a number of positivity rates and see where it looks like the minima are. Below, I plot the function for test positivity rates of 0.01 (blue), 0.1 (red), and 0.25 (green). States like Connecticut have their rates even below 0.01. There are quite a few states near 0.1, and, as I mentioned, Arizona is approaching 0.25. For all three cases, I used 10,000 as the total number of samples.

In addition, I ran a quick simulation for 10 runs at each positivity rate at pool sizes of 1, 2, 3, 4, 5, 10, 25, and 100. You can see the mean and variance for the simulation at the points on the plot.

<div align="center">
<img src="/assets/images/covidTest/batchSize.png" alt="good" width="500" height="500"/>
</div>

From this plot, you can see that the best pool size when the positivity rate is 0.01 is 10 and results in 80% savings in terms of test materials. For both 0.1 and 0.25, the best pool size is 3, and result in 40% and 10% savings, respectively. So even at 25% test positivity, there is an opportunity for improving efficiency.

## Risk Tranches for Increased Efficiency
Health officials are already implementing prioritization levels to ensure tests for patients in hospitals, elder-care facilities, and other high risk environments are getting returned quickly. The FDA specifically suggested that sample pooling only be done on asymptomatic samples. Here's how that might work.

An option to increase efficiency further is to separate tests based on risk and adopt different pooling strategies for each tranche. Imagine a simple scenario where we separate tests based on whether the person is symptomatic versus asymptomatic. Let's assume that divides the population in half and that the symptomatic population has a test positivity rate of 49% and the asymptomatic population has a test positivity rate of 1%.

The total test positivity rate in this example is 25%, so for 10,000 samples and a single pooling strategy, the best we could do is around 9,000 tests needed. However, now in the asymptomatic half, we can adjust to the 1% rate, meaning we need only 1,000 tests to cover those 5,000 samples. We'll still need 5,000 to test all of the symptomatic samples individually, but then we only need 6,000 tests compared to 9,000.

Probably additional criteria can be used to break down samples into different groups further, such as whether the person is known to have been exposed to COVID-19 or not.

## Final Thoughts
Obviously, this type of analysis doesn't include the effect of any operational difficulties involved in changing the size of pools, be it machine constraints, material wastage, or time requirements.

But hopefully, the adoption of pooled testing by Quest and, in the future, other labs will have a significant impact in reducing wait times for people to receive their results. Improving the speed at which asymptomatic people can determine if they're carrying the virus is critical to containing the spread through second-degree interactions.

As for the reduction in wait time, that will be determined by how much this change increases the lab processing through-put and how big the backlog is now. If Quest is now able to process more tests per day than are being sent in, they'll start reducing the wait time and eventually be able to return all results within about 1 day, which we know is possible for priority cases. On the other hand, tests are still being throttled in many places to keep the workload manageable, so as throughput increases, the total test count will also increase.