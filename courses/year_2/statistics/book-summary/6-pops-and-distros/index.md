# 6. Populations and distributions

## Population

For a given variable, we call the *population* of the variable the range of possible values that the variable may have.

- **Random variable**. We use this term to mean a random number drawn from a population. There's a distinction between an observed or realized random variable and one that is not. An observed random variable has a concrete value, an unrealized one, on the other hand, has potentially any value from the population.
- **Distribution of a random variable**. As mentioned, an unobserved random variable has the potential to be any value of a population, the shape of the likelihood for the values that the variable may take is what we call its *distribution*, though it may alternatively also be called *population*.
- **Probability**. A number from 0 to 1 that describes the likelihood of a value occurring in a random variable. The number represents the proportion of times that said value might be observed happening with respect to the number of total observations.

    Let an *event $E$* be a set of outcomes, for a flat distribution, one where all outcomes are equally likely, with a finite number of outcomes, we can model the probability of an event as

    $$P(E) = \frac{\small\text{\# events in E}}{\small\text{\# events in total}}$$

    This implies that

    - $P(E) \geq 0$ for all events
    - The event of all possible outcomes has a probability of 1
    - For any two events, $P(A \lor B) = P(A) + P(B) - P(A \land B)$

- **Population mean**. Denoted by $\mu$, if X is a random variable with a population, then the mean is also called the *expected value of X*, and is often also written as $\text E(X)$.

    For discrete data, the mean can be calculated as

    $$\mu = E(X) = \sum kP(X=k)$$

    For continuous data, calculus is necessary. In the specific case of a uniform distribution on $[0,1]$ it is $1/2$.

- **Population standard deviation**. Denoted by $\sigma$ or $\text{SD}(X)$, it is the square root of the variance.

    For a discrete random variable X, the variance can be calculated as

    $$\sigma^2 = \text{VAR}(X) = E((X-\mu)^2)$$

    For continuous variables, calculus is necessary. In the specific case of a uniform distribution on $[0,1]$, the standard deviation is $1/12$

- **p.d.f.** Probability distribution or density function, depending on if the variable is discrete or continuous, respectively. In both cases the p.d.f.  is a function that plays the role of showing the likelihood of a specific event happening.

    For a discrete X, this is defined as

    $$f(k) = \text P(X=k)$$

    For a continuous X the *density of X*, is a function $f(x)$ such that

    $$\int^b_a f(x) \, dx = \text P (a < X \leq b)$$

    There's also the related function $F(b) = \text P(X\leq b)$.

- **c.d.f**. The cumulative distribution function is the function $F(b) = \text P(X \leq b)$ that returns the cumulative probability of all values of X up to a certain point $b$.

    For a discrete X this is

    $$F(b) = \sum^b_k \text P(X=k)$$

    For a continuous X, it is

    $$F(b) =  \int^b f(x) \, dx$$

- **Quantile**. The inverse of the c.d.f., the quantile function returns the point $b$ for which the area of the probability of X is equal to a given $p$.

### Discrete random variables

### Continuous random variables

- **Range**. The range of $X$ is the set of all $k$ such that $P(X=k) > 0$.
- **Distribution**. The distribution of $X$ is the specification of all the probabilities in the range. As per the rules of probability, the distribution is not arbitrary, as for all $k$ in the range we have  $P(X=k) > 0$ and $P(X=k) \leq 1$.

- **Density**. Instead of trying to define the probability in an infinite amount of terms of $P(X=k)$, for continuous data, we use ranges $P(a < X \leq b)$. For this, we have a function $f(x)$ which, for a given variable X, is called the *density* of X. This function has the property that the area under its curve between two points is equal to the probability of X being between those two points. So we define the related function $F(b)=P(X\leq b)$.

    In other words, $f(x)$ gives us the shape of the density of X, while $F(b)$ gives us the probability for X under a certain value, as defined by the area under the density until that value.

### Sampling

- **Sample**. a sample is a sequence of random variables from a common parent population. It is **identically distributed** if each individual variable has the same distribution. It is **independent** if knowing the value of some variables does not affect the outcome of the rest. It is a **random** or **i.i.d. sample** if it is both independent and identically distributed.

    R has the function `sample()` which can be used to generate random samples with the parameter `replacement=TRUE`.

- **Statistic**. A numeric value that summarizes a random sample, such as the median or mean. A statistic of a random sample is also a random variable, as such, it is denoted using capital letters: $\bar X$. A statistic whose distribution doesn't change dramatically for moderate changes in the parent population is called a *robust statistic*.
- **Sampling distribution.** The distribution of a statistic. These distributions can be quite complex, however, for many common statistics the properties of such distributions are known.

## Families of distributions

Distributions come in families described by a function with a series of parameters that define the specific distribution.

R has series of functions for getting information about a family of distributions. These functions start with the letters `d` (p.d.f.), `p` (c.d.f.), `q` (quantiles), `r` (random samples) and are followed by an identifier for the specific family, such as `unif` or `norm`.

### Bernoulli distribution

A Bernoulli random variable X measures the chance of success vs failure. A sequence of these variables is known as a sequence of *Bernoulli trials*.

The variable is denoted by $\text{Bernoulli}(p)$, where $p$ is the probability of success.

Distribution:

$$\text P(X=1) = p$$

Mean:

$$\mu = p$$

Standard deviation:

$$\sigma^2 = p(1-p)$$

There is no specific R function family for Bernoulli distributions, but you can use `sample(0:1, replace=TRUE, prob=c(1-p,p))` to sample it.

### Binomial distribution

A discrete distribution that, for a random variable X, counts the number of successes in $n$ Bernoulli trials.

The variable is denoted by $\text{Binomial}(n,p)$, where $n$ is the number of trials, and $p$ is the probability of success.

Distribution:

$$P(X=k) = {n \choose k}p^k(1-p)^{n-k}$$

Mean: 

$$\mu = np$$

Standard deviation:

$$\sigma = \sqrt{np(1-p)}$$

You can use the family of functions `_binom()` in R to analyze the normal distribution

### Normal distribution

A continuous distribution that defines the meaning of "bell-shaped". It is used to describe many naturally occurring phenomena, as well as the sampling distribution of many statistics.

The variable is denoted by $\text{Normal}(\mu, \sigma)$, sometimes $\text{Normal}(\mu, \sigma^2)$. A standard normal is the name given to the normal distribution with mean 0 and spread 1. A property of this distribution is that for any random variable X, the z-score is a standard normal.

Density:

$$f(x|\mu, \sigma)=\frac{1}{\sqrt{2 \pi \sigma^2}}e^{-\frac{1}{2\sigma^2}(x-\mu)^2}$$

You can use the family of functions `_norm()` in R to analyze the normal distribution

### Uniform distribution

The uniform distribution on $[a,b]$ is useful for populations that have no preferred values in their range.

We denote it $\text{Uniform}(a,b)$, where $a$ and $b$ are the bounds of the distribution.

Density:

$$f(x|a,b) = \frac{1}{b-a}$$

Mean:

$$\mu = \frac{a+b}{2}$$

Variance:

$$\sigma^2=\frac{(b-a)^2}{12}$$

You can use the family of functions `_unif()` in R to analyze the normal distribution

### Exponential distribution

An example of a very skewed distribution. Popular with populations such as the time a light-bulb lasts, or the half-life of a radioactive isotope.

Denoted by $\text{Exponential}(\lambda)$, where $\lambda$ is related to the mean and sd as detailed below.

Density: for $x \geq 0$

$$f(x|\lambda)=\lambda e ^{-\lambda x}$$

Mean:

$$\mu = \frac{1}{\lambda}$$

Standard deviation:

$$\sigma = \frac{1}{\lambda}$$

### Weibull distribution

A generalized case of the exponential distribution.

It is denoted by $\text{Weibull}(s,z)$, where $s$ control the decay, and $z$ controls the initial growth. When $\text{z}=1$ it is identical to the exponential distribution.

### Logonormal distribution

A heavily skewed distribution on the positive numbers. Useful for populations with a hard lower-bound but no concrete upper-bound, such as income. The random variable $X$ when transformed to $\log(X)$, follows a normal distribution.

You can use the family of functions `_lnorm()` in R to analyze the normal distribution. Its parameters `meanlog=` and `sdlog=` are the mean and sd of $\log(X)$ not $X$.

### Sampling distributions

- **Sample mean**.

    Mean:

    $$\mu_{\bar X} = \mu$$

    Standard deviation:

    $$\sigma_{\bar X} = \frac{\sigma}{\sqrt{n}}$$

These are used to describe other sampling distributions

- $**t$-distribution**.
- $F$-distribution
- $\chi^2$-distribution

You can use the families of functions `_t`, `_f`, and `_chisq`.

## Central limit theorem

As the sample size of a statistic increases, its distribution narrows around a center equal to the statistic of the population.

For the case of the mean, this is known as the *law of large numbers*. The central limit theorem states that for any parent population, the sampling distribution of $\bar X$ for a sufficiently large $n$ satisfies

$$\text P\left(\frac{\bar X - \mu}{\sigma / \sqrt{n}} \leq b \right) \approx \text P (Z \leq b)$$

Where $Z$ is a standard normal random variable. A fact about this theorem is that we can replace $\sigma$ with the sample standard deviation $s$ when we standardize $\bar X$ and still get

$$\text P\left(\frac{\bar X - \mu}{ {\color{red} s}\,/\sqrt{n} } \leq b \right) \approx \text P (Z \leq b)$$

This theorem only applies for large $n$. But what is a large enough $n$? it is of a size such that the sampling distribution is normal, which is about 30 for symmetric, unimodal populations.
