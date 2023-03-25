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
OUTPUT
![comletedR](https://user-images.githubusercontent.com/44755841/227680113-4140ab7a-3546-4ecc-b0e2-20377b313014.png)




