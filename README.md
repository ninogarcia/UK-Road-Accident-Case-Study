# Case Study - UK Road Accident Using Python, SQL and Tableau

## Introduction
 The case study involves a Dataset of UK Accidents.

The UK government maintains an exhaustive repository of data on traffic accidents, which is updated annually and made available to the public via the Open Data website. 

The analysis will follow the 6 phases of the Data Analysis process: Ask, Prepare, Process, Analyze, and Act (APPAA).

## ASK

In this case study, we will explore the UK Road Accident dataset to analyze different factors that contribute to traffic accidents. The dataset includes a range of variables, such as geographic locations, weather conditions, vehicle types, and the number of casualties involved, making it a valuable resource for understanding the causes and effects of road accidents. To achieve this goal, we will address the following questions:
&nbsp;

* What are the most common types of vehicles involved in accidents?
&nbsp;

* What are the most frequent weather conditions associated with accidents?
&nbsp;

* How do the number of casualties vary across different types of accidents?
&nbsp;

* Which geographic areas have the highest occurrence of accidents?
&nbsp;

By answering these questions, we can gain valuable insights into the factors that contribute to road accidents and identify areas where interventions may be necessary to reduce the occurrence of accidents.
&nbsp;
&nbsp;
&nbsp;

## PREPARE

The dataset contains an array of information, ranging from geographic locations to weather conditions, vehicle types, and the number of casualties involved. This comprehensive and fascinating collection of data provides an excellent opportunity for in-depth analysis and research, and the Department of Transport is responsible for publishing it. 

## PROCESS

### Cleaning and Preparation of data for analysis

#### USING PYTHON 

Read the CSV file
```python
import pandas as pd

df = pd.read_csv(r'...\data.csv')
```
&nbsp;

Check the columns
```python
df.columns
```
&nbsp;

Calculate the percentage of missing values for each column in the dataframe
```python
df.isnull().sum() / len(df) * 100
```
&nbsp;

Clean the date column
```python
df['Accident_Date'] = df['Accident_Date'].str.replace('-', '/')
df['Accident_Date'] = pd.to_datetime(df['Accident_Date'], format='%d/%m/%Y')
```
&nbsp;

Clean longitude and latitude columns
```python
df['Longitude'] = df['Longitude'].replace('NA', pd.NA)
df['Latitude'] = df['Latitude'].replace('NA', pd.NA)
```
&nbsp;

Drop rows with NaN
```python
df = df.dropna(subset=['Longitude', 'Latitude', 'Road_Type', 'Road_Surface_Conditions', 'Weather_Conditions'])
```
&nbsp;

Save the cleaned data to a new CSV file
```python
df.to_csv('cleaned_data.csv', index=False)
```
&nbsp;

## ANALYZE

#### USING MySQL 

-- Check the data
```sql
SELECT * FROM accidents;
```
&nbsp;

-- Total number of accidents
```sql
SELECT
	COUNT(ID) AS total_accidents
FROM 
	accidents;
```
&nbsp;


-- What are the number of casualties in each day of the week? Sort them in descending order.
```sql
SELECT
  DATE_FORMAT(Accident_Date, '%W') AS Day_of_Week,
  SUM(Number_of_Casualties) AS Total_Casualties
FROM
  accidents 
GROUP BY
  Day_of_Week
ORDER BY
  Total_Casualties DESC;
```
&nbsp;



-- What are the number of casualties in each  month? 
```sql
SELECT 
    DATE_FORMAT(Accident_Date, '%Y-%m') AS Month, 
    SUM(Number_of_Casualties) AS Total_Casualties
FROM 
    accidents
GROUP BY 
    Month
ORDER BY 
    Month ASC;
```
&nbsp;

-- Which geographic areas have the highest occurrence of accidents?
```sql
SELECT 
    District_Area, 
    SUM(Number_of_Casualties) AS Total_Casualties
FROM 
    accidents
GROUP BY 
    District_Area
ORDER BY 
    Total_Casualties DESC
LIMIT 10;
```
&nbsp;

-- What are the most frequent weather conditions associated with accidents?
```sql
SELECT 
    Weather_Conditions, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
GROUP BY 
    Weather_Conditions
ORDER BY 
    Num_Accidents DESC;
```
&nbsp;

-- What are the most common types of vehicles involved in accidents?
```sql
SELECT 
    Vehicle_Type, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
GROUP BY 
    Vehicle_Type
ORDER BY 
    Num_Accidents DESC;
```
&nbsp;

-- How do the number of casualties vary across different types of accidents?
```sql
SELECT 
    Accident_Severity, 
    Road_Type, 
    COUNT(*) AS Num_Accidents, 
    SUM(Number_of_Casualties) AS Num_Casualties
FROM 
    accidents
GROUP BY 
    Accident_Severity, Road_Type
ORDER BY 
    Accident_Severity ASC, Num_Casualties DESC;
```
&nbsp;

-- Quantity and ratio of severity. 
```sql
SELECT 
	accident_severity,
	COUNT(accident_severity) AS total_severity,
	ROUND(COUNT(*) * 100./ SUM(COUNT(*)) OVER (),2) as percentage_of_accidents
from
	accidents
GROUP BY accident_severity
ORDER BY 3 DESC;
```
&nbsp;


-- Verify if most accidents happens during the night or day.
```sql
SELECT 
    Light_Conditions, 
    COUNT(*) AS Num_Accidents
FROM 
    accidents
WHERE 
    Light_Conditions IN ('Daylight', 'Darkness - lights lit', 'Darkness - no lighting', 'Darkness - lighting unknown', 'Darkness - lights unlit')
GROUP BY 
    Light_Conditions
ORDER BY 
    Num_Accidents DESC;
```
&nbsp;

### VISUALIZATION

DYNAMIC DASHBOARD USING TABLEAU

The visualization of data has become an integral part of data analysis. Among various tools available, Tableau is one of the most popular and user-friendly data visualization tools that is widely used in the industry. With Tableau, users can easily create interactive dashboards, charts, and graphs that help to visualize complex data sets in a meaningful way. To fully experience the power of Tableau, users can click on the provided link to view the visualization and explore the data in detail. [Click here](https://public.tableau.com/app/profile/ninz.g/viz/UKAccidents_16788586726130/Dashboard1)!

![Dashboard 1](https://user-images.githubusercontent.com/7455410/225262909-34d27be9-ec1c-4d13-ad7e-247b7f2bac6c.png)
&nbsp;

## ACT

### Conclusions:

1. What are the most common types of vehicles involved in accidents?
	* Based on the query results, we can see that the most common types of vehicles involved in accidents are cars, vans or goods vehicles under 3.5 tonnes, buses or coaches with 17 or more passenger seats, motorcycles over 500cc, and goods vehicles over 7.5 tonnes. These five types of vehicles account for the vast majority of accidents, with cars being the most common type of vehicle involved in accidents by a significant margin.

	* Interestingly, motorcycles are the second most common type of vehicle involved in accidents, even though they are less frequently seen on the roads compared to cars and vans. This suggests that motorcycles are particularly vulnerable to accidents and may require additional safety measures to prevent accidents from occurring.
&nbsp;

2. What are the most frequent weather conditions associated with accidents?
	* Based on the results of the query on the most frequent weather conditions associated with accidents, it is evident that the majority of accidents occur when the weather is fine with no high winds, with a total of 517,875 accidents. The next most common weather condition is rain without high winds, with a total of 79,257 accidents. Other weather conditions such as fog or mist, snowing without high winds, and snowing with high winds have much lower accident frequencies, with less than 10,000 accidents each. Accidents that occur in rainy conditions with high winds or fine conditions with high winds are relatively rare, with 9,559 and 8,501 accidents respectively. It can be concluded that the majority of accidents occur during fine weather conditions without high winds, which could be attributed to the increased number of vehicles on the road during these conditions. However, it is essential to note that even though rain without high winds is the second most frequent weather condition associated with accidents, it is still a significant cause of accidents, and drivers should exercise caution while driving in such conditions.

3. How do the number of casualties vary across different types of accidents?
	* The results of the query indicate that the number of casualties varies across different types of accidents. Single carriageway accidents have the highest number of accidents across all categories, with 406,706 slight accidents, 69,116 serious accidents, and 6,452 fatal accidents. Dual carriageway accidents have the second-highest number of accidents, with 84,252 slight accidents, 11,604 serious accidents, and 1,790 fatal accidents. Roundabout accidents have the third-highest number of accidents, with 39,017 slight accidents, 3,603 serious accidents, and 140 fatal accidents. Slip road accidents have the lowest number of accidents across all categories, with 6,239 slight accidents, 605 serious accidents, and 49 fatal accidents. The results suggest that single carriageway accidents are the most dangerous and result in the highest number of casualties. Dual carriageway and roundabout accidents also have a significant number of casualties, while slip road accidents are the least dangerous.

4. Which geographic areas have the highest occurrence of accidents?
	* Based on the query results, it appears that Birmingham has the highest occurrence of accidents, with a total of 18,015 casualties reported. Leeds and Bradford follow closely with 12,301 and 9,204 casualties respectively. Manchester and Liverpool also had high numbers of casualties, with 8,779 and 8,480 respectively. Other areas that had high numbers of casualties include Sheffield, Kirklees, Westminster, Glasgow City, and Doncaster. It is worth noting that these areas may have high numbers of accidents due to factors such as population density, traffic volume, and road infrastructure. Further analysis would be needed to determine the specific causes of these high accident rates in each area.
	
### Recommendations:

Based on the above conclusions, here are some recommendations to improve road safety:

1. Encourage additional safety measures for motorcycles: Given that motorcycles are the second most common type of vehicle involved in accidents, it may be worthwhile to explore additional safety measures such as improving road infrastructure, mandatory helmet use, or better education for motorcycle riders.

2. Focus on improving driving in fine weather conditions: As the majority of accidents occur during fine weather conditions, it may be worthwhile to explore ways to improve driver education and awareness during such conditions to help reduce the number of accidents.

3. Improve road infrastructure in areas with high accident rates: Given that certain geographic areas have higher accident rates, improving road infrastructure in those areas could help reduce the number of accidents.

4. Increase public transportation options: Encouraging the use of public transportation in areas with high accident rates, such as Birmingham, Leeds, and Bradford, could help reduce the number of accidents by reducing the number of vehicles on the road.

Overall, improving road safety requires a multi-faceted approach that involves education, infrastructure, and policy changes. These recommendations are a starting point for improving road safety and should be further explored and tailored to specific geographic areas and the types of accidents that occur.


## References

Dataset Source: [Department of Transportation](https://transparentcalifornia.com/download/salaries/san-francisco/)

Licenses: [Database: Open Database, Contents: Database Contents](http://opendatacommons.org/licenses/dbcl/1.0/)


## Contact Information
[LinkedIn](https://www.linkedin.com/in/ninogarci/)
&nbsp;

[UpWork](https://www.upwork.com/freelancers/~01dd78612ac234aadd)
