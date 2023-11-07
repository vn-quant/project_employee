# project_employee
Employee data for classification task

# Introduction and summary

In today's corporate environment, understanding the attributes and experiences of employees is paramount. This project analyzes a dataset in \[kaggle\](Employee dataset (kaggle.com) containing information about the company's workforce, including their educational backgrounds, demographics, and employment factors. It aims to uncover valuable insights that can inform strategies to optimize workforce management and foster a productive, inclusive, and satisfied work environment.

By delving into the dataset, this project will provide an overview of the educational qualifications, examine the variation in employees' length of service across cities, explore the correlation between payment tier and domain experience, assess the gender distribution within the workforce, and investigate patterns in leave-taking behavior. These insights can empower the company to make data-driven decisions that enhance its operations and employee well-being.

* Objective: To analyze a dataset with employee information to gain insights into workforce management.

* Analytical Goals:
    + Overview of educational qualifications of employees.
    + Analysis of service length variation across cities.
    + Exploration of payment tiers in relation to domain experience.
    + Assessment of gender distribution in the workforce.
    + Investigation of leave-taking patterns.
* Methodology:
    + Extensive data exploration and cleaning.
    + Parallel processing to expedite computations.
    + Employed a variety of machine learning models (Random forest, XGboost,Support Vector Machine and Stacking) for classification task.
* Conclusion:
    + A comprehensive analysis revealed significant correlations between educational background and job performance, underscoring the importance of aligning educational initiatives with career development programs.
    + Developed a predictive model with accuracy 84.8% 

# Explore the data

## Target variable

![image](https://github.com/vn-quant/project_employee/assets/83815398/f698f3f7-65b3-4a5f-a0b8-34e040130b62)

we can observed that an imbalance in the dataset

## Relationship between predictors and target variable

![image](https://github.com/vn-quant/project_employee/assets/83815398/42440521-856f-4687-b7be-6526048d21e3)

These features look like they exhibit some big differences in the decision to leave or stay

# Modeling

My approach involves exploring three distinct models: Random Forest (RF), XGBoost (XGB), Support Vector Machine (SVM) and Stacking method using the three models to construct a predictive model. The primary objective is to identify the influential features that impact the decision-making process of these models.
The stepwise process begins with data preparation, where i split the data into training and testing sets, and create cross-validation resamples. Following this, i proceed to model training and evaluation, individually exploring the RF, XGB, SVM, and Stacking models. Each model’s performance will be assessed using ROC AUC and accuracy.
Due to the dataset’s class imbalancendefinedndefined, the primary performance metric used for evaluation is the Receiver Operating Characteristic Area Under the Curve (ROC AUC), which offers a robust assessment of model performance. Additionally, accuracy is considered for its ease of interpretation, although it is secondary to ROC AUC in addressing class imbalance.
By following the process above , i aim to develop a predictive model that not only performs well but also provides some insights into the key features that drive the decision of the employee and suggest some solution and conclude conclusions

## Random forest

`min_n` and `m_try` tuning results

![image](https://github.com/vn-quant/project_employee/assets/83815398/2b49eda9-9e7f-4daa-91d8-0af5f910b62d)

This grid did not try every combination of min_n and mtry but we can see that lower value of mtry , from 2 to 5 is fairly good while in case of min_n , we should focus more when min_n above 10. I set ranges of the parametre again, but this time more focus in min_n .

![image](https://github.com/vn-quant/project_employee/assets/83815398/636ec683-a064-4755-8437-ec95c192ed02)

VIP result with `min_n = 32` and `m_try = 4` 

![image](https://github.com/vn-quant/project_employee/assets/83815398/b45ebcec-f5e3-4b52-adc6-b523435a59de)


## XGboost

Tuning result

![image](https://github.com/vn-quant/project_employee/assets/83815398/6aa2fef8-6979-44c5-bc5a-f7396af65834)

Choosing best paras:
  mtry = 6
  trees = 500
  min_n = 12
  tree_depth = 8
  learn_rate = 0.0372029996176391
  loss_reduction = 1.62354669613487
  sample_size = 0.982596664283192
  
SHAP plot

![image](https://github.com/vn-quant/project_employee/assets/83815398/dd032b12-130f-4a6f-9269-eac5d54a7d10)

The y-axis indicates the variable name, in order of importance from top to bottom. The value next to them is the mean SHAP value. (the higher, the more important)
On the x-axis is the SHAP value. Indicates how much is the change in log-odds. From this number we can extract the probability of the output.
Gradient color indicates the original value for that variable.
Each point represents a row from the original dataset.

In general, SHAP results are similar to VIP plot, but more detailed.

## SVM

`cost` and `margin` tuning results

![image](https://github.com/vn-quant/project_employee/assets/83815398/24aa25b6-47c4-4a1e-b203-6754f6f1e59e)

best SVM paras:
  cost = 9.08581669060231
  margin = 0.0203436492756009

## stacking

Fitting results

![image](https://github.com/vn-quant/project_employee/assets/83815398/a7bfc6cb-0f31-451b-9cdd-9f5c3c182877)
![image](https://github.com/vn-quant/project_employee/assets/83815398/b6faadb0-9fcd-4db9-94e6-4d6787ebbf3f)

type          weight
rand_forest   2.95   
rand_forest   0.873  
rand_forest   0.00182


# Results and Conclusion

## ROC AUC plot

![newplot](https://github.com/vn-quant/project_employee/assets/83815398/1b16fd3c-2316-4f73-aae1-b0bab5a7edfa)

The perfomance of the four model is quite similar. Let took a closer look to their confusion matrix and some metrics.

## Confusion matrix

![image](https://github.com/vn-quant/project_employee/assets/83815398/fc425ce9-154b-4791-8ac0-15d5254c99c3)
![image](https://github.com/vn-quant/project_employee/assets/83815398/0e401215-08f1-4784-9272-f919a3b3e108)
![image](https://github.com/vn-quant/project_employee/assets/83815398/74afdc43-7cdc-42a0-a8ba-5d09c4a36203)
![image](https://github.com/vn-quant/project_employee/assets/83815398/415892a5-3409-4dad-a414-b2334696ede3)


## Analysis
- Random Forest (RF) exhibits a balanced performance with an accuracy of 0.832 and a ROC AUC of 0.855, indicating its ability to maintain a commendable trade-off between sensitivity (0.917) and specificity (0.671).
- XGBoost (XGB) offers a performance closely aligned with RF, with a slightly lower sensitivity of 0.902 but marginally better specificity at 0.683. Its ROC AUC of 0.847 showcases its robust discriminative power.
- Support Vector Machine (SVM) mirrors a performance akin to XGB with an accuracy of 0.83. Its specificity stands at 0.688, a tad higher than RF, while maintaining a sensitivity of 0.904.
- Stacking leads in accuracy with 0.85 and exhibits an outstanding sensitivity of 0.969. However, this seems to come at the cost of specificity, which at 0.621, is the lowest among the models. Its ROC AUC of 0.852 suggests a strong overall performance but hints at its inclination towards predicting positive instances.

## Selection
- For applications prioritizing the detection of positive instances, the Stacking model emerges as the top choice due to its unparalleled sensitivity. However, its lower specificity indicates a potential for higher false positives, which is more flavor in some situation.
- For scenarios demanding a balanced outcome between true positives and true negatives, the Random Forest model stands out as the most versatile choice
- XGB and SVM serve as robust alternatives to RF, with performances that are closely matched, but their complexity make the tranning time inefficient.










































