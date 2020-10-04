# 9. Significance tests

## Significance tests

A significance test is an alternative form of statistical inference form confidence values. In a significance test we assume a value for the population parameter, and then compute its probability based on a sample taken.

- **The null-hypothesis**. Denoted by $H_0$, this hypothesis is generally some assumptionthat we wish to challenge. We will assume this hypothesis to be true unless we can prove, with a certain degree of certainty, that it is not the case.
- **The alternative hypothesis**. Denoted by $H_\text A$, this hypothesis is the theorized new state or the state with which we challenge $H_0$. If $H_0$ is rejected, it does not mean that $H_\text A$ will be accepted.
- **Test statistic**. This statistic is generated from the sample taken from the population, and it is the basis by which we will assess the hypotheses.
- $**p$-value**. It's the chance that the sample would produce the values it produced in the test statistic (or values that are yet more extreme), assuming that the null hypothesis holds.

    $$p\text{-value} = \text P(\text{test statistic}_\text{ expected} \succeq \text{test statistic}_\text{ observed}\ |\ H_0)$$

    The smaller the value, the more likely it is that $H_0$ is false. A small value is called *statistically significant*.

    ![p-values significance table](table.png)

    Levels of significance for ranges of p-values

    When a $p$-value is small enough, the exact value depending on the desired certainty, the null hypothesis is rejected, otherwise, it is accepted. This, however, doesn't necessarily imply anything about the validity of the hypothesis, as extraordinary values can occur.

    - **Type I error**. When the null-hypothesis is rejected even though it's true. The chance of a type I error occurring is given by our choice of $\alpha$ which, by convention, tends to be 0.05
    - **Type II error**. When the null-hypothesis is accepted, even though it's false. The probability of a type II error occurring is denoted by $\beta$ which, by convention, ought to be below 0.2.
    - **Power**. the power of a test is equal to  $1-\beta$, this value depends on the size of our sample, which should be chosen so that the chance of errors are bounded within acceptable limits.
- **Calculating the $p$-value**.
    1. Specify a model for the underlying data. This is the distribution and its parameters.
    2. Identify the null and alternative hypotheses.
    3. Specify a test statistic to discriminate between the two hypotheses. The sampling distribution of the statistic needs to be known under the assumption of $H_0$.
    4. Collect the data and find the observed value of the test statistic.
    5. Using $H_\text A$, define the values that are considered extreme under $H_0$.
    6. Calculate the $p$-value with the help of the quantile function.

## Population proportion

We may want to know if a proportion in a population has changed, or if two sample proportions belong to the same underlying population.

- $\text{H}_0:$  $p = p_0$, that is, we assume that the currently unknown proportion $p$ is equal to our "baseline" $p_0$.
- $\text{H}_\text A:$ one of $p \not= p_0$,  $p < p_0$,  or $p > p_0$.  In other words, the currently unknown proportion will be either greater than, less than, or different from our baseline, depending on our assumptions.

A test can then be performed with the statistic

$$Z= \frac{\hat p - \text E(\hat p | H_0)}{\text{SD}(\hat p | H_0)} = \frac{\hat p - p_0}{\sqrt{p_0(1-p_0)/n}}$$

Where $Z$ has a normal distribution under $H_0$ given that $\hat p$ is the proportion observed on a simple random variable with large enough $n$.

- **Standard deviation**. In this case we assume that $p_0$ is known, so we can operate with the standard deviation (as opposed to the standard error used for confidence intervals).

    $$\text{SD}(\hat p | H_0) = \sqrt{\frac{p_0(1-p_0)}{n}}$$

- **Calculating the p-value**. The p-value is the probability that the assumed distribution will yield the result observed, thus

    $$p\text{-value} = \begin{cases}
    F_\text{ Normal} (\hat p) & \text{for }H_A: p < p_0 \\
    \text 1 - F_\text{ Normal} (\hat p) & \text{for }H_A: p > p_0 \\
    \text 2\times F_\text{ Normal} (p - |p - \hat p|) & \text{for }H_A: p \not= p_0
    \end{cases}$$

    where $F_\text{ Normal}$ is the c.d.f. for a $\text{Normal}(p_0 , \text{SD}(\hat p | H_0))$ distribution.

    You can use `prop.test(x, n, p=..., alternative="two.sided")`, where `x` is the sample, `n` is the size of the sample, `p` is $p_0$, and `alternative` indicates the alternative hypothesis and can be `less`, `greater`, or `two.sided`.

- **Power**. Power calculations are one to determine an appropriate sample size that will reduce the chance of type-I and II errors.

    R provides functions for doing power calculations. For proportions, this is `power.prop.test()`, take a look at the documentation to see how it works.

## Mean

Significance test can be constructed for the unknown mean of a parent population.

- $\text{H}_0:$  $\mu = \mu_0$, that is, we assume that the currently unknown mean $\mu$ is equal to our "baseline" of $\mu_0$.
- $\text{H}_\text A:$ one of $\mu\not= \mu_0$,  $\mu < \mu_0$,  or $\mu > \mu_0$.  In other words, the currently unknown mean will be either greater than, less than, or different from our baseline mean, depending on our needs.

For a population with a normal distribution, or for large a enough $n$, the following statistic is appropriate

$$T = \frac{\bar x - \text E(\bar x | H_0)}{\text{SD}(\bar x | H_0)} = \frac{\bar x - \mu_0}{s/\sqrt{n}}$$

- **Calculating the p-value**. The p-value is the probability that the assumed distribution will yield the result observed. Let $t=(\bar x - \mu_0)/(s/\sqrt n)$ be the observed value for the test statistic,

    $$p\text{-value} = \begin{cases}
    F_t (t) & \text{for }H_A: \mu < \mu_0 \\
    \text 1 - F_t (t) & \text{for }H_A: \mu > \mu_0 \\
    \text 2\times (1-F_t (|t|)) & \text{for }H_A: \mu \not= \mu_0
    \end{cases}$$

    where $F_t$ is the c.d.f. for a $\text{t}(n-1)$ distribution.

    You can use `t.test(x, mu=..., alternative=...)`, where `x` is the sample, `mu` is $\mu_0$, and `alternative` indicates the alternative hypothesis and can be `"less"`, `"greater"`, or `"two.sided"`.

- **Power**. Power calculations are one to determine an appropriate sample size that will reduce the chance of type-I and II errors.

    R provides functions for doing power calculations. For the mean, this is `power.t.test()`, take a look at the documentation to see how it works.

## Median

Mean calculations rely on large samples or an assumption that the parent population is normally, or nearly normally, distributed. When this is not the case we can take a non-parametric approach and calculate the significance of the median.

### Sign test

Assuming that the distribution of the parent population is continuous and with a positive density, we have

- $\text{H}_0:$  $\text{median} = m$. Where $m$ is the "baseline" value, and $\text{median}$ is the value to be observed from the sample.
- $\text{H}_\text A:$ $\text{median} <m,\ >m,\ \text{or } \not= m$

Calculations can be performed with the test statistic

$$T = \#( X_i ) \text{ such that } X_i > m$$

Ignore any values equal to $m$. Under $H_0$, this statistic has a $\text{Binomial}(n,1/2)$ distribution.

- **Calculating the p-value**. The p-value is the probability that the assumed distribution will yield the result observed. Let $t$ be the observed value for the test statistic,

    $$p\text{-value} = \begin{cases}
    1 - F_\text{ Binom} (t - 1) & \text{for }H_A: \text{median} > m \\
    \text 1 - F_\text{ Binom} (n - t - 1) & \text{for }H_A: \text{median} < m \\
    1-F_\text{ Binom} (\text{max}(n-t,t)-1) & \text{for }H_A: \text{median} \not= m
    \end{cases}$$

    where $F_\text{ Binom}$ is the c.d.f. for a $\text{Binomial}(n,1/2)$ distribution.

    There is no basic function for this in R, but we can calculate the test statistic using `sum()` and use `pbinom()` for the binomial c.d.f.

### Signed rank test

Better than the sign test, it relies on the assumption that the population has a continuous and symmetrical distribution.

- $\text{H}_0:$  $\text{median} = m$. Where $m$ is the "baseline" value, and $\text{median}$ is the value to be observed from the sample.
- $\text{H}_\text A:$ $\text{median} <m,\ >m,\ \text{or } \not= m$

Here we use the test statistic

$$T = \sum_{i:\ x_i > m}\text{rank}(|x_i - m|)$$

- **Calculating the p-value**.

    For this method we can use the `wilcox.test(x, mu=..., alternative=...)` function, where `mu` is the null hypothesis.

## Two-sample tests of proportion

Sometimes we wish to compare the values of two samples, as opposed to a single sample against an assumption.

- $\text{H}_0:$  $p_1 - p_2 = 0$
- $\text{H}_\text A:$ $p_1 - p_2 < 0$,  $>0$,  or $\not=0$

The statistic for this test is

$$Z = \frac{\hat p_1 - \hat p_2}{\sqrt{\hat p (1- \hat p)\left(\frac{1}{n_1}+\frac{1}{n_2}\right)}}$$

where $\hat p$ is the combined sample of $\hat p_1$ and $\hat p_2$

$$\hat p = \frac{n_1\, \hat p_1 + n_2\, \hat p_2}{n_1 + n_2}$$

Large values for $Z$ support $p_1 > p_2$, small values support $p_1 < p_2$.

- **Calculating the p-value**.

For this, we can use the function `prop.test(x, n, alternative=...)`, where `x` is a vector containing the success counts for the two samples, and `n` is a vector containing the total counts for the respective samples.

## Two-sample test of center

Similarly to two-sample tests of proportion, we might wish to test the significance in the difference between the centers of two samples, as opposed to testing a single sample against an assumption.

### For normal populations

Let our hypotheses be

- $\text{H}_0:$  $\mu_x = \mu_y$
- $\text{H}_\text A:$ $\mu_x > \mu_y$,  $\mu_x < \mu_y$,  or $\mu_x \not= \mu_y$

As $\bar x$ and  $\bar y$ estimate $\mu_x$ and $\mu_y$, we can use $\bar x - \bar y$ as a good estimate for $\mu_x - \mu_y$. We use the following statistic:

$$T=\frac{(\bar x - \bar y) - (\mu_x - \mu_y)}{\text{SE}(\bar x - \bar y)}$$

- **Standard error**. As we did with confidence intervals, the value of the standard error depends on our assumptions on the variance of the parent populations of the samples

    $$\text{SE}(\bar x - \bar y) = \begin{cases}
    \sqrt{s_p^2\left(\frac{1}{n_x} + \frac{1}{n_y}\right)} \qquad &\text{if }\sigma_x=\sigma_y \\[2ex]
    \sqrt{\frac{s_x^2}{n_x} + \frac{s_y^2}{n_y}} \qquad &\text{if }\sigma_x\not=\sigma_y
    \end{cases}$$

    Where $s_p^2$ is the pooled variance, which is more accurate due a to larger sample size.

    $$s_p^2 = \frac{(n_x-1)s_x^2 + (n_y-1)s_y^2}{n_x + n_y - 2}$$

- **Degrees of freedom**. For the $t$-distribution of the difference, these are

    $$\text{degrees of freedom} = \begin{cases}
    n_x + n_y - 2\qquad&\text{if }\sigma_x = \sigma_y\\[2ex]
    \left(\frac{s_x^2}{n_x} + \frac{s_y^2}{n_y}\right)^2 \times \left(\frac{s_x^2/n_x}{n_x-1} + \frac{s_y^2/n_y}{n_y-1}\right)^{-1}\qquad&\text{if }\sigma_x \not= \sigma_y
    \end{cases}$$

- **Calculating the p-value**.

    The function `t.test(x, y, alternative=..., var.equal=FALSE)` will calculate the p-value

### For matched samples

When the two samples depend on each other in some way, such as in paired studies, or follow-up samples. If the two samples are matched so that $x_i - y_i$ is an i.i.d sample, then we can test the significance of the hypotheses

- $\text{H}_0:$  $\mu = 0$
- $\text{H}_\text A:$ $\mu_x > 0$,  $< 0$,  or $\not= 0$

If the differences are from a symmetric distribution, then the Wilcoxon signed-rank test can be used. Otherwise, the sign test will do, where $\mu$ is interpreted to be the difference between the medians.

Both `t.test` and `wilcox.test` have parameters to indicate that paired tests should be performed.

### Wilcoxon rank-sum test for equality of center

The two-sample t-tests measure if two samples have different centers when the parent populations are normally distributed. However, sometimes the distribution may be heavily skewed. However, if we can assume that our two samples come from identical populations, up to a shift of center, the we can do a Wilcoxon rank-sum test.

For two samples $x_i$ and $y_i$ with distribution densities $f(x-\mu_x)$ and $f(x-\mu_y)$ (same-shaped, except for a different center), let our hypotheses be

- $\text{H}_0:$  $\mu_x = \mu_y$
- $\text{H}_\text A:$ $\mu_x > \mu_y$,  $\mu_x < \mu_y$,  or $\mu_x \not= \mu_y$

We can use the function `wilcox.test(x, y, alternative=...)` to calculate this, where `x` and `y` store the two datasets.

## Confidence intervals and significance tests

You may have noticed that the R functions for confidence intervals and significance tests are the same. This is because in both cases we use the same statistic, just in different ways.

In a confidence interval, we are determining the probability that a statistic is within a range of our sample statistic. While in a significance test, we are determining the probability of a value existing within a certain range away from a statistic.

The choice of one or the other method depends on various things, but many prefer confidence intervals, as significance tests can almost always be made to reject the null-hypothesis by choosing an arbitrarily large $n$.
