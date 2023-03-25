# Uber_Request_Analysis
## ANALYZNG UBER DEMAND AND SUPPLY
## METHOD USED
* Data discovery
* Data preparation
* Data transformation 
* Data wrangling
* Data visualization
## Tools
* Python libraries such Pandas, Numpy, Matplolib, Seanborn
Pandas it has functions for analyzing, cleaning, exploring, and manipulating data.
Numpy used for performinng mathematical operations
Matplotlib for creating static, animated, and interactive visualizations
Seanborn it is build on top of matplotlib it is used for data visualization and exploratory data analysis
## Target Data
*  We are going to perform exploratory data analysis EDA to Uber request data for a given period of time.
*  The purpose of this analysis is to establish the demand and supply of uber rides in a given time frame and conditions.
## Requirements for this project
* data exploration/descriptive statistics.
* data processing/cleaning
* writeup/reporting
## Importing relevant libraries 
```
import pandas as pd
import numpy as np
import datetime as dt
import plotly.express as px
import plotly.graph_objects as go
import plotly.io as pio
import seaborn as sns
import matplotlib.pyplot as plt
pio.templates.default = "plotly_white"
```
## Data Display
We display our data stored in CSV format to have a general view of our data set 
```
data = pd.read_csv('Uber Request Data.csv')
print(data.head())
```
##  We check null values in our dataset
```
print(data.isnull().sum())
```
This will help us to clean our data by removing irrelavant fields in our data set
## We check basic information in our data 
It will help us to determine the number of columns and rows we have in our dataset and also describe their datatypes
```
data.info()
```
### We determine the number of rides which were commpleted , cancelled and no car available
```
data.Status.value_counts()
```
This will give us the sum of the rides which were completed , cancelled and no cars available , where we find out that we have many completed 
rides as compared to the cancelled rides during that periods and also more no car available as compared to cancelled rides

![comletedR](https://user-images.githubusercontent.com/44755841/227680113-4140ab7a-3546-4ecc-b0e2-20377b313014.png)

### Rename columns
Given that we are analyzing data which is stored in csv format will have to rename our columns to a convetion that is readable 
by python script such as in camel case format
```
## first we rename columns in the data set
newData=data.rename(columns={'Request id': 'Request_id', 'Driver id': 'Driver_id','Pickup point':'Pickup_point',
                             'Request timestamp':'Request_timestamp','Drop timestamp':'Drop_timestamp','Status':'status'})
newData.head()
```
![changeCoumns](https://user-images.githubusercontent.com/44755841/227681463-ec6b22e4-73ff-4054-b003-1e7ce4ecf960.png)
### How many drivers were there in our data set and the number of rides requested in a day
```
demand_vs_supply = pd.DataFrame({'Total No. of Drivers':[newData['Driver_id'].nunique()], 
                                 'Total Demand Per Day':[newData['Request_id'].nunique()/5],
                                 'Trip_Completed_Count_Per_Day':newData[(newData['status']=='Trip Completed')].shape[0]/5,
                                 ' Trip_cancelled_count_per_day' :newData[newData['status']=='Cancelled'].shape[0]/5})
                                 
demand_vs_supply
```
We arrive to this by counting the number of individual drivers id which is the unique identifier , demand per day by calculating the number of trips 
requested in a day, This summary also gives us the number of completed and cancelled trips in a day
![summay](https://user-images.githubusercontent.com/44755841/227681857-3342c899-ca4a-4557-8556-d8045b11fa04.png)

### What is the avarege request for each driver in a day
```
# on average how many request each driver gets
demand_vs_supply['Total Demand Per Day']/demand_vs_supply['Total No. of Drivers']
```
The result is 4 however we expect each uber driver to have atleast 4 rides in a day according to the uber demand in that area
### Where did we had most pickup
```
print("Pickuo point:\n", newData.Pickup_point.value_counts())
## percentage of camplete, cancelled and not not availble trips in each pick point
print("Percantage as per drop: \n", round(newData.Pickup_point.value_counts()/newData.Pickup_point.size*100,1))
newData.groupby(by=['Pickup_point'])['status'].value_counts(normalize=True).unstack('status').plot.bar(stacked=True);
```
Most pick up occurred in the city which make a total of 52% and 48% was from the Airport. This show there  is high demand in the city as compared to the 
airport.

![StatusPickup](https://user-images.githubusercontent.com/44755841/227682561-e131e5fb-a3b6-4ab3-915b-7a0b43e69cb8.png)

### For easy analysis we change our data format for our dataset this the pickup date and drop date
```
newData['Request_timestamp'] = pd.to_datetime(newData['Request_timestamp'],
                                       errors='coerce')
newData['Drop_timestamp'] = pd.to_datetime(newData['Drop_timestamp'],
                                     errors='coerce')
newData.head()
```
![changeDatafor](https://user-images.githubusercontent.com/44755841/227682831-07f1eda3-5db2-4a71-9ff2-7b5bb6df4e21.png)

### Splitting the Reuest time to date and time column and then converting the time into four different categories i.e. Morning, Afternoon, Evening, Night
```
from datetime import datetime

newData['Request_month'] = pd.DatetimeIndex(newData['Request_timestamp']).month
newData['Request_date'] = pd.DatetimeIndex(newData['Request_timestamp']).date
newData['Request_time'] = pd.DatetimeIndex(newData['Request_timestamp']).hour
 
#changing into categories of day and night
newData['day-night'] = pd.cut(x=newData['Request_time'],
                              bins = [0,10,15,19,24],
                              labels = ['Morning','Afternoon','Evening','Night'])
newData.head()
```
![changingDayFor](https://user-images.githubusercontent.com/44755841/227683982-51df73d9-3c76-4fcc-bfdf-6d0ec9097442.png)


### How many null values do we have in pick timestamp and drop timestamp
```
Request_timestamp_count_of_null_values = newData['Request_timestamp'].isnull().sum()
Drop_timestamp_count_of_null_values = newData['Drop_timestamp'].isnull().sum()
Driver_id_count_of_null_values = newData['Driver_id'].isnull().sum()
print("Number of rows containing null values in Request timestamp:", Request_timestamp_count_of_null_values)
print("Number of rows containing null values in Drop timestamp:", Drop_timestamp_count_of_null_values)
print("Number of rows containing null values in Driver id:", Driver_id_count_of_null_values)

```
![nullTimestamp](https://user-images.githubusercontent.com/44755841/227685537-7c03e75f-aa98-43a2-a4ad-f7f023210cdb.png)

### Grouping data by Status (completed , cancelled and no cars available
```
newData.groupby(newData.status).count()
```
![groupStatus](https://user-images.githubusercontent.com/44755841/227686863-b0059996-3e21-404c-99fd-e9521cd8a601.png)











