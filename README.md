## Simple Linear Regression 2 - Estimating Yacht Hydrodynamics

Having briefly gone through the mathematical theory in part 1, let's demonstrate how simple linear regression can be used in practice.

Previous part: https://github.com/tommyzakhoo/simple-regression1

<hr>

### Data Description

The dataset that we will be using comes from http://archive.ics.uci.edu/ml/datasets/yacht+hydrodynamics. And is contributed by Dr Roberto Lopez, Ship Hydromechanics Laboratory, Maritime and Transport Technology Department, Technical University of Delft.

Description of the variables in the dataset taken from the link above:

* <i> Longitudinal position of the center of buoyancy, adimensional. </i>
* <i> Prismatic coefficient, adimensional. </i>
* <i> Length-displacement ratio, adimensional. </i>
* <i> Beam-draught ratio, adimensional. </i>
* <i> Length-beam ratio, adimensional. </i>
* <i> Froude number, adimensional. </i>
* <i> Residuary resistance per unit weight of displacement, adimensional. </i>

Here, we are trying to predict <b>Y</b> = <i>Residuary resistance per unit weight of displacement</i> using the rest of the variables.

<hr>

### CSV Conversion

The data comes in a text file "yacht.txt</i>, which is space separated in a inconsistent way, and is also missing headers for each column. Let's add a row of headers, clean up the excess whitespace, and turn it into csv format.

```python

f = open('yacht.txt','r') # open file
x = f.readlines() # 
f.close()

g = open('yacht.csv','w')
g.write('buoyancy,prismatic,length-displace,beam-draught,length-beam,froude,resist\n') # write header row

for index in range(0,len(x)):
  y = x[index].strip() # remove leading/trailing whitespace
  y = y.replace('  ',' '); # some rows had double spacing
  y = y.replace(' ',','); # replace single spaces with comma
  g.write(y + '\n') # added a line break after each row

g.close()

```

<hr>

### Preliminary Exploration

Let's start by looking at the scatter plots of <b>Y</b> with the rest of the variables.

```python

import pandas as pd # data structures module
import matplotlib.pyplot as plt # for visualization

data = pd.read_csv('yacht.csv')

for i in range(0,6):
    plt.subplot(2,3,i+1)
    plt.scatter(data.iloc[:,i], data.iloc[:,6])

plt.show()

```

<p align="center">
  <img src="https://raw.githubusercontent.com/tommyzakhoo/simple-regression2/master/scatter.png", width="800">
</p>

While some of these variables look like good candidates for splitting the data set into clusters, only the bottom right plot shows some potential for a simple linear regression model. The relationship shown in the scatterplot is not linear, but we can apply a logarithmic transform to <b>Y</b> variable to get a more linear looking scatterplot.

<p align="center">
  <img src="https://raw.githubusercontent.com/tommyzakhoo/simple-regression2/master/log_transform.png", width="800">
</p>

Originally, our dependent variable was <b>Y</b> = <i>Residuary resistance per unit weight of displacement</i>. Here, it has become <b>Z</b> = <b>log(Y+1)</b>, where <b>log</b> is the natural logarithm. We have added <b>1</b> because some values of <b>Y</b> were close to zero, which behaves badly under logarithmic transformation. Our independent variable here is <b>X</b> = the <i>Froude number</i>.

## Fitting A Simple Linear Regression Model

Like we mention in [part 1](https://github.com/tommyzakhoo/simple-regression1), we want to fit this linear model to our data.

<p align="middle">
  <b>y<sup>^</sup><sub>i</sub> = a<sup>^</sup> + b<sup>^</sup> x<sub>i</sub></b>
</p>

Like we mentioned in part 1 of this article, he a<sup>^</sup> and b<sup>^</sup> can be found by minimizing the sum of squares. In sklearn, this solution is computed using Singular Value Decomposition (SVD). See [the documentation](https://scikit-learn.org/stable/modules/linear_model.html#ordinary-least-squares) for more details.

```python

import pandas as pd # data structures module
import numpy as np # n-dimensional array module
from sklearn.linear_model import LinearRegression as lr

data = pd.read_csv('yacht.csv')

x = data.iloc[:,5]
x = x.reshape(-1, 1)

y = np.log(data.iloc[:,6]+1)
y = y.reshape(-1, 1)

reg = lr()
reg.fit(x, y)

```
We can plot the fitted line over a scatter plot to visualize the fit.

<p align="center">
  <img src="https://raw.githubusercontent.com/tommyzakhoo/simple-regression2/master/regression.png", width="800">
</p>

Our simple regression line does looks like a "good" estimate at first glance. However, there are mathematical tools we can use to quantify this.

## Fitted Coefficients

Let us take a look at the fitted coefficients.

```
>>> reg.coef_[0][0]
11.957987175940932

>>> reg.intercept_[0]
-1.7672239620561152

```

What this means is that for our line model <b>y<sup>^</sup><sub>i</sub> = a<sup>^</sup> + b<sup>^</sup> x<sub>i</sub></b>, the coefficients <b>a<sup>^</sup> = -1.7672</b> and <b>b<sup>^</sup> = 11.9579</b>, to four decimal places.

but since resistance cannot go below <b>0</b>, the interc

## Coefficient Of Determination

As mentioned in part 1 of this article, the coefficient of determination <b>R<sup>2</sup></b> can be interpreted as "how much of the variation in <b>Y</b> is explained by our model". For simple linear regression, <b>R<sup>2</sup></b> is actually equal to the square of the sample correlation coefficient.

```
>>> reg.score(x,y)
0.9635494120195388
```

## Residual Analysis

We can take a look at the residual for clues on how good our model is.

```
e = y - y_predicted
t = np.arange(0,len(e))

plt.scatter(t,e) # scatter plot
plt.hist(e) # histogram

```

<p align="center">
  <img src="https://raw.githubusercontent.com/tommyzakhoo/simple-regression2/master/residual.png", width="800">
</p>


## Conclusions & Physics

It turns out that the Froud number is an [important Physics concept](https://en.wikipedia.org/wiki/Froude_number) for determining resistance. This highlights a particular situation in which linear regression excels: estimating physical laws of nature.

## More Real World Application

Please proceed to part 3 of this article for another application of simple linear regression to data: https://github.com/tommyzakhoo/simple_regression3




