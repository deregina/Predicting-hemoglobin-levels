# Predicting-hemoglobin-levels

## Introduction 
### Background
> Hemoglobin (Hb) is the protein contained in red blood cells that is responsible for delivery of oxygen to the tissues. To ensure adequate tissue oxygenation, a sufficient hemoglobin level must be maintained. The amount of hemoglobin in whole blood is expressed in grams per deciliter (g/dl).  
  
> When the hemoglobin level is low, the patient has anemia. 
Anemia is a condition that develops when your blood produces a lower-than-normal amount of healthy red blood cells. If you have anemia, your body does not get enough oxygen-rich blood. The lack of oxygen can make you feel tired or weak. You may also have shortness of breath, dizziness, headaches, or an irregular heartbeat. 

### Goal
> Given the potential severe health implications which can be casued by  low hemoglobin levels, individuals at a higher risk for anemia should be aware of it and seek medical advice. The objective of this research is to identify physical characteristics that may serve as predictors of hemoglobin levels. Predictions can suggest individuals at a higher risk for anemia take proactive measures to manage their hemoglobin level before severe illnesses arise.

### Objectives (research hypotheses)
> Hypothesis 1  
**Older males have a higher possibility of low hemoglobin levels.**
> Hypothesis 2  
  **People who smoke and drink have a higher possibility of low hemoglobin levels.**

### Data Description 
> Data source
**[Kaggel: Smoking and Drinking Dataset with body signal]** https://www.kaggle.com/datasets/sooyoungher/smoking-drinking-dataset/data  
This dataset is collected from National Health Insurance Service in Korea.  

#### Column description
- Sex (Nominal Categorical) : male, female
- age (Discrete Quantitative) : round up to 5 years
- SMK_stat_type_cd (Categorical) : Smoking state, 1(never), 2(used to smoke but quit), 3(still smoke)
- DRK_YN (Categorical) : Drinker(Y) or Not(N)
- hemoglobin (Continuous Quantitative) : hemoglobin level (g/dL)

## Data Preprocessing:
- Missing Values: No nan value  
- Outliers: The rows with the hemoglobin level 1.0 have been considered as outliers and removed. 
Because the values in other columns were reasonable or within defined ranges (age 20 ~ 85, DRK_YN Y or N, SMK_stat_type_cd 1 ~ 3), 
no further process to find and remove outliers was proceeded.    
- Duplicate rows: There were 26 duplicate rows out of 991346 rows. All duplicate rows have been removed. 
Because the number of duplicate rows is very small considering the size of dataset, 
it was expected that removing all duplicate rows would not have a negative impact on model predictions.  
- Transform variables: Two clumns with object datatype (sex, DRK_YN) have been transfored to numerical datatype.  

## EDA
- Correlation
Correlations between column sex and SMK_stat_type_cd, hemoglobin are over 0.5. We can infer that the features which have high correlations affect to each other.
- Visualization (conducted only with a part of the data, because the whole dataset was too big to be visualized.)
    - The hemoglobin distribution shows clearer peaks when split into men and women.
    - Based on scatterplot with hemoglobin and sex, we can see that hemoglobin of male is generally higher than female.
    - Despite rounding, the age column also shows a distribution with the peak located in the middle.
    - Based on scatterplot with hemoglobin and DRK_YN or SMK_stat_type_cd, we cannot clearly see the relationship between hemoglobin and drking status or smoking status. 
This implies that hypothesis 2 may not be true.

## Methodology 
- Using Pearson's Chi-Square Test, check the feasibility of the two hypotheses.
    - Each features (sex, age, smoking status, and drinking status) have significant association with hemoglobin, based on the p_value which is really close to 0.
- Using various regression models (Linear regression, Ridge, Lasso, Elastic Net, Quantile regression), check whether or not the hemoglobin can be predicted using other features (age and sex, or drinking status/smoking status)
    - Ridge and Lasso were used because regularization can be helpful for more accurate prediction.
    - ElasticNetCV was used to find the best hyperparameters.
    - Quantile regression was used because the independant variable and dependant variable can have different linear line depending on the quantile.
 
## Model Evaluation and Selection 
- Linear Regression  
If there are linear relationships between dependent variable(hemoglobin) and independent variables, the linear regression model can predict hemoglobin quite well.  
- Ridge and Lasso  
If there is overfitting in a simple linear regression model, ridge and lasso can reduce the ovefitting by each regularization method. 
But this was not expected to perform better than the linear regression model, because there are only 4 columns, which is comparably small number, 
it is predicted that a simple linear regression model will not overfit the train set.
- Elastic Net  
Since the elastic net is a model that combines Ridge and Lasso models, it is expected that Elastic net also will not be the best model 
if the ridge and lasso are not performing better than simple linear regression. 
