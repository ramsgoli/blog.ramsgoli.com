---
title: "Hypothesis Testing With Simple Linear Regression"
description: "Understanding and using statistical measures when developing models"
date: 2019-01-07T00:46:32-08:00
draft: true
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>

## Is there a relationship between a father's height and his son's height?
These kinds of questions and more are ones that we can answer with simple statistical tests.
<img src="/img/simple-regression/plot.png" />
<br />
The above plot contains 1082 data points, which we might call "training examples" from a learning
standpoint, plotting a male's height on the x-axis, and his son's height on the y-axis. To the
naked eye, it appears that there is an upward trend in the data; taller fathers tend to have
taller sons, and vice versa.

## Linear Regression
Using linear regression, we can come up with a simple linear model that can fit this data,
and possibly allow us to make predictions about a son's height, given the father's height. Such a model would
have the following form:
$$Y = \beta_0 + \beta_1X + \epsilon$$
where $\beta_1$ is the slope of our model, $\beta_0$ is the intercept, $\epsilon$ is a random
variable which describes the noise in our model, $Y$ is the target value, and $X$ is a predictor value.
<br/>
Note that we only use one explanatory variable in this model, $X$, which represents the height
of the father, in calculating our target value, the height of the son.
<hr/>
Using **numpy**, we can calculate _estimations_ for $\beta_0$ and $\beta_1$, and then come up with
the following model:
$$\hat{Y} = \hat{\beta_0} + \hat{\beta_1}X$$
where $\hat{\beta_0}$ and $\hat{\beta_1}$ are our estimations for the true parameters of the model
$\beta_0$ and $\beta_1$, respectively.

```python
import numpy as np

poly = np.polyfit(x, y, deg=1, full=True)
b1, b0 = poly[0]
```

This is great. We now have estimated parameters for our model.

## Hypothesis Testing
Because our model only contains one explanatory variable, it is easy to plot the data and use our
eyes to guage whether there is a relationship between X and Y, i.e. $\beta_1 \neq 0$. In the real world,
we might have tens or hundreds of explanatory variables in our dataset, and we might want to
examine the dataset for relationships between the variables, before
trying to fit large, unecessay models. We can come up with two hypotheses for our height
dataset:

* Null hypothesis: There is no relationship between a father's height and a son's height
* Alternate hypothesis: There is a relationship between a father's height and a son's height

In order to reject or accept our null hypothesis, we can perform a `t-test`.

```python
n = len(x)
xBar = np.mean(x)
se = np.sqrt((residuals / (n-2)) / np.sum([np.square(xi-xBar) for xi in x]))
tTest = b1 / se
```

In our dataset, the t-statistic comes to **0.20012**. This corresponds to a p-value of
< .05. We can then reject our null hypothesis, and say that there is a relationship between
a father's height and a son's height.

## Evaluating $R^2$
The $R^2$ is a statistical measure used in regression analysis to conclude how close the real
data fits to our computed regression line. It is a ratio which tells the the _fraction of variance
explained_.

We define the _residual sum of squares_, or $SS\_{res}$, as follows
$$SS\_{res} = \sum_i^n (y_i - \hat{y_i})^2$$

We define the _total sum of squares_, or $SS\_{tot}$, as follows
$$SS\_{tot} = \sum_i^n (y_i - \bar{y})^2$$

Finally, we will define $R^2$, our _coefficient of determination_ as follows
$$R^2 = 1 - \frac{SS\_{res}}{SS\_{tot}}$$

```python
residuals = poly[1][0]
yBar = np.mean(y)
tss = np.sum([np.square(yi-yBar) for yi in y])
rsquared = 1 - residuals/tss
```

In our dataset, $R^2$ comes out to **0.2511**, which means 25% of the variability in
son's height can be explained by their father's height, while the remaining 75% of
the variability is due to random noise.
