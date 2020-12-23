# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
This is the Bank Marketing dataset related with direct marketing campaings. It contains information about the age, job, 
marital status, education, housing etc. of individuals. We seek to predict whether the client subscribed to a term deposit.

The best performing model was the VotingEnsemble model created by Azure AutoML, whose accuracy was 0,917.

## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**
The pipeline consisted of several steps. First the workspace and experiment were created and logging started. Then a previously created CPU-cluster 
was utilised, as the jypyter notebook could not be run without a cluster. Then details about the run were set, such as parameter sampling, policy and estimator
as well as the hyperdrive config calling train.py
In the train.py the first step was obtaining the data from the given url and made into a TabularDataset with
TabularDatasetFactory. 
The the data was cleaned. In the cleaning, text variables were transformed into numerical ones so the computer
could work with them more easily. In the categorical variables the answers, such as yes and no, were turned into 1 and 0 and so forth. The
output label was separated to a separate object.
This clean dataset was split into train and test-data.
in the main function, arguments were added to aid in the working of the model containing maximum iteration and regulation strength.
Model was chosen to be Logistic Regression. The model was fit to the data and then it's accuracy was evaluated. These actions were also logged and a place
was created for the model.
Returning to the jupyter notebook, the experiment was submitted, best hyperparameters chosen and model saved.

**What are the benefits of the parameter sampler you chose?**
I chose the RandomParameterSampler due to the test previously run on the course comparing random search and grid search. Random search performed as well as
grid search, but was faster and less demanding computationally. For the regularisation strength I chose a uniform search from 0.0 to 1. For the max iterations I chose a choise of 50, 100, 150, 200 and 250. The best model had the regularisation strength of approximately 0,8168 and the maximum iterations of 50.

**What are the benefits of the early stopping policy you chose?**
I chose the BanditPolicy with slack_factor 0.1. This allows runs to be terminated if the ratio used to calculate the allowed distance from the
best performing experiment run is over 0.1.The benefit of this is that no unnecessary time is spent on runs with clearly inferior results.
I used the evaluation_interval of 1, which meant that each time the training script logs the primary metric, the bandit policy is applied. I set the 
delay_evaluation of 5 to avoid prematurely terminating training runs. The bandit policy all in all aims at lessening unnecessary computation. 

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**
The AutoML tried many algorithms including RandomForest, LightGBM and XGBoost as well as StackEnsemble, but VotingEnsemble 
achieved the highest accuracy for this data. This included many individual models giving their individual ideas on the label and then the majority vote wins. The hyperparameters were chosen by the AutoML. 

## Pipeline comparison
Both achieved an accuracy of over 0,9, which is very good. The AutoML model achieved a higher accuracy of 0,917 while the logistic regression achieved the accuracy 
of 0,9096. The architecture of the models was very different as the hyperdrive-run had a single algorithm, whose parameters were selected by the hyperdrive. The 
AutoML had several trees voting on the label and then the most common label was selected. This often creates a higher accuracy than a single model by itself.

## Future work
Future improvements could be achieved by gathering more data. Also adding domain knowledge would help with the selection and improvement of the feature engineering.
This domain knowledge could improve the results. Also the selection of a business question and the implementation of this solution would require more work. Also, the code could use a cleanup, but I ran out of time.

## Proof of cluster clean up
![]('Proof_of_cleanup.png')
