# Case Study - UK Road Accident Using Python, SQL and Tableau

## Introduction
 The case study involves a Dataset of UK Accidents.

The UK government maintains an exhaustive repository of data on traffic accidents, which is updated annually and made available to the public via the Open Data website. The dataset contains an array of information, ranging from geographic locations to weather conditions, vehicle types, and the number of casualties involved. This comprehensive and fascinating collection of data provides an excellent opportunity for in-depth analysis and research, and the Department of Transport is responsible for publishing it. 


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

### Cleaning and Preparation of data for analysis

#### USING PYTHON 

Read the CSV file
```python
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

