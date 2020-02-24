
## Simple Linear Regression 2 - Predicting Yacht Hydrodynamics

Having briefly gone through the theory in part 1, let's demonstrate how simple linear regression can be used in practice.

<br>
<hr>
<br>

### Data Description

The dataset that we will be using comes from http://archive.ics.uci.edu/ml/datasets/yacht+hydrodynamics. And is contributed by Dr Roberto Lopez, Ship Hydromechanics Laboratory, Maritime and Transport Technology Department, Technical University of Delft.

Description of the variables in the dataset taken from the link above:

* Longitudinal position of the center of buoyancy, adimensional.
* Prismatic coefficient, adimensional.
* Length-displacement ratio, adimensional. 
* Beam-draught ratio, adimensional. 
* Length-beam ratio, adimensional. 
* Froude number, adimensional. 
* Residuary resistance per unit weight of displacement, adimensional.

Here, we are trying to predict Y = "Residuary resistance per unit weight of displacement" from the rest of the variables.

<br>
<hr>
<br>

#### Formatting The Data

The data comes in a text file "yacht.txt", which is space separated inconsistently, and is also missing column headers. Let's add a header row, clean up the excess whitespace, and turn it into csv format.

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

<br>
<hr>
<br>

#### Preliminary Exploration

Let's take a look at the scatter plots.



