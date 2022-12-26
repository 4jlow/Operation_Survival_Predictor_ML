# Operation_Survival_Predictor_ML

Generated a predictive survival model for surgical healthcare data obtained from kaggle. 

## The Data 

With a total of 180 variables and 100k columns, this dataset had an expansive amount of potential variables that might have been useful for predicting survival rate.
Much of the data was related to lab tests, and there were many missing values which had to be accounted for.

## The Cleaning

Alot of data was missing for variables related to lab tests, however I wanted to preserve as much data as possible, and imputed all columns with their median values, with the exception of categorical columns which were exluded. ~750 total rows were excluded. This might seem like an odd choice, since imputations of large proportions of data would have them settle near the median, however I had to choose between accuracy, and best practices. While in a professional setting, I would have excluded the columns, I wanted to have a result that would have provided as much accuracy as possible. The imputation could have led to some undesirable effects, which would be detailed below.
## Modeling Choices

I produced a full logistic regression model using all the variables and used a train/test split to evaluate the accuracy of the model. Afterwards, I used
LASSO in order to reduce the total number of variables in the model, while trying to preserve accuracy as much as possible.
While I initially wanted to do a stepwise regression, there were 2 major reasons why I did not. 1 was that stepwise regressions would remove variables if they
are not significant to the model, however model significance does not mean that they don't have an effect on the actual outcome. Hence, why it isn't popular
with the stats community. The second and probably more important reason is that the stepwise regression function takes way too long to execute with so many
variables in the dataset, that I got tired of waiting and ended up looking for alternatives.

## Model Results

The full model had an accuracy of 92.6% with a 70/30 split. It also had a Kappa of 0.3 which suggests there was only a fair agreement with the model and the test data.
However, after using LASSO, I utilized lambda 1se which gives the most regularized model such that the cross-validated error is within one standard error of the minimum lambda.
This option is typically the most recommended for LASSO models and it removed far more variables than using the minimum lambda which only removed about 5 variables.
Since my goal was to reduce the complexity of the model, 1se provided the best results and removed 43 variables in total.  

The model also allowed for the identification of the factors that had the most significant effect on survival. Many of the variables had to do with respiration, and 
the makeup of gases in the body, with the exception of arterial oxygen measured within the first hour. This suggests that surgical death is either dependant or the result
of the patient's ability to get oxygen into their body. While the most common conclusion of not breathing = death seems basic and asinine, it could also merit further investigations
into practices such as the use of anesthesia and ventilators. Anesthesia has a known side effect of reducing breathing, which could lead to asphyxiation in the patient. 
Asphyxiation would then lead to cardiac arrest and inevitably death.  

Other findings include age having a larger correlation with death probability, while electing to have a surgery is correlated with a smaller death probability. This makes
sense in that people who elect to have surgeries are probably not the same as those who have life threatening surgeries. Time to get access to the surgical room is also
correlated with increased death probabilities, which all seem logical.  

As for types of surgery, the only surgery deemed relevant in the LASSO regression was NEUROICU, suggesting that this factor had a significant difference in death probability 
compared to the other factors.  

Interestingly, bmi, weight, and presence of diabetes mellitus had a negative correlation with death probability. This is not to suggest that these are protective factors,
but that surgery associated with an increase of these factors is non-lethal in nature. Further exploration may be warranted if interested. 

As for variables like immunosuppressed status, leukemia (which is known to be associated with the prior), solid tumor during metastasis are all variables
that are correlated with higher death probability, which in this situation makes sense as they are known risk factors. Also, some internal measurements like
bilirubin concentration are also associated with immunosuppressed status as a high bilirubin concentration means that cell waste is not being disposed quickly, which
could indicate some issues with the immune system.  

Overall, the LASSO model is pretty interesting and succeeded in reducing the overall complexity. It also had an accuracy of 92.5% and a kappa of 0.226. The accuracy
has barely changed even with the removal of 43 variables, however the kappa has been reduced immensely, suggesting only a slight agreement with the model and test data.  
However, the initial kappa of 0.3 in the original model could have been boosted by all the imputations done to remove the NAs from the data. So its difficult to tell 
just how much of an effect the imputations had. I was also unwilling to exclude missing data, as that would have yielded ~1000 rows in total, which would have been
insufficient to train a decent model due to the sheer number of variables in the dataset.

There were also variables related to the apache scoring and diagnosis in the dataset, however, the results didn't match the standard range of APACHE scoring, 
thus almost all apache variables were excluded, barring some that could be compared with the model predictions.
Based on APACHE, the LASSO model yielded a weak-slight positive correlation.

