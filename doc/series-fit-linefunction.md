# series_fit_line()

Applies linear regression on a series, returning multiple columns.  

Takes an expression containing dynamic numerical array as input and performs [linear regression](https://en.wikipedia.org/wiki/Line_fitting) in order to find the line that best fits it. This function should be used on time series arrays, fitting the output of make-series operator. It generates the following columns:
* `rsquare`: [r-square](https://en.wikipedia.org/wiki/Coefficient_of_determination) is a standard measure of the fit quality. It's a number in the range [0-1], where 1 - is the best possible fit, and 0 means the data is totally unordered and do not fit any line 
* `slope`: slope of the approximated line (this is a from y=ax+b)
* `variance`: variance of the input data
* `rvariance`: residual variance which is the variance between the input data values the approximated ones.
* `interception`: interception of the approximated line (this is b from y=ax+b)
* `line_fit`: numerical array holding a series of values of the best fitted line. The series length is equal to the length of the input array. It is mainly used for charting.

**Syntax**

`series_fit_line(`*x*`)`

**Arguments**

* *x*: Dynamic array of numeric values.

> [!TIP]
> The most convenient way of using this function is applying it to the results of [make-series](make-seriesoperator.md) operator.

**Examples**

<!-- csl: https://help.kusto.windows.net:443/Samples -->
```
print id=' ', x=range(bin(now(), 1h)-11h, bin(now(), 1h), 1h), y=dynamic([2,5,6,8,11,15,17,18,25,26,30,30])
| extend (RSquare,Slope,Variance,RVariance,Interception,LineFit)=series_fit_line(y)
| render timechart
```

![alt text](./Images/samples/series-fit-line.png "series-fit-line")

| RSquare | Slope | Variance | RVariance | Interception | LineFit                                                                                     |
|---------|-------|----------|-----------|--------------|---------------------------------------------------------------------------------------------|
| 0.982   | 2.730 | 98.628   | 1.686     | -1.666       | 1.064, 3.7945, 6.526, 9.256, 11.987, 14.718, 17.449, 20.180, 22.910, 25.641, 28.371, 31.102 |
