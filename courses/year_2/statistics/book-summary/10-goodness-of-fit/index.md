# 10. Goodness of fit

Sometimes we significance tests for categorical data with more than the two categories we've been dealing with so far (success, and failure). In such cases, we can do a goodness-of-fit test.

## The multinomial distribution

A probability model for categorical data can be defined with the *multinomial distribution**.***

Distribution:

$$\text P(y_1 = y_1, \dots, y_k = y_k) = {n \choose y_1}{n - y_1 \choose y_2}\cdots{n - y_1 - \dots y_{k-1} \choose y_k} p_1^{y_1}\dots p_k^{y_k}$$

where $m$ is the number of categories and $n$ is the population size.

## Pearson's chi-squared statistic

Using the multinomial distribution directly is hard, as is only gives values for a particular observation and all the variables are correlated. So we use the $\chi^2$ statistic, which grows the more an observation deviates from the expected values.

$$\chi^2 = \sum_{i=1}^k \frac{(y_i - np_i)^2}{np_i}=\sum \frac{(\text{observed} - \text{expected})^2}{\text{expected}}$$

Given that the multinomial model is appropriate for the data, the distribution of a $y_i$ approaches that of the $\chi ^2$ distribution with $k-1$ degrees of freedom, for $k$ number of categories, if the expected values for all $y_i \geq 5$.

### The chi-squared significance test for goodness-of-fit

Given a set of categories with values $y_1,\dots, y_k$, and a multinomial model with probabilities $p_1,\dots,p_k$. Let our hypotheses be

- $\text H_0: p_1 = \pi_1,\dots,p_k = \pi_k$, where $\pi_i$ are the expected probabilities and $p_i$ are the observed probabilities. That is, we assume the data reflects our expectations across all categories.
- $\text H_\text A : p_i \not = \pi_i$ for at least one $i$. Meaning that some or all categories deviate from what we expected to see.

A significance test can then be performed with the help of the $\chi^2$ statistic.

$$\chi^2 = \sum_{i=1}^k \frac{(y_i - np_i)^2}{np_i}$$

- **Calculating the p-value**. The p-value is the probability that the assumed multinomial model will yield either the result observed or something more extreme.

    $$p\text{-value} = F_\text{chi-squared}\left(\chi^2\right)$$

    Where $F_\text{chi-squared}$ is the c.d.f. for a $\text{Chi-squared}(k-1)$ distribution.

    R provides us with `chisq.test(x, p=...)`, where the data is in tabulated form in `x`, and the probabilities re given as a vector in `p=`.
    We also have the `_chisq` family of functions for other distribution-related calculations.

### The chi-squared test of independence

Sometimes we need to test whether some variables are independent of one another. That is, whether there is some correlation between the values that two variables take. This is usually expressed in a table where the columns represent the states for one variable, the rows represent the other, and the cells contain the observed counts when the two variables held those values.

For this, we perform a significance test. Let $p_{ij}$ be the observed probability in a given cell. Let $n_r$ and $n_c$ be the number of rows and columns in the table, respectively. Let $p_i^r$ be the marginal probability of row $i$, that is, the sum of all the values in that row. And let $p_j^c$ be the marginal probability of column $j$.

- $\text H_0: p_{ij} = p_i^r p_j^c$, in simpler terms, our null hypothesis is that the variables are independent. They are each the product of their marginal distributions.
- $\text H_\text A : p_{ij} \not= p_i^r p_j^c$ or, the variables are not independent, as their values are not a result of their distribution

We adapt the $\chi^2$ statistic for this problem

$$\chi^2 = \sum \frac{(\text{observed} - \text{expected})^2}{\text{expected}} = \sum_{i=1}^{n_r} \sum_{j=1}^{n_c} \frac{(y_{ij} - n\hat p_{ij})^2}{n\hat p_{ij}}$$

Where $\hat p_{ij}$  is an estimiate of  $p_{ij}$ based on the observed values. That is  $\hat p_{ij} = \hat p_i^r \hat p_j^c$, and $\hat p_i^r, \hat p_j^c$ are the observed marginal distributions.

Use the function `chisq.test(x)` where `x` is your table. Use the parameter `simulate.p.value=TRUE` when your table has cells with values under 5. This will use a Monte Carlo simulation to help populate the table a bit more.

### The chi-squared test of homogeneity

When we want to calculate whether two samples with categorical values have the same underlying distribution we do a test of homogeneity.

We lay down the data in a two-row table (for our two samples). Let the random variable be the column variable. Let $p_{ij}$ be the probability that the variable will give the result $j$ for the row $i$.

- $\text H_0:p_{ij} = p_j$ for all rows $i$. In other words, the two distributions are the same
- $\text H_\text A : p_{ij} \not= p_j$ for any row $i$. In other words, the two distributions are different

The test for this is the same as in the test for independence above.

## Goodness-of-fit tests for continuous distributions

To calculate whether continuous data is sampled from a given distribution

### The Kolmogorov-Smirnov goodness-of-fit test

Assume $x_1,x_2,\dots,x_n$ is an *i.i.d.* sample from a continuous distribution
with c.d.f. $F(x)$. Let $F_n(x)= \#\{i\ |\ x_i \leq x \}/n$ be the empirical c.d.f.

- $\text H_0: F(x) = F_\text{expected}(x)$, where $F_\text{expected}(x)$ is the c.d.f. for the expected distribution and $F(x)$ is the c.d.f for the observed distribution
- $\text H_\text A: F(x) \not= F_\text{expected}(x)$

For this we use the $\text D$

$$D = \text{max}(|F_n(x)-F(x)|)$$

Small $\text D$s support the null-hypothesis, large $\text D$s support the alternative.

Use the function `ks.test(x, y="name", ...)` where `x` stores the data, `y` sets the distribution name, the other arguments are used for supplying the selected distribution's parameters.

### The Shapiro-Wilk test for normality

The Kolmogorov-Smirnov works if we have a fully specified expected distribution under the null-hypothesis (we should not calculate the parameters for the distribution from our sample). If we want to test whether the sample belongs to a normal population, without knowing any of the parameters, we can use the Shapiro-Wilk test.

The definition for the test is not given in the book.

- $\text H_0:$ parent distribution is normal
- $\text H_\text A:$ parent distribution is not normal

To test this, we use the `shapiro.test(x)` function. With `x` being a vector with the data.