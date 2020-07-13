---
layout: post
title:  "I Never Retrain My Models, and I Never Get Tired of Winning"
date:   2020-07-13 8:00:00 -0800
tags: tech
---
The emphasis of academic papers on improving performance on standardized tasks and Kaggle competitions likewise for big prizes might give you the idea that most important task when applying machine learning is to achieve the highest possible accuracy, precision, recall, or what have you.

Unfortunately, there aren't too many problems where business metrics move in lock-step with improvements to a single model. Marginal increases in model performance usually have less than marginal return. To be clear, there are times when model retraining is necessary for success.

However, what I would like to discuss today is how increasing the number of ways you can intervene in your system is more likely to give you big wins, and how this is generally much more effective than tweaking model performance.

## Metric Trajectories
Let's imagine a very simple machine-learning based dating app, where we are predicting the probability **y** that a user will click on a profile that we show and, in order to maintain a positive experience, we apply a threshold **T** to these profiles:

**y** > **T**.

Then imagine there are two business metrics we care about, M1 and M2, and assume it's better for the business if they go up (such as revenue, site visits, weekly active users, etc.). As we vary T, we should carve out a trajectory in the space defined by M1 and M2.

If it looks like the plot below, we would be very happy because we can simultaneously improve both metrics.

<img src="/assets/images/pareto/metrics.001.jpg" alt="good" width="400" height="400"/>

Alternatively, if it looks like the next one, that's not as good because we need to degrade one metric to get an improvement in the other.

<img src="/assets/images/pareto/metrics.002.jpg" alt="bad" width="400" height="400"/>

Regardless of the exact shape, we are confined to move along the trajectory. Furthermore, our trajectory likely has definite endpoints; even if our threshold doesn't have a finite range, there may be asymptotic behavior with respect to our business metrics.

## What Does Retraining Do?
When we retrain our model, we might be able to shift our trajectory's shape or endpoints, but we can't fundamentally change the fact that we're confined to that line.

<img src="/assets/images/pareto/metrics.003.jpg" alt="retrain" width="400" height="400"/>

If we have a pretty good model already, movement in our trajectory might not be enough to provide significant change in our business metrics.

One time that it obviously makes sense to retrain our models with updated training data is when it seems likely that preferences have shifted. For example, a site that ranks news stories might need to retrain very often, as user affinity for different topics may shift depending what is happening in the world. On the other hand, our dating site may not need to retrain very often because dating interests may not be varying rapidly.

However, when retraining is essential, it means we're fighting to maintain the status quo, and the updated model is only going to return from a degraded position to our original one.

## Adding Interventions
An intervention our system can be anything from a hyperparameter in our decision function to a new product feature. It's anything that allows us to modify the user experience algorithmically.

Let's extend our simple example to a slightly more complex decision logic, by imagining that in addition to predicting the probability a user, U1, will click on the profile corresponding to a second user, U2, we also predict the probability, **z** that U2 will click on U1's profile. Again, we apply a threshold **V** in order to improve the quality of the users' experiences:
**y** > **T** && **z** > **V**.

Now, we have two hyperparameters to vary. Each one individually traces out a different trajectory when holding the other fixed. Ideally, they should be complementary. Looking at the plot below, if we started with the blue trajectory when varying **T** alone, the green curve would be a nice complement to obtain by introducing **z** and **U**.

<img src="/assets/images/pareto/metrics.005.jpg" alt="retrain" width="400" height="400"/>

The real strength is that instead of being locked onto a single line, we can now tune our system in a two-dimensional region. As long as we have done a reasonable job in designing our intervention, we are practically guaranteed to have opened up a region in metric space that was previously inaccessible.

<img src="/assets/images/pareto/metrics.006.jpg" alt="retrain" width="400" height="400"/>

The more (good) interventions we add, the easier it is to find ways into better metric wins. The better our system gets, the harder it is to improve, so we continually need to add ways to move around in the metric space. If we have a larger number of metrics, having additional handles also lets us "fix" trade-offs we sometimes have to make.

For example, maybe moving along the red curve is the best way to get improvements in M2. So after we take some improvement in M2 by tuning that intervention, we can recover the loss in M1 using the green curve. And it might look like blue is better for M2, but maybe it's much worse in a hidden metric, M3!

<img src="/assets/images/pareto/metrics.004.jpg" alt="retrain" width="400" height="400"/>

## Final Thoughts
As I said at the top, it's hard to find a business problem where moving your machine learning model metric by 1% or 0.1% is going to have an outsized impact. Usually, the business impact will be a fraction of what that improvement is. So, you have to ask yourself what the value of investing in those improvements is.

Completely new interventions in your product give you access to much larger portions of phase space in your key business metrics. Those interventions can be totally new product features or new hyper parameters in your algorithmic decision making. While adding new capabilities takes effort and introduces complexity, it's also a pathway for delivering big wins.