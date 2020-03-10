## Simple Linear Regression 2 - Predicting Yacht Hydrodynamics

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

data = pd.read_csv('yacht.csv')

import matplotlib.pyplot as plt # for visualization

for i in range(0,6):
    plt.subplot(2,3,i+1)
    plt.scatter(data.iloc[:,i], data.iloc[:,6])

plt.show()

```

<p align="center">
  <img src="https://raw.githubusercontent.com/tommyzakhoo/simple-regression2/master/scatter.png", width="600">
</p>


