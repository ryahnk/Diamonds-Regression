# Project Description 

### In this project I practiced linear regression techniques in R using Kaggle's Diamond Prices data set
### Techniques Demonstrated: Multicollinearity (VIF & Condition Index), Stepwise Elimination (AIC Forward, Backward, Bidirectional), Cross Validation, Correlation Heatmap & Pairs Plot

# Fitting the Linear Regression Model

The linear regression model is fit without variables x, y, and z because they are highly correlated with carat. Since carat represents the weight of a diamond,
it naturally follows that the larger a particular diamond is, and thus the larger the x, y, and z dimensions of a diamond is, the greater weight (carat) it will have. 

<img width="750" alt="image" src="https://github.com/user-attachments/assets/122727cf-6d67-4279-b7ca-b6d1ee4e3fc6" />

# Checking for Multicollinearity

Now that the linear regression modes is fit, we further explore how well our model fits. We will check Variance Inflation Factor values (VIF) of each coefficient.
Then we will confirm our results by checking the condition index of each coefficient.

## VIF

We will use 10 as our cutoff value. Values greater than 10 suggest multicollinearity with the corresponding coefficient.

<img width="750" alt="image" src="https://github.com/user-attachments/assets/0533b2bc-8ce3-4406-a683-6c777800cc41" />

We can see that the VIF value for cutIdeal has a value over 10, which suggests that there could exist multicollinearity. However, this variable is a dummy variable, so this is not a cause for concern. 

## Condition Index

We will use 30 as our cutoff value. Values greater than 30 suggest multicollinearity with the corresponding coefficient.

<img width="750" alt="image" src="https://github.com/user-attachments/assets/d7c098ec-4a61-4c8c-a2d9-513ddd491894" />

We see here that we have no condition index values over 30, so we do not have significant concern for multicollinearity. 

# Stepwise Elimination (AIC)

We will now attempt to improve our model through stepwise elimination. We will do AIC forward, backward, and bidirectional stepwise elemination and compare the results. We will use a k-value of 2.

## Forward

<img width="750" alt="image" src="https://github.com/user-attachments/assets/27cba740-0103-474b-aa4f-6ff75f1ffcfc" />
<img width="750" alt="image" src="https://github.com/user-attachments/assets/7f8c4566-d5c8-49d0-a1d8-c983bce90e30" />

## Backward

<img width="750" alt="image" src="https://github.com/user-attachments/assets/8382d86a-d354-430d-9d2b-3bf69a669cd9" />
<img width="750" alt="image" src="https://github.com/user-attachments/assets/37fc16e2-990b-4f62-b600-0475a7aa84dd" />

## Bidirectional

<img width="750" alt="image" src="https://github.com/user-attachments/assets/a7d40f20-566f-4e17-ad48-9d2f595eae2f" />

Performing bidirectional stepwise elimination returns the same result as backwards stepwise elimination. 
This suggests that the best model includes only predictors cut, carat, and color. Forward stepwise elimination returns the best model to include only predictors cut, carat, color, and table. 

The inclusion of the table predictor in this model could be due to forward stepwises inability to remove predictors after adding them. 
If when table was added to the model, it improved the fit then after other predictors are added it weakened the fit, it cannot remove table. 

Backward and bidirectional stepwise elimination are more computationally taxing then forward stepwise elimination, but they generally return better results.

The model returned from backwards and bidirectional stepwise elimination appears to be slightly better than our original diamonds_model because the Adjusted R-squared value is slightly higher, 
with a value of 0.837 rather than 0.8367. This is a minor improvement, but an improvement nonetheless. 

# Cross Validation

Now that stepwise elimination has returned an a new linear model we will use cross validation to confirm the improvement new model by comparing the MSE of our original model with our new model. 
We will do this by building a cross validation function

<img width="750" alt="image" src="https://github.com/user-attachments/assets/adce94de-eb13-454d-8216-57afd0591a26" />

## Original Model

<img width="750" alt="image" src="https://github.com/user-attachments/assets/6e1f0786-36dd-4cbc-9ab2-a5b807863c8a" />

## New Model

<img width="750" alt="image" src="https://github.com/user-attachments/assets/ecdd72f0-d63f-4b6e-8be3-5ac08a216e0f" />

Cross validation is giving us an interesting result. We can see that MSE for our simplified model is higher than the MSE for our original model. 
The results of cross validation pose need for a look into the correlation between depth and price and tableand price. For this we can use a correlation plot.

# Correlation Heatmap & Pairs Plot

<img width="750" alt="image" src="https://github.com/user-attachments/assets/c451a4fa-a8ec-4664-8867-1a6be4bee0b8" />

<img width="750" alt="image" src="https://github.com/user-attachments/assets/137818ea-aa48-495a-9815-46eff06024b9" />
<img width="750" alt="image" src="https://github.com/user-attachments/assets/4fad0af4-67dd-4c43-b224-b3cd20a5e147" />

It appears that neither table, depth, or price are significantly correlated. It seems strange that MSE increases when they are removed from the model. 

# Conclusion

Stepwise elimination returns a reduced linear model with an improved adjusted $R^2$ value. However when analyzing the MSE of this reduced model, we notice that the original model has a lower MSE. 
This suggests that the original model better represents the data, a conclusion that conflicts with the results of stepwise elimination. 
It's possible that this strange result of cross validation is due to a sampling error. The seed used could be responsible for these results. 

However, after trying multiple seeds, the result is the same. The original MSE is lower than the reduced model. 
If anyone has other ideas to explain the lower MSE with the original model, I would love to explore it further. 

It is still possible this is a sampling error, since there is not significant correlation between price, table, and depth. 
For now we will assume there is a sampling error and conclude that a model with only the predictors cut, carat, and color is better due to a larger adjusted $R^2$ value.





