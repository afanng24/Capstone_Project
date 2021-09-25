# Covid Analysis Capstone

### Resources
**Raw Data Source** : https://data.cdc.gov/Case-Surveillance/COVID-19-Case-Surveillance-Public-Use-Data-with-Ge/n8mc-b4w4/data, https://data.cdc.gov/resource/n8mc-b4w4.json, https://data.cdc.gov/Case-Surveillance/COVID-19-Case-Surveillance-Public-Use-Data-with-Ge/n8mc-b4w4/data, https://rafifcoviddata.s3.amazonaws.com/COVID-19_Case_Surveillance_Public_Use_Data_with_Geography.csv

**Software** : Python, Pandas, Jupyter Notebook, PySpark, Amazon Web Services RDS, pgAdmin (SQL)

Json Data info: 19 columns, 28,652,764 rows. 

**Prepped CSV File**: https://rafifcoviddata.s3.amazonaws.com/cleaned_covid_data.csv
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/rafif-csvfile/resources/cleaned_data_sample.png)

**Google Slides Link:** https://docs.google.com/presentation/d/16uZDT7L-L3IMNzPAzESrh303vs9Um8ul3L3JFFOzpnk/edit?usp=sharing

## Data Overview

### Data Topic
Our team has chosen to analyze covid data from the CDC. We selected this data due to its relevancy as well as availability. The data has approximately 26 million rows, with each row being a unique individual that has been tested for covid. It includes the individuals age, race, ethnicity, hospitalization status, state, etc. We'd like to analyze and figure out which factor in the data contributes the most to an individual being hospitalized. After our data exploration, the team decided to decrease the size of the database and only analyze covid cases in the state of Virginia. 


### Data Exploration/Analysis


For the data exploration and analysis phase of our project, we first examined the null values in our data set. Then, we determined that even after getting rid of rows with null values, our dataset was still too large at ~7M rows to perform our machine learning models. From there, we decided to focus specifically on covid hopsitalizations in the state of Virginia. After filtering to only include Virginia data, we made pie charts for each of our factors to ensure that there was still a good distribution of data across all of the different factors. An example of a pie chart we created can be found below. 
#### Age Group Pie Chart
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Age%20Group%20Pie%20Chart.png)


## Machine Learning Models


### On Machine Learning

The data was in JSON format directly from the CDC website and we loaded the raw data into an AWS RDS which allowed us to make a preliminary dataset using postgres in pgAdmin. The data was then loaded into a csv file using an S3 bucket which can then be loaded into pyspark for cleaning. The data was cleaned to drop null values as well as columns that weren’t essential. The cleaned table data included variables from every state in the United States which we could draw from if we wished to split off into different states. The decision was made to split the dataset further into individual states for a more efficient machine learning process, so the sample state of Virginia was selected. The data was split with a target of “hosp_yn” which was the column that detailed if the patient went to the hospital. A dummy dataset was created without  “hosp_yn” which allowed us to split the data into training and testing sets. The models we decided to use were: 

- Support Vector Machine
  - The SVM has a balanced accuracy score of 93.14%
- BalancedRandomForestClassifier
  - The BalancedRandomForestClassifier has a balanced accuracy score of 78%
- EasyEnsembleClassifier
  -   The Easy Ensemble AdaBoost Classifier has a balanced accuracy score of 75%
- LogisticRegression
  -   The LogisticRegression has a balanced accuracy score of 75%
- DecisionTreeClassifier
  -   The DecisionTreeClassifier has a balanced accuracy score of 93%
- GradientBoostingClassifier
  -   The GradientBoostingClassifier has a balanced accuracy score of 93%
- RandomOverSampler
  -   The RandomOverSampler has a balanced accuracy score of just over 75% 
- SMOTE
  -   The SMOTE Oversampling has a balanced accuracy score of 75% 

### Machine Learning Dashboard
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Machine%20Learning%20Dashboard.png)

#### Summary
Overall the DecisionTreeClassifier has a slightly higher balanced accuracy score of 93.14% over the GradientBoostingClassifier of 93.07%. DecisionTreeClassifier is both simple to understand and is perfect for the type of data in this given dataset as it is essentially numerical data. GradientBoostingClassifier is not as intelligible as the decision tree, however, they both have similar accuracy ratings which surpass most other machine learning methods. The one that had the highest percentage balanced accuracy score was Support Vector Machine at 93.14%. These three models had the highest balanced accuracy score, however, they are inaccurate because they don’t have many true or false negatives on the confusion matrix. The Support Vector Machine for example has zero true or false negatives, which means it’s taking the binary form of yes and no answers on “hosp_yn” and making the true positive “no” and false positive “yes” in conclusion ending with a very low precision score. 

The Balanced Random Forest model has a lower accuracy score in comparison to SVM, Decision Trees, or Gradient Boosting but it has a higher one compared to the other models and unlike the high accuracy score models it has a significantly higher precision and f1 score, which means there is a goob balanced between true positive, false positive, true negative, and false negative. The Balanced Random Forest Model then led us to the conclusion that the column with the most correlation on hospitalization rates was the age group, and the age group that had the most yes as opposed to no was in the 65+ range. 


![balancedrandommodel](https://user-images.githubusercontent.com/82983000/134440482-003d4cde-2a84-4843-94f9-3deeffefff7e.png)




  
The diagram for the machine learning model is below: 
  ![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Machine%20Learning%20Model%20Diagram%20(1).jpg)
  
 ## Database Information

### AWS DB
We are using a postgresql on AWS; to access the db use the following:

AWS URL: database-1.cvyxzrizmaqm.us-east-1.rds.amazonaws.com

Port: 5432

DB name: postgres

Schema: Public

Username: [ask rafif]

Password: [ask rafif]

### ERD 
Below is the ERD for our database: 

![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/ERD.png)


### Cleaned Data Export
We wrote our full cleaned df including all U.S. states data from pyspark to our RDS, resulting in a "cleaned_covid_data" table in our RDS. 

![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Cleaned%20Covid%20Data%20Table.png)

### Join
We wrote our virginia df from pyspark to our RDS, resulting in a "virginia_covid_data" table in our RDS. We did the same thing for our VA county population dataset, resulting in a "county_population" table in our RDS. From there, we used SQLAlchemy to join the two tables together. 


#### Joined Table
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Joined%20Table.png)

## Dashboard

The dashboards created so far used Tableau public as the visualization tool. The interactive elements include real time filters to slice and update the visualizations to show covid data based on sex, race, ethnicity, age group, etc. Draft storyboard for full dashboard can be found on our google slides here: https://docs.google.com/presentation/d/16uZDT7L-L3IMNzPAzESrh303vs9Um8ul3L3JFFOzpnk/edit?usp=sharing

Link to tableau public: https://public.tableau.com/app/profile/ava.wolfe/viz/VACovidHospitalizationAnalysis/CovidMachineLearning?publish=yes

### VA Covid Hospitalizations Dashboard 
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/Hospitalizations%20Dashboard.png)

### Covid by VA County Per Capita Dashboard
![alt text](https://github.com/RafifAlzayat/thecoolteam-/blob/main/Covid%20Analysis%20Images/County%20Map.png)


