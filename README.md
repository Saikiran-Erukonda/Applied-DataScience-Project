# **Predicting Falcon 9 First Stage Landings: A Machine Learning Approach to Cost-Efficient Space Missions**
![95f1dabb1c2cff922fc56f985c197460](https://github.com/user-attachments/assets/8fe3edef-4021-45f7-a884-9e0c71597ce6)

**Author(s):** Erukonda Saikiran  
**Affiliation(s):** Courseera -IBM
**Contact:** [GitHub Profile](https://github.com/Saikiran-Erukonda)  
**Date:** [2025-06-12]  

-------------

 # Project Overview

In this project, I developed a complete end-to-end data science solution to predict the success or failure of SpaceX Falcon 9 first-stage landings. The goal was to enable cost-efficient forecasting for rocket launches, which can support competitive bidding decisions in the aerospace industry.
# The workflow included:
- Data Acquisition: Collected launch data via SpaceX’s API and enriched it with mission details through Wikipedia web scraping.
- Data Wrangling: Performed thorough data cleaning and preprocessing, including handling missing values, feature engineering, and encoding categorical variables.
- SQL Integration: Queried and managed structured launch data using SQL for effective exploration and analysis.
- Data Visualization:
   - Basic and advanced plots using matplotlib and seaborn for EDA.
   - Folium maps to visualize launch site locations and geospatial trends.
   - An interactive dashboard built with Plotly and Dash for real-time data inspection and user-driven insights.
- Predictive Modeling:
  - Applied multiple machine learning models, including logistic regression, decision trees, SVM, and KNN.
  - Tuned hyperparameters using GridSearchCV and compared test accuracies to identify the best-performing model.
  - Final model used to predict first-stage landing success based on input launch configurations

## Objective 
1. Can machine learning predict Falcon 9's first-stage landing success?
2. What factors influence successful landings?
3. Which model performs best for prediction?
4. Can data-driven insights help competitors optimize costs?

     
## Datasets
All the Datasets are provided in one [Dataset](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset) folder.
- [`dataset_part_1.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/dataset_part_1.csv)
- [`dataset_part_2.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/dataset_part_2.csv)
- [`dataset_part_3.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/dataset_part_3.csv)
- [`Spacex.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/Spacex.csv)
- [`spacex_launch_dash.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/spacex_launch_dash.csv)
- [`spacex_web_scraped.csv`](https://github.com/Saikiran-Erukonda/Applied-DataScience-Project/blob/main/Dataset/spacex_web_scraped.csv)

## Key insights from EDA(Exploratory Data Analysis)

#### **Flight Number vs. Launch Site**

![image](https://github.com/user-attachments/assets/fa572b1f-eedf-4b79-b618-bdc726a53c5f)
- Higher flight numbers generally show an increased success rate, especially at sites like Cape Canaveral, indicating learning and optimization over multiple launches.
- VAFB-SLC has relatively fewer launches, possibly due to its focus on specific missions, while KSC and CCAFS support frequent launches with varied success rates.
- Early flights at most sites show lower success rates, reinforcing the trend that experience leads to better outcomes in landing reliability.

#### **Payload vs. Launch Site**

![image](https://github.com/user-attachments/assets/b3d06b65-3ee3-44bd-817b-bd55bcfee96c)
+ VAFB-SLC primarily handles lighter payloads (below 10,000 kg), suggesting its missions are focused on smaller satellites or specialized low-mass payload deployments.
+ Heavy payload launches (above 10,000 kg) are concentrated at Cape Canaveral and Kennedy Space Center, which are equipped for larger missions requiring higher thrust and recovery infrastructure.
+ VAFB-SLC may be optimized for polar or sun-synchronous orbits, making it less suited for missions involving massive payloads that need different orbital characteristics.

#### **Success Rate vs. Orbit Type**
![image](https://github.com/user-attachments/assets/317af609-9bc9-44d5-a8e0-b078002f646c)
- LEO (Low Earth Orbit) has the highest success rate, making it the most reliable orbit for successful landings.
- ISS (International Space Station) and SSO (Sun-Synchronous Orbit) also show strong success rates, indicating stable performance for missions targeting these orbits.
- GTO (Geostationary Transfer Orbit) has a relatively lower success rate, suggesting challenges in achieving successful landings for this orbit type.

#### **Flight Number vs. Orbit Type**

![image](https://github.com/user-attachments/assets/6a046449-b243-421f-92ab-faf0044904dc)
+ LEO (Low Earth Orbit) shows an increasing trend in success rate with higher flight numbers, indicating that experience and iterative improvements contribute to better landing outcomes.
+ GTO (Geostationary Transfer Orbit) does not show a clear relationship between flight number and success, suggesting that other factors, like payload complexity, might play a bigger role.
+ Polar and ISS orbits have moderate success rates across different flight numbers, showing stable performance but without a distinct upward trend.

#### **Payload vs. Orbit Type**
![image](https://github.com/user-attachments/assets/3272d716-26ab-49e6-b628-2d99b9e19a32)
+ Low payloads (<5000 kg) succeed consistently in LEO, ISS, and Polar orbits, reinforcing their suitability for smaller payload missions.
+ Mid-range payloads (5000-10,000 kg) show stable performance, especially in LEO and SSO, maintaining high success rates across launches.
+ Heavy payloads (>10,000 kg) have mixed results, with LEO and ISS adapting well, while GTO struggles due to complex trajectory challenges.

#### **Launch Success Yearly Trend**
![image](https://github.com/user-attachments/assets/9bf8e9d0-5b23-4baa-9aaf-340c30f8b9f8)
- Success rates have steadily increased since 2013, reflecting improvements in technology and operational efficiency.
- A peak trend around 2020 indicates a period of high reliability, likely due to refined landing techniques and reusability optimizations.

### Q&A using SQL quering in python ***sqlite3***

1. All Launch Site Names
```python
%sql  SELECT DISTINCT Launch_site from SPACEXTABLE;
```
![image](https://github.com/user-attachments/assets/8eb92b1e-71a1-45ab-9c14-5b83387be2c3)

2. Find 5 records where Launch sites begin with 'CCA'
```python
%sql  SELECT * from SPACEXTABLE WHERE Launch_site like "CCA%" LIMIT 5;
```
![image](https://github.com/user-attachments/assets/796ee8a8-7653-4482-8b9e-57f93f39261d)

3. Calculate the total Payload carried by boosters from NASA.
``` python
%%sql SELECT SUM(PAYLOAD_MASS__KG_)  as Total_payload
FROM SPACEXTABLE
WHERE Customer = 'NASA (CRS)';
   ```
![image](https://img.shields.io/badge/Total__payload%20%3A%2045596-white?style=plastic)

4. Calculate the average payload mass carried  by booster version F9 v1.1
```python
#average payload mass carried by booster version F9 v1.1
%%sql SELECT Round(AVG(PAYLOAD_MASS__KG_),2)  as Average_payload
FROM SPACEXTABLE
WHERE Booster_Version like 'F9 v1.1%';
```
![image](https://github.com/user-attachments/assets/28c753f8-d240-4040-8f01-77586a704913)

5. Find the dates of the first successful landing outcome on ground pad
```python
%sql ALTER TABLE SPACEXTABLE MODIFY COLUMN Date DATE;
%sql SELECT MIN(Date) as first_launch_date  FROM SPACEXTABLE WHERE Mission_Outcome LIKE '%SUCCESS%';
```
![image](https://github.com/user-attachments/assets/72838053-3c56-4dc8-a050-5f3862f80cb3)

6. List the names of boosters which have successfully landed on drone ship and had payload mass greater than 6000
```python
%%sql
SELECT * FROM SPACEXTABLE WHERE PAYLOAD_MASS__KG_ > 4000
  AND PAYLOAD_MASS__KG_ < 6000
  AND Landing_Outcome LIKE '%Success (drone ship)%' ;
```
![image](https://github.com/user-attachments/assets/5554ea3a-bb83-453b-9b92-7ba93cfda5f5)

7. Calculate the total number of successful and failure mission outcomes
```python
%sql Select Mission_Outcome,Count(*) AS No_of_attempts from SPACEXTABLE GROUP BY Mission_Outcome;
```
![image](https://github.com/user-attachments/assets/c0cb9f2b-732d-455d-88ce-af4bece5a92f)
8.List the names of booster which have carried the maximum payload mass
```python
%sql ALTER TABLE SPACEXTABLE ADD COLUMN Booster VARCHAR(10);
%sql UPDATE SPACEXTABLE SET Booster = SUBSTR(Booster_Version, 1, 5);

%sql Select Booster,MAX(PAYLOAD_MASS__KG_) from SPACEXTABLE GROUP BY Booster ORDER BY PAYLOAD_MASS__KG_ desc;
```
![image](https://github.com/user-attachments/assets/581ef08d-910b-4661-80b1-e89a7adb2883)
9. List the failed landing outcomes in drone ship, their booster versions and launch site names for in year 2015
```python
%sql SELECT SUBSTR(Date,6,2) AS Month ,Landing_Outcome,Booster_Version,Launch_site
from SPACEXTABLE
WHERE Landing_Outcome LIKE "Failure%" AND SUBSTR(Date,0,5)='2015';
```
![image](https://github.com/user-attachments/assets/5882ddb6-d296-4e83-8e5b-06372e1503b4)

10. Rank the count of landing outcomes Between 2010-06-04 and 2017-03-20
``` python
%%sql SELECT Landing_Outcome, COUNT(*) AS Outcome_Count
FROM SPACEXTABLE
WHERE Date BETWEEN '2010-06-04' AND '2017-03-20'
GROUP BY Landing_Outcome
ORDER BY Outcome_Count DESC;
```
![image](https://github.com/user-attachments/assets/7a17f032-5683-4658-a7de-be407ea946cd)

### Mapping using Folium Library (Geo Mapping)
#### Launching sites Geomapping
![image](https://github.com/user-attachments/assets/f66f50a8-fa82-476c-a6f3-76e1ade0219a)

#### SUCCESS RATE of Launching sites
![image](https://github.com/user-attachments/assets/bdb942e6-da04-47e8-b968-2dbb09c426bb)
![image](https://github.com/user-attachments/assets/b3a33977-62a9-4e41-a6cc-5247dcf2dd81)
#### Coastal line to Launching Site distance
![image](https://github.com/user-attachments/assets/b628c501-e4ec-46f3-9d9e-ba064d3fa5dd)

---------------------------------------------
## Spacex Launch Dashboard
![image](https://github.com/user-attachments/assets/8475a6b6-5068-4c59-8a26-251fbc48b0e0)

---------------------------------------------------------------
## Predictive Analysis Classification

|Accuracy                           | 
|-----------------------------------|                                                   
|Logistic Regression: **83.33%**         |
|SVM: **83.33%**                         |
|Decision Tree: **94.44%** (Best Performing)|
|KNN: **83.33%**                         |
 ![Picture1](https://github.com/user-attachments/assets/8bbcc179-e16f-4f84-b021-4bc24bc44a0a)

#### **Confusion Matrix**
+ GridsearchCV object with 10 fold cross validation
  
 | GridSearchCV  | KNN Model |
 |---------------|-----------|
 | ![image](https://github.com/user-attachments/assets/0d17cd9b-6756-46c4-9038-5f60d07b17b1)|![image](https://github.com/user-attachments/assets/8849900d-9524-41ef-b25b-0c6295b63666)|



 

 




