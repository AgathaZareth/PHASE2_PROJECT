# How Renovations Can Increase the Estimated Value of a Home Located in King County Washington. 
- - -
### Linear regression of King County House Sales dataset

- - -
- - -
## Overview
The goal of this linear regression model is to offer insight as to how renovations can increase the estimated value of a home located in King County WA. A real estate company has tasked me with evaluating what features increase home sales, and by how much. This company is interested in providing homeowners with recommendations about how to invest their renovation-dollars to boost the value of their home. They want at least three specific features that boost sale price, and they would like to know by much each feature increase the value of the home.

The regression modeling will yield findings relevant to this goal, and will include:

 - multicollinearity checks with VIF (Variance Inflation Factor) values.
 - visual modeling to show:
    - distribution of the model residuals 
    - linearity checks to show the relationship between Sale Price and features used
    - a homoscedasticity check of the final model. 
 - interpretations of the final models r squared, adjusted r squared and prob(f-statistic) to evalute the final model 
 - And finally, there are three feature-specific effects on Sale Price, stated in the [Recommendations](https://github.com/AgathaZareth/PHASE2_PROJECT/blob/main/README.md#recommendations) section. 


- - -
## The Data

This regression analysis uses the King County House Sales dataset. For more information, other than what is provided below, see the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r)


<p align="center">
<img src='README_images/column_description_tabel.png'>
    </p>


- - -
## Methods

Below is a summary of how I arrived at the final model:
### Baseline Model
- I started with a Baseline Model of the highest correlated feature: `‘sqft_living’`. And an evalutation of the distribution of residuals. 
- **NOTES:** It was obvious there was a problem with residuals. I made the choice to log transform the target variable, `'price'`

<img src='README_images/baseline_model.png'>

<img src='README_images/Baseline_dist_QQ.png'>

### Model using log transformed target:`'price_log'`
- Log transform target: `‘price’` >> `'price_log'`
- Model with `‘price_log’` and `‘sqft_living’`
 - **NOTES:** Big improvement in distribution of residuals however there is an obvious outlier problem. 

<img src='README_images/price_log_initial.png'>

### Model with outliers removed from `'price'`
 - this removed 2.0% of the data

<img src='README_images/outliers_removed_price.png'>

 - **NOTES:** log transformation of target did a better job at normalizing the distribution of the residuals but it had a couple extreme outliers. 
 - A choice needed to be made:
    - either stick with the outlier removed df and move forward with the way it was
    - log transform the outliers removed `'price'` 
    - or see what happens if I removed outliers from log transformed `'price_log'` (this resulted in only 1.0% data lost vs 2.0%)

### Model removing outliers from `'price_log'`

<img src='README_images/outliers_removed_price_log.png'>

### Additional Models and steps leading to FINAL MODEL include:
- Model with other continuous features added 
- Model with categorical features added 
- Model removing features with p-values greater than .05 
- Model removing one collinear features
- Model removing another collinear features
- Model removing features with p-values greater than .05 
- Model adding interactions. Abandoned this idea due to negative effect on distribution of residuals, and likely multicollinearity impact

## FINAL MODEL  

<img src='README_images/final_model1.png'>
<img src='README_images/final_model2.png'>
<img src='README_images/final_model3.png'>

### Assumption Checks of Final Model
- Normality: histogram of residuals and qq plot

<img src='README_images/final_normality.png'>

- Linearity: scatter plots of continuous features used in final model

<img src='README_images/final_linearity.png'>

- Homoscedasticity: scatter plot of final model residuals

<img src='README_images/final_homo.png'>

- Independence: re check to ensure no features have VIF greater than 5
    - No features with VIF above 5

- - -
- - -

## Recommendations

Renovations should focus on upgrading _Condition_ and _Grade_ of the home. In addition, adding a _floor_ will increase value. 

The top ways renovations can increase home value: 
-	**Upgrading home _CONDITION_ from average or below to 'GOOD' will increase home value by 8.3%**
-	**Improvement in _CONDITION_ from average or below to 'VERY GOOD' will increase value by 17.87%**
-	**For every increase in _GRADE_ level, the home value will increase by 14.71%**
- **For every additional _FLOOR_, home value increases by 5.89%**

Below I have placed reminders of the description of _GRADE_ and _CONDITION_ from [table](https://github.com/AgathaZareth/PHASE2_PROJECT/blob/main/README.md#the-data) at top of this notebook. As well as additional information from the King County [Residential Glossary of Terms](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r#top).

**GRADE**: _Overall grade of the house. Related to the construction and design of the house. Classification by construction quality which refers to the types of materials used and the quality of workmanship. Buildings of better quality (higher grade) cost more to build per unit of measure and command higher value. (See Glossary in Area Report for Residential Building Grades in use by the King County Department of Assessments.)_  
       
**CONDITION**: _How good the overall condition of the house is. Related to maintenance of house._ 

- - -
## Conclusions on Final Model

The p values are important, they are a measurement of how likely your coefficient is measured through our model by chance. However, they are not the only value to focus on. R-squared is the measurement of how much of the independent variable is explained by changes in our dependent variables. In our case, the *r squared* value of .744 tells us this final linear regression model is able to explain 74.4% of the variability observed in Sale Price. Because *r squared* will always increase as features are added, in this case 34 as shown in *Df Model*, we should also look to the *adjusted r squared* to get a better understanding of how the model is performing. *Adjusted r squared* takes into account the number of features in the model by penalizing the R-squared formula based on the number of variables. If the two were significantly different it could be that some variables are not contributing to your model’s R-squared properly. In this case, the *adjusted r squared* is the same as the *r squared* so we can be confident in the 76% reliability, as stated above, of this model. The f statistic is also important. More easily understood is the prob(f-statistic), it uses the F statistic to tell the accuracy of the null hypothesis, or whether it is accurate that the variables’ effect is 0. In our case, it is telling us 0.00% chance of this. I'd like to tighten up the normality distribution of residuals, suggests on how to acomplish that are below, but based on values mention, I feel confident in the recommendations.
 - - -
 
## Next Steps

Ways to possibly improve recommendations to the average homeowner (appealing to the broadest possible range of clients):
- Starting with sqft_lot, remove outliers from features, one by one, modeling between each, and try to eliminate the outlying residuals and improve normality. 

- There is a huge range in location coefficients. Intuitively this makes sense since city homes often sell at a higher price. I would like to split properties into urban and rural locations to explore this attribute to get more accurate findings based on location. That is to say, do rural home buyers want different features in their homes compared to city buyers? Does the estimated increase in value change with location?

- Building on that divide (between urban and rural), I would like to find the price per sq foot of living and explore if homes (on average) fall nicely into this assumption that rural homes are cheaper per square foot of living. Specifically looking to add a greater degree of accuracy in how much renovations will increase home value. Another way to explore a possible similar attribute of rural homes vs urban homes is comparing square foot lot to square foot living. 

- On that same note I'd like to find the number of bathrooms per square foot living, and similarly, number of bedrooms per square foot. Trying to weed out the extreme outliers of mansion type homes. What might be considered excessivly large in a city could be normal in a rural location.  

- Because only conditions 'Good' and 'Very Good' remained, I would like to make this feature binary as `'condition_above_average'` so that it is more interpretable.

- - -
## Thank you!

**Email:** cassigroesbeck@gmail.com

**GitHub:** @AgathaZareth

**LinkedIn:** linkedin.com/in/cassarra-groesbeck-a64b75229



