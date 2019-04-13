
### https://en.wikiversity.org/wiki/Data_Analysis_using_the_SAS_Language/Examples


- Anova - dependency of categorical variable on continuous variable
- correlation - dependency of continuous variable on continuous variable
- t- test - to check if the null hypothesiszed parameter is valid or is significant or not compared to the sample meean calculated.

# Things to notice in scatter plots:
 - check for non linear relationships. these cannot be identified by the correlation coeffiecint
 - check for outliers in any variable that might be effecting the coefficient. 
  - if there is an outlier. Find out why it is there whether it is correct or not.
  - if its authentication cannot be validated then we do 2 analysis one with it and one without it to get to know the scenarios

## For correlation coeffient to be significant p-value should be less than 0.05

## Generate correlation coeffiecints between variables to determine teh multicollienearity.



# Analyze a new dataset

## Step 1: Identify exploratory and response variables
 - exploratory = X variables 
 - response = Y variables(to be predicted)
## Step 2: Define the exploratory variables
 - identify whether they are categorical or continuous
## Step 3: Define the response variables
 - identify whether they are categorical or continuous
## Step 4: Form hypothess
 - how does each response variable change with each exploratory variable
 - null hypotheses H0: the exploratorry variable has no effect/change on the response variable
 - alternative hypothese Ha: the exploratory variable has some effect on the response variable
 - p-value is estimate of the probability that any effect of the exploratory variable on the response variable could have  occured by chance, if H0 were true.
  - that is it is the probabilty for the cahnge seen in the response variable due to exploratory variable be a coincidence
  - the lower the p-value the higher the relation between the 2 higher probablity to reject the null hypotheses.


# One way to do Exloratory Data Analysis

- determine the shape of the dataset
- determine the number of categorical and numerical variables
- check for missing values
- do a through a univariate analysis 
 - use difference between mean and median to hypothesize the skewness if any
 - use the difference between 75% and max value to look for outliers
 - use the mean and standard deviation to check for normal distribution as 68% of the values need to be inside. (75% value should be less than mean+std. deviation)

- find correlation coefficients for oridinal and numeric variables only!
- based on correlation coefficients 
 - determine the variables which are highly correlated(positvely and negatively).
 - determine the variables which are not correlated with the target variable.
  
- plot box plots to check for outliers

To use linear regression for modelling,its necessary to remove correlated variables to improve your model.

