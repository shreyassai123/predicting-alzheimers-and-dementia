## Predicting the Likelihood of Dementia in Senior Citizens and Alzheimer's Analysis

### Abstract

Alzheimer’s disease and dementia are debilitating conditions that affect millions of lives worldwide, and early detection of these disease is critical to provide effective treatment. This report presents a comprehensive review of various machine learning techniques for detecting Alzheimer’s disease and dementia. 

The report also presents a comparative analysis of different machine learning models to identify the most effective techniques in detecting these conditions. In addition to evaluating individual models, the study also ensembles the models using hard and soft voting. 
These machine learning models are trained on a pre-processed OASIS Longitudinal dataset which is a widely used and publicly available dataset of longitudinal MRI scans.

The results of this study have important implications for future research and the potential use of machine learning algorithms in clinical settings for the early detection and treatment of Alzheimer’s and dementia. The comparative analysis of different machine learning models and their effective in the detecting the said diseases can guide the selection of appropriate models for future research and clinical applications.

### Feature Selection

Iterative Feature Selection based on Gini Importance:

Get Gini Importance (i.e. Mean Decrease Impurity) for all the attributes in the dataset using Random Forest Classifier
Sort the attributes in Descending Order of Gini Importance and store it in sorted_list

```
optimal_score = 0
optimal_index = 0

for feature in sorted_list:

	feature_list.append(feature)
	updated_X_train = X_train[:,features_list]
	updated_X_test = X_test[:,features_list]

	model.fit(updated_X_train, y_train)
	y_pred = model.predict(updated_X_test)
	
	score = accuracy_score(y_test, y_pred)
	if score > optimal_score:
		optimal_score = score
		optimal_index = index(feature)
	
optimal_features = sorted_list[1:optimal_index]
```

### SES Values Imputation
`SES -> 14 missing values`

From the dataframe, we observed that `EDUC = 12 or 16` for the records which had missing SES values

Also, Pearson's correlation coefficient, `r(SES,EDUC) = -0.71`

Then, we found the count of the different values of SES when `EDUC = 12 and 16`:
```
EDUC = 12
SES	Count
2	13
3	32
4	42

EDUC = 16
SES	Count
1	36
2	14
3	25
4	3
```

Finally, we create a function to impute the missing SES values:

```
function addSES(record):
	if record.SES == NULL:
		if record.EDUC == 12:
			num = random(1, 87) // 87 = 13+32+42 i.e. it is the sum of count of the different values of SES when EDUC = 12
			if num <= 42:
				return 4
			else if num <= 74: // 74 = 42+32
				return 3
			else:
				return 2
		else: // i.e. if record.EDUC == 16
			num = random(1, 78) // 78 = 36+14+25+3
			if num <= 36:
				return 1
			else if num <= 61:
				return 3
			else if num <= 75:
				return 2
			else:
				return 4
```

### MMSE Value Imputation

`MMSE -> 2 missing values`

From the dataframe, we observed that `EDUC = 12`, `CDR = 1`, `M/F = F` for the records which had missing MMSE values

Then, we found the count of the different values of `MMSE` when `EDUC = 12, CDR = 1, M/F = F`:

```
MMSE	Count
16	2
18	1
19	1
27	2
```

Finally, we create a function to impute the missing MMSE values:

```
function addMMSE(record):
	if record.MMSE == NULL:
		num = random(1, 6) // 6 = 2+1+1+2 i.e. it is the sum of count of the different values of MMSE when EDUC = 12, CDR = 1, M/F = F
		if num <= 2:
			return 16
		else if num <= 4: // 4 = 2+2
			return 27
		else if num == 5: // 5 = 2+2+1
			return 18
		else: // i.e. if num == 6; 6 = 2+2+1+1
			return 19
```
