
## Simple Linear Regression - Part 2

### Predicting Yacht Hydrodynamics

Having briefly gone through the theory in part 1, let's demonstrate how simple linear regression can be used in practice. 

#### Data Description

The dataset we are using comes from: http://archive.ics.uci.edu/ml/datasets/yacht+hydrodynamics. And is contributed by Dr Roberto Lopez, Ship Hydromechanics Laboratory, Maritime and Transport Technology Department, Technical University of Delft.

Description of the variables in the dataset taken from the link above:

* Longitudinal position of the center of buoyancy, adimensional.
* Prismatic coefficient, adimensional.
* Length-displacement ratio, adimensional. 
* Beam-draught ratio, adimensional. 
* Length-beam ratio, adimensional. 
* Froude number, adimensional. 
* Residuary resistance per unit weight of displacement, adimensional.

Here, we are trying to predict Y = "Residuary resistance per unit weight of displacement" from the rest of the variables.

#### Formatting The Data

#### Preliminary Exploration

Let's take a look at the scatter plots.



