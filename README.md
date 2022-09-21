# Statistics

## Structured Data

**Numeric**
Data that are expressed on a numeric scale.

1) **Continuous**: Data that can take on any value in an interval. (Synonyms: interval, float, numeric)

2) **Discrete**: Data that can take on only integer values, such as counts. (Synonyms: integer, count)

**Categorical**
Data that can take on only a specific set of values representing a set of possible categories. (Synonyms: enums, enumerated, factors, nominal)

1) **Binary**: A special case of categorical data with just two categories of values, e.g., 0/1, true/false. (Synonyms: dichotomous, logical, indicator, boolean)

2) **Ordinal**: Categorical data that has an explicit ordering. (Synonym: ordered factor)

There's a reason for this classification. The befits are:
- Knowing that data is categorical can act as a signal telling software how statistical procedures, such as producing a chart or fitting a model, should behave. In particular, ordinal data can be represented as an ordered.factor in R, preserving a user-specified ordering in charts, tables, and models. In Python, scikit-learn supports ordinal data with the sklearn.preprocessing.OrdinalEncoder.
- Storage and indexing can be optimized (as in a relational database).
- The possible values a given categorical variable can take are enforced in the software (like an enum).


## Rectangular Data
The typical frame of reference for an analysis in data science is a rectangular data object, like a spreadsheet or database table.

Rectangular data is the general term for a two-dimensional matrix with rows indicating records (cases) and columns indicating features (variables); data frame is the specific format in R and Python.

Key terms for rectangular data:

- **Data frame**: Rectangular data (like a spreadsheet) is the basic data structure for statistical and machine learning models.

- **Feature**: A column within a table is commonly referred to as a feature. Synonyms: attribute, input, predictor, variable

- **Outcome**:Many data science projects involve predicting an outcome—often a yes/no outcome (in Table 1-1, it is “auction was competitive or not”). The features are sometimes used to predict the outcome in an experiment or a study. Synonyms: dependent variable, response, target, output

- **Records**: A row within a table is commonly referred to as a record. Synonyms: case, example, instance, observation, pattern, sample.

## Nonrectangular Data Structures

**Spatial data structures**, which are used in mapping and location analytics, are more complex and varied than rectangular data structures. In the object representation, the focus of the data is an object (e.g., a house) and its spatial coordinates. The field view, by contrast, focuses on small units of space and the value of a relevant metric (pixel brightness, for example).

**Graph (or network) data structures** are used to represent physical, social, and abstract relationships. For example, a graph of a social network, such as Facebook, may represent connections between people on the network. Distribution hubs connected by roads are an example of a physical network. Graph structures are useful for certain types of problems, such as network optimization and recommender systems.

## Estimates of Location

KEY TERMS FOR ESTIMATES OF LOCATION
- **Mean**: The sum of all values divided by the number of values. Synonym: average

- **Weighted mean**: The sum of all values times a weight divided by the sum of the weights.Synonym: weighted average

- **Median**: The value such that one-half of the data lies above and below. Synonym: 50th percentile

- **Percentile**: The value such that P percent of the data lies below. Synonym: quantile

- **Weighted median**: The value such that one-half of the sum of the weights lies above and below the sorted data.

- **Trimmed mean**: The average of all values after dropping a fixed number of extreme values. Synonym: truncated mean

- **Robust**: Not sensitive to extreme values. Synonym: resistant

- **Outlier**: A data value that is very different from most of the data. Synonym: extreme value

Computing mean, trimmed mean and median in R.

```
> state <- read.csv('state.csv')
> mean(state[['Population']])
[1] 6162876
> mean(state[['Population']], trim=0.1)
[1] 4783697
> median(state[['Population']])
[1] 4436370
```

Computing mean, trimmed mean and median in Pandas.

```
state = pd.read_csv('state.csv')
state['Population'].mean()
trim_mean(state['Population'], 0.1)
state['Population'].median()
```

If we want to compute the average murder rate for the country, we need to use a weighted mean or median to account for different populations in the states. 

Since base R doesn’t have a function for weighted median, we need to install a package such as matrixStats:

```
> weighted.mean(state[['Murder.Rate']], w=state[['Population']])
[1] 4.445834
> library('matrixStats')
> weightedMedian(state[['Murder.Rate']], w=state[['Population']])
[1] 4.4
```

Weighted mean is available with NumPy. For weighted median, we can use the specialized package wquantiles:

```
np.average(state['Murder.Rate'], weights=state['Population'])
wquantiles.median(state['Murder.Rate'], weights=state['Population'])
```

## Estimates of Variability

Key terms for Estimates of Variability:

- **Deviations**: The difference between the observed values and the estimate of location. Synonyms: errors, residuals

- **Variance**: The sum of squared deviations from the mean divided by n – 1 where n is the number of data values. Synonym: mean-squared-error

- **Standard deviation**: The square root of the variance.

- **Mean absolute deviation**: The mean of the absolute values of the deviations from the mean. Synonyms: l1-norm, Manhattan norm

- **Median absolute deviation from the median**: The median of the absolute values of the deviations from the median.

- **Range**: The difference between the largest and the smallest value in a data set.

- **Order statistics**: Metrics based on the data values sorted from smallest to biggest. Synonym: ranks

- **Percentile**: The value such that P percent of the values take on this value or less and (100–P) percent take on this value or more.Synonym: quantile

- **Interquartile range**: The difference between the 75th percentile and the 25th percentile. Synonym: IQR



### Standard Deviation and Related Estimates

The most widely used estimates of variation are based on the differences, or deviations, between the estimate of location and the observed data. For a set of data {1, 4, 4}, the mean is 3 and the median is 4. The deviations from the mean are the differences: 1 – 3 = –2, 4 – 3 = 1, 4 – 3 = 1. These deviations tell us how dispersed the data is around the central value.

The best-known estimates of variability are the variance and the standard deviation, which are based on squared deviations. The variance is an average of the squared deviations, and the standard deviation is the square root of the variance.

### Estimates Based on Percentiles

A different approach to estimating dispersion is based on looking at the spread of the sorted data. Statistics based on sorted (ranked) data are referred to as order statistics. The most basic measure is the range: the difference between the largest and smallest numbers. The minimum and maximum values themselves are useful to know and are helpful in identifying outliers, but the range is extremely sensitive to outliers and not very useful as a general measure of dispersion in the data.

A common measurement of variability is the difference between the 25th percentile and the 75th percentile, called the interquartile range (or IQR).


## Exploring the Data Distribution

- **Boxplot**: A plot introduced by Tukey as a quick way to visualize the distribution of data. Synonym: box and whiskers plot

- **Frequency table**: A tally of the count of numeric data values that fall into a set of intervals (bins).

- **Histogram**: A plot of the frequency table with the bins on the x-axis and the count (or proportion) on the y-axis. While visually similar, bar charts should not be confused with histograms. See “Exploring Binary and Categorical Data” for a discussion of the difference.

- **Density plot**: A smoothed version of the histogram, often based on a kernel density estimate.

