# 8. Confidence intervals

The following formulas assume a sufficiently large $n$. What that means is discussed at the end of [6. Populations and distributions](6%20Populations%20and%20distributions%206f5d1c90df464f0f8655bda51bbd70d6.md).

## Confidence intervals for a population proportion, or $p$

It is rarely feasible to query an entire population in order to compute the probability of an event. Instead, it is much easier to query a subset of the population, a sample.

- **Sample proportion**. denoted by $\hat p$, it is the proportion of successes observed in a random sample. That is $\text{Binomial}(n,p)$, for a random sample of size $n$ with a proportion of success $p$. $\hat p$ can be used to estimate the value of $p$ with a certain degree of certainty ue to the fact that the following statistic has a normal distribution.

    $$Z=\frac{\hat p - p}{\text{SE}(\hat p)}$$

- **Standard deviation of $\hat p$**.

    $$s = \text{SD}(\hat p) = \sqrt{\frac{p(1-p)}{n}}$$

- **Standard error of $\hat p$**. It is the standard deviation, except the unknown parameters are replaced by the sample estimates.

    $$\text{SE}(\hat p) = \sqrt{\frac{\hat p(1-\hat p)}{n}}$$

- **Margin of error**. the margin of error refers to the size of the interval within which an observation falls with a given confidence level. It is calculated by

    $$Q\left(1-\frac{\alpha}{2}\right)\times\text{SE}(\hat p)$$

    Where $\alpha = 1 - \text{confidence level}$.

- **Confidence interval**. It is the range of values within which an estimate falls in for a given confidence level. The confidence interval for $p$ can be expressed as:

    $$\hat p \pm z^* \times \text{SD}(\hat p)$$

    Where $z^*$ is the standard normal multiplier, equal to $Q\left(1-\frac{\alpha}{2}\right)$.

    The function `prop.test(x, n, conf.level = 0.95)` will calculate a confidence interval for a certain confidence level given the number of successes `x` and the total number of trials `n`. *This function uses a slightly different formula than the one given by the book, so the results will not be the exact same.*

## Confidence intervals for the population mean, or $\mu$

Given a random sample $x_1, x_2, \dots , x_n$. The following describes properties about the sample mean $\bar x$.

- $**t$-distribution**. It is a bell-shaped distribution with a single parameter, degrees of freedom. It is similar to the standard normal distribution, except that for small values of the degrees of freedom, it has fatter tails. It is the distribution, with $n-1$ degrees of freedom, for the following statistic

    $$T=\frac{\bar x - \mu}{\text{SE}(\bar x)}$$

    The $t$-distribution allows us to solve for either $\alpha$ or $t^*$, provided the other is known.

- **Confidence interval for the mean**. It is the range of values within which the mean $\mu$ of a normal population with a standard deviation $\sigma$ falls in for a given confidence level. The confidence interval for $\mu$ can be expressed as:

    $$\bar x \pm t^* \times \text{SE}(\bar x)$$

    Where $t^*$ is equal to

    $$Q_{t}\left(1-\frac{\alpha}{2}\right)$$

    Where $Q_t$ is the quantile function for the $t$-distribution with $n-1$ degrees of freedom

- **One- vs two-sided intervals**. So far we've discussed intervals that cover equal areas on both sides of the center. This doesn't have to be so, the area subtracted by $\alpha$ can be on a single side of the distribution, yielding what is called a one-sided interval.

## Other confidence intervals

So far, we've used the key fact that certain statistics have known sampling distributions that do not involve any parent population parameters. Such statistics are called *pivotal quantities*, and can be used to generate confidence intervals in various situations.

We can apply the same procedures we've applied so far to generate these.

## Confidence intervals for differences

When samples are taken at different point in time, the results will vary. We need a way to determine whether this difference is due to chance, or due to an underlying change in the population.

The expectation is that a difference of $0$ is within the confidence interval. If it is not, it means that, for the given certainty value, there appears to be an underlying difference in the parent population.

### Difference of proportions

We compare two proportions $\hat p_1$ and $\hat p_2$, to see if the difference between them is explainable by sampling error, we rely on the Z statistic of the difference:

$$Z=\frac{(\hat p_1-\hat p_2) - (p_1 - p_2)}{\text{SE}(\hat p_1-\hat p_2)}$$

- **Standard error**.

    $$\text{SE}(\hat p) = \sqrt{\frac{\hat p_1(1-\hat p_1)}{n_1} + \frac{\hat p_2(1-\hat p_2)}{n_2}}$$

The procedure past this is equivalent to that of the regular proportion confidence interval.

The function `prop.test(x, n, conf.level = 0.95)` can also be used here, with `x` being a vector of the success counts, and `n` being a vector of the sample sizes.

### Difference of means

Let $x_1, x_2, \dots , x_n$ and $y_1, y_2, \dots , y_n$ be two samples with sample means $\bar x$ and $\bar y$ and sample standard deviations $s_x$ and $s_y$. We rely on the T-statistic for this

$$T=\frac{(\bar x - \bar y) - (\mu_x - \mu_y)}{\text{SE}(\bar x - \bar y)}$$

- **Standard error**. the standard error of $\bar x - \bar y$ is calculated differently depending on the assumption of whether the variances of the two samples are the same or not.

    First, for independent random variables, the variance of a sum is equal to the sum of the variances. This gives us that $\text{VAR}(\bar x - \bar y) = \sigma_x^2/n_x + \sigma_y^2/n_y$, we take the square root and normalize to get

    $$\text{SE}(\bar x - \bar y) = \begin{cases}
    s_p\sqrt{\frac{1}{n_x} + \frac{1}{n_y}} \qquad &\text{if }\sigma_x=\sigma_y \\[2ex]
    \sqrt{\frac{s_x^2}{n_x} + \frac{s_y^2}{n_y}} \qquad &\text{if }\sigma_x\not=\sigma_y
    \end{cases}$$

    Where $s_p^2$ is the pooled variance, which is more accurate due a to larger sample size.

    $$s_p^2 = \frac{(n_x-1)s_x^2 + (n_y-1)s_y^2}{n_x + n_y - 2}$$

- **Degrees of freedom**. For the $t$-distribution of the difference, these are

    $$\text{degrees of freedom} = \begin{cases}
    n_x + n_y - 2\qquad&\text{if }\sigma_x = \sigma_y\\[2ex]
    \left(\frac{s_x^2}{n_x} + \frac{s_y^2}{n_y}\right)^2 \times \left(\frac{s_x^2/n_x}{n_x-1} + \frac{s_y^2/n_y}{n_y-1}\right)^{-1}\qquad&\text{if }\sigma_x \not= \sigma_y
    \end{cases}$$

- Confidence interval.

    $$(\bar x - \bar y) \pm t^* \times \text{SE}(\bar x - \bar y)$$

The function `t.test(x, y, var.equal = TRUE/FALSE, conf.level = 0.95)` can be used here, with `x` and `y` being vectors of the two samples, and `var.equal` being the assumption on the equality of the variances.

### Matched samples

Sometimes we have two samples that may not be independent. They may involve matched or paired data in some way.

For the two samples $x_1, x_2, \dots , x_n$ and $y_1, y_2, \dots , y_n$ we have

$$T=\frac{(\bar x - \bar y) - (\mu_x - \mu_y)}{\text{SE}(\bar x - \bar y)}$$

However, in these cases, as the samples are not independent, the standard error of the two samples is not applicable, instead, we take it to be the standard error of the single sample $x_i - y_i$.

- **Confidence interval**.

    $$(\bar x - \bar y) \pm t^* \times \frac{s}{\sqrt{n}}$$

    Where $s$ is the standard deviation of the derived sample $x_i - y_i$, and $t^*$ is found from the $t$-distribution of $n-1$ degrees of freedom.

The function `t.test(..., paired = TRUE)` or `t.test(x - y, ...)` can be used for these cases

## Confidence intervals for the median

There are situations in which the distribution of the statistic is not known or not easily utilized. In these cases *non-parametric* methods are preferred.

### **Using the binomial distribution**.

Let $T$ count the number of points above the median in a sample of size $n$, then $T$ is a $\text{Binomial}(n, 1/2)$ random variable. We can then build a confidence interval by sorting and finding the largest index $j$ such that $\text P(j \leq T \leq n-j) < \alpha$. That is, we find the largest span of indices around the center of the sorted sample such that there's a probability smaller than $\alpha$ of $T$ being in that range.

To do this, we find the calculate $j = Q_\text{Binomial}(\alpha/2)$ of a $\text{Binomial}(n, 1/2)$ distribution. And find the value for the indexes $j$ and $n-j$. That's the interval.

### Signed-rank statistic

If we can reasonably assume that the distribution of our population is symmetrical around the *median*, then we can use the Wilcoxon signed-rank statistic, which works with the distance from the median, rather than the probability of a value being higher.

Since our assumptions about the distribution are stronger, this method will yield a smaller interval.

R provides us with a handy way to calculate this, using the `wilcox.test(x, conf.int = TRUE, conf.level = 0.95)` function, where `x` is the sample.

### Rank-sum statistic

When we want to compare two samples whose parent populations distributions are decidedly non-normal, the previous parametric methods will not be reliable. But, if we can assume that the two parent-distributions are the same up to a shift of center, then a non-parametric method can be used.

For two populations $x_1,x_2\dots ,x_n$ with density $f(x-\mu_x)$ and $y_1,y_2\dots ,y_n$ with density $f(x-\mu_y)$, the *rank-sum statistic* looks at all possible pairs of data and counts the number of times $x$ is greater than $y$.

We can use the `wilcox.test(x, y, conf.int = 0.95)` function with two sample values to calculate an interval based on this statistic, and see if the interval includes 0.

### Paired rank-sum statistic

If we have paired data, we can ensure a narrowed confidence interval

We can use `wilcox.test(..., paired = TRUE)` for this purpose