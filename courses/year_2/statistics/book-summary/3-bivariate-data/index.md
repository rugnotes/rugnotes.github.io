# 3. Bivariate data

## Independent samples

Independence refers to the fact that the existence or knowledge of a sample does not affect the odds or behavior of the others. An experiment with a control and a treatment groups results in independent samples, drawing cards from a deck does not.

- Stem and leaf plot
- Dot plots
- Box plots
- Density plots

## Paired data

### Correlation

A numeric summary of how closely related two measurements are. Perfect correlation and a plot of the two variables would follow a line, no correlation means they do not.

![3%20Bivariate%20data%206dd433ca361f4bf7910e0e3fb6c11e8e/Untitled.png](3%20Bivariate%20data%206dd433ca361f4bf7910e0e3fb6c11e8e/Untitled.png)

Correlation can also be calculated over summarized data, in which case the correlation values will tend to be higher than on the raw data.

- **Covariance**: quantifies the difference between points scattered in the four quadrants around the means as origin, versus the points scattered in just two opposing quadrants.

    ![3%20Bivariate%20data%206dd433ca361f4bf7910e0e3fb6c11e8e/Untitled%201.png](3%20Bivariate%20data%206dd433ca361f4bf7910e0e3fb6c11e8e/Untitled%201.png)

    The covariance formula is

    $$\text{cov}(x,y) = \frac{1}{n - 1} \sum_i (x_i - \bar x) (y_i - \bar y)$$

- The **Pearson correlation coefficient** is similar, except it uses z-scores

    $$\text{cor}(x,y) = \frac{\text{cov}(x,y)}{s_x\,s_y} = \frac{1}{n-1}\sum_i \left(\frac{x_i - \bar x}{s_x}\right) \left(\frac{y_i - \bar y}{s_y}\right)$$

    In both cases, a value close to 0 will mean little linear correlation, while values approaching an absolute value of 1 will mean that the values are arranged in a line

- **Spearman correlation coefficient**: used for monotonic relationships, that is, data which may be related as either increasing or decreasing, but which is not necessarily linearly related. In such cases, we first apply a transformation where we rank the data and then we apply the correlation coefficient on this transformation.

### Trends

- **Linear trends**: the model for a linear trend can be specified as *"The mean response value depends linearly on the predictor value"*

    $$\mu_{y|x}=\beta_0 + \beta_1x$$

    where $\mu_{y|x}$ means the mean of the response value $y$ for a specific value of the predictor $x$. For individual datapoints this becomes:

    $$y_i = \beta_0 + \beta_1x_i + \epsilon_i$$

    where $\epsilon_i$ are the error terms that, on average, tend closer to 0 the more values are considered.

- **Residual error**: the residual error can be defined as

    $$\text{residual} = \text{observed} - \text{expected} = y - \hat y$$

    where expected is the value calculated by the formulas above, that is, the error in y is the difference between the observed value of y for a given x and the value of y as calculated by our linear model for that same x.

- **Least squares regression line**: defined in terms of the residual error, it is the line which minimizes the square of the residuals. It is denoted as

    $$\hat y = \hat \beta_0 + \hat \beta_1\,x$$

    Solving the minimization problem for $\hat \beta_0$ and $\hat \beta_1$ results in

    $$\begin{array}{rl}\hat \beta_1 \!\!\!\!&= \text{cor}(x,y)\frac{s_y}{s_x} = \frac{\sum(x_i - \bar x)(y_i - \bar y)}{\sum(x_i - \bar x)^2} \\[3ex] \hat \beta_0 \!\!\!\!&= \bar y - \hat \beta_1 \, \bar x\end{array}$$

    Which tells us that (1) the slope of the line is proportional to the correlation of the two variables, (2) the predictor and the response variables are not interchangeable, (3) the intercept point is derived from the fact that the center of the scatter plot—$(\bar x, \bar y)$—is a point in the line, (4) the sum of the residuals will always be 0.

    R provides the `lm()` function, which is used to calculate the *linear model* for the given data. It can be visualized using `abline()`. You can also use `fitted()` to calculate the predicted value $\hat y$ for a given $x$, and `resid()` to get the residual value for a point.

- **Standard deviation line**: this line shows us the trend in terms of the z-scores of the various points. it is a line with a slope ${s_y}/{s_x}$ passing through $(\bar x, \bar y)$.

    In terms of this line, the *linear regression line* has a slope of  $\text{cor}(x,y)\left({s_y}/{s_x}\right)$.

- **Quantile regression line**: this line minimizes the absolute vertical distance between itself and the points, as opposed to the vertical distance squared of the *least-squares linear regression line*.

    For this we can use the function `rq(... , tau=0.5)` of the `quantreg` package.

    In a way it can be sad that this line tracks the rolling median of the data.

- **Principal component analysis line**: Another alternative is to minimize total perpendicular distance to the line. This is useful when working with data were neither variable is a natural predictor of the other and thus are somewhat interchangeable.
- **Robust regression lines**: just like the mean, the standard deviation and the correlation coefficients, linear regression lines are sensitive to outliers. Alternatives may try to counteract this.

    The `MASS` package provides various more robust alternatives to the `lm()` function. Such as the `rlm()` function.

- **Smoothers**: While the regression line is computed by considering all data, other lines such as the one provided by the function `loess()` calculate a local fit using polynomial curves.

### Transformations

Transformations can be used to adapt data that is monotonically related into a linear relationship. Common transforms are powers, roots, and logarithms.

The interpretation of models that have been transformed is different, for example if we are transforming both axes under a logarithm, then the errors are no longer additive but multiplicative.

## Bivariate categorical data

### Tables

Two way tables are a common way to display bivariate categorical data.

R offers: matrices for simple data, `table()` for better display, `margin.table()` for tables with marginal summaries, `prop.table()` for tables with proportional values instead of counts, `xtabs()` for converting dataframes into tables based on model formulas, and `ftable()` to better organize three-dimensional tables.

### Graphical summaries

- **Bar plots**: like in univariate data, bar plots are useful for displaying categorical data. For bivariate bar plots, the second variable can either be stacked or side by side.
- **Mosaic plots**: an extension of bar plots that use the area to display counts in a two-dimensional array. They are not suited for a large number of variables as they quickly grow chaotic.

### Association in categorical data

- **Correlation**: correlation is defined for numerical data, not categorical data, but many of its concepts can be translated into categorical data provided that the categories are ordinal, that is, they can be ranked.
- **Kendal $\tau$:** is a statistic used to calculate the association degree of categorical data

    $$\tau = \frac{\text{No. of concordant pairs} - \text{No. of discordant pairs}}{n(n-1)/2}$$

    A pair of observations are concordant if both ranks are in agreement, that is, the difference between both variables has the same sign.

- **Chi-squared**: related to the residuals of a table

    $$\chi^2 = \sum\frac{(f_o - f_e)^2}{f_e}$$

    where $f_o$ is the observed value and $f_e$ is the expected value, calculated as the product of the proportion of the two marginal values for a cell and the total count of observations.

- **The deviance**: also known as $G^2$, is related to chi-squared in that it requires similar inputs.

    $$G^2 = 2 \sum \left( f_o \ln \left(\frac{f_o}{f_e} \right) \right)$$

- **Odds ratio**: used in 2-by-2 tables, the odds ratio is, as the name suggests, the ratio of the odds of the variables.