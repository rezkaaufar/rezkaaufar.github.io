---
layout: post
title: AB test notes
description: AB testing is a common method used in tech companies to evaluate the effectiveness of different strategies. In this post I try to go over the concept and visualize it to make it easier to remember.
date: '2026-02-07'
tags: []
---

I have known AB Testing and used it for a while in my work but sometimes I find myself remembering the equation too much and unsure about how things worked under the hood. In this post I try to go over the concept and visualize it to make it easier to remember. 

# MDE, power, alpha, and sample size

We start off with two distributions of a conversion metrics, one for the control group and one for the treatment group. Here is the detail of our distributions:

```python
p_base = 0.1
p_treat = 0.12
se0 = np.sqrt(p_base * (1 - p_base) / n)
se1 = np.sqrt(p_treat * (1 - p_treat) / n)
```

We can plot both our distribution above with varying sample size n:

![Control vs treatment](/assets/img/control-v-treatment.png)

*Figure 1: Control versus treatment distribution*

We can see that as the sample size increases, the gap between the two distributions also increases. This is because the standard error decreases as the sample size increases. And this will affect our AB testing later.

Now that we have the control and treatment distribution, we can do aa hypothesis testing by constructing a third distribution, which is the distribution of difference in the means. I will not go over the theory too deep here, but this approach is based on the Central Limit Theorem, where the distribution of the difference between two means will approach a normal distribution as the sample size increases.

We calculate the se_diff, mu0, and mu1 

```python
# Standard Errors
se0 = np.sqrt(p_base * (1 - p_base) / n)
se1 = np.sqrt(p_treat * (1 - p_treat) / n)
se_diff = np.sqrt(se0**2 + se1**2)

mu0 = 0
mu1 = mde
```

We set mu0 to 0 because this is our null hypothesis. We assume that there is no mean difference in both control and treatment, whereas mu1 is our mde, which is the effect that we want to see.

Now we can plot the distribution of this mean difference and see how all different components interact and how does the shape of the distribution change as we adjust the sample size. 

![AB testing distributions](/assets/img/ab-testing-distributions.png)
*Figure 2: Comparison of difference distributions with expanded range*

The solid black line (threshold) represents the critical value, which is the value that we use to determine if our result is statistically significant. This black line is calculated based on the z-score of the alpha value and the standard error of the difference in means.

```python
# Significance Threshold (Critical Value)
z_alpha = norm.ppf(1 - alpha)
x_crit = mu0 + z_alpha * se_diff
```

We can see at $n=200$ the threshold is actually to the right of the green dashed line (the MDE). This shows that with such a small sample, even if we hit our exact target of $0.02$, it wouldn't be "statistically significant." We would need a massive, lucky fluke to actually reject the null. As $n$ hits $20,000$, the black threshold line moves significantly to the left of our $0.02$ target, meaning almost any result near our MDE will be correctly identified as a winner.

Notice how the power changes as we have increased sample as well. Power is the entire rest of the green curve to the right of the significant line. It represents all the times the treatment actually worked and we correctly identified it as a winner. If we have power of 80%, this means that if the 2% lift is real, we will correctly identify it 4 out of 5 times.

Now we can see that there is indeed a relationship between the MDE, alpha, beta and the minimum sample size required. For a test to be valid in detecting our MDE, we need to have a sample size that is large enough to reject the null hypothesis.

# Deciding sample sizes

In an AB test, what we want usually is to determine the minimum sample size to be able to conclude an experiment given the experiment config. Hence, what we usually do is that we set alpha, beta (power), and our MDE to calculate how many samples do we need to run the AB test.

Here is how the image looks if we do one-tail test with alpha=5%, beta=20%, and MDE of 0.02

![Min sample size](/assets/img/one-tail-min-sample-size.png)
*Figure 3: Distribution of difference when by setting alpha=5%, beta=20%, and MDE of 0.02*

Here we observe the following:

At $n=3,024$, the significant threshold is positioned perfectly so that 5% of the Blue Curve is to its right (our $\alpha$ or False Positive risk) and 20% of the Green Curve is to its left (our $\beta$ or False Negative risk)

We can also observe that he curves are just "skinny" enough that the Green Shaded Area (Power) covers exactly 80% of the Treatment distribution. This means if the 2% lift is   real, we will correctly identify it 4 out of 5 times.

Also even if our result lands slightly to the left of the "True Effect" ($0.02$), as long as it stays to the right of the black threshold ($\approx 0.013$), we still detect it as statistical significant.

# Next post

In the next post we will go over on what happens if both control and treatment are equally splitted, meaning that it will effect the standard error of the distribution of mean difference.




