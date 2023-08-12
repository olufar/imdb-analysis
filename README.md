# imdb-analysis

# Data Analysis of IMDb dataset
!![imdb](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/c0f18227-bdee-499b-8a88-48b84e2dc46f)

**Overview**
---
This project is focusing on the use of Python to analyse data. The dataset is the International Movies Database (IMDb) Data.

**Problem Statement**
---
The purpose is to analyse IMDB dataset and provide insights to the following business questions:
+ Investigate revenues generated across years for IMDB movies.
+ What is the relationship between revenue and budget? 
+ Which country generated the highest revenue in movies? 
+ What is the highest rated movie and which genre does it belong to?
+ Does movie budget affect movie rating? Show this with your analysis?

**Software and Tools**
---
The following are the required tools and libraries to carry out this data analysis:
* Python (version 3:10)
* Python libraries: pandas, numpy, matplotlib and seaborn
* Jupyter Notebook

The first step in the project is to import the libraries we are going to be using for the peoject as follows:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

**Loading of Dataset**
---
After importing the libraries, I loaded the dataset from our CSV file using pd.read_csv() as follows:
```
Movies=pd.read_csv(r'\Users\muyiw\Downloads\imdb_movies.csv')
```

**Data Assessment**
---
As a practice, I always create a copy of the dataframe and make use of this copy for analysis while keeping the original dataset loaded into the dataframe. This following code shows how I created a copy,'Movies_clean' of the dataframe, 'Movies'. 
```
Movies_clean= Movies.copy()
```

To confirm the dimension(number of rows and columns)of the dataframe, the following line of code was implemented:
```
Movies_clean.shape
```
*The output showed that thisd dataframe has 10178 rows and 12 columns.*

The columns and data types of the dataframe were determined as follows:
```
Movies_clean.dtypes
```
The output showed that :

* names,date_x,genre,overview,crew,orig_title,status,orig_lang and country are object datatype columns
* rate_ score is an int64 datatype column
* budget_x ($) and revenue ($) are float64 datatype columns

I checked the dataframe for list of columns with null values using this code:
```
Movies_clean[Movies_clean.columns[Movies_clean.isnull().any()]].isnull().sum()
```
*It was confirmed that genre column has 85 null values and crew column has 56 null values*

The folowing code line was used to express number of null values as a percentage of number of values in each column
```
Movies_clean[Movies_clean.columns[Movies_clean.isnull().any()]].isnull().sum()* 100/ len(Movies_clean)
```
I checked the dataframe for existence of duplicated rows using the following code:
```
Movies_clean.duplicated().any()
```
*It was confirmed that the dataframe has no duplicated rows.*

The summary statistics of the data set was displayed using this code:
```
 Movies_clean.describe()
```
!![statistics](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/70261c12-07c1-4133-98d2-6a3be382d1d8)

*Observations:*
* date_x column shows as object datatype, there is need to change it to datetime datatype
* Values in names and orig_title are the same for movies where orig_lang is English. Movies with orig_lang not English have different values for names and orig_title
* The dataset has no duplicated rows
* All columns except genre and crew columns have 10178 values. The genre column has 85 null values and crew column has 56 null values.
  
**Data Cleaning and Transformation**
---
Based on my observations during the data exploration process, the dataframe was efficiently cleaned and transformed using pandas libraries. The steps are as follows:
* I changed date_x column datatype from object to datetime datatype using .astype() and specifying parameter as 'datetime64[ns]'
  ```
  Movies_clean["date_x"] = Movies_clean["date_x"].astype('datetime64[ns]')
  ```
  
* I dropped all rows with null values using .dropna() on the dataframe,setting axis parameter to 0 and inplace parameter as True
  ```
  Movies_clean.dropna(axis=0 , inplace=True) 
  ```
* Created a new column,'Movie Year' as an extraction of year from date_x values using .dt.year()
```
  Movies_clean['Movie Year'] = Movies_clean['date_x'].dt.year 
```

At this stage, the dimension of the dataframe was reconfirmed using this code:
```
Movies_clean.shape
```
*The dataframe contains 10052 rows and 13 columns after transformation.*

I also reconfirmed existence of null values using this code:
```
Movies_clean[Movies_clean.columns[Movies_clean.isnull().any()]].isnull().sum()
```
*The dataframe has no(zero) null value after transformation.*

## Exploratory Data Analysis

To understand the numerical columns better, I represented them visually as follows:

!![Box_Plot_Rating_Score](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/15bd6877-f9f5-49d3-afb7-251614e26d4b)

*This box plot is a left skewed one as the left whisker is longer than the right whisker.There are also more outliers tothe left than we have to the right.*
```
Movies_clean['rate_ score'].plot(kind='hist',bins=20)
plt.title('Rating Score Distribution', pad=20);
```

!![Rating_Score_Distribution](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/16267947-d187-42f9-87a5-277ce4a10474)

*This Rating Score distribution is a left skewed(negatively skewed) because it has a longer tail to the left. It is also unimodal because it has one main peak.*

!![Box_Plot_Movie_Budget](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/bfe71f7a-150a-422e-a0ea-5b89d688b303)

*The box plot is a right skewed one as Q3-Q2>Q2-Q1 and the right whisker is longer than the left whisker.This plot also shows huge number of outliers to the right.*
```
Movies_clean['budget_x ($)'].plot(kind='hist', bins=20)
plt.title('Movie Budget Distribution', pad=20);
```

!![Movie_Budget_Distribution_Histogram](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/2fdc04a9-e7bb-4365-b9ee-3db5141439c6)

*This shows that Movie Budget distribution is a right skewed(positively skewed) one because it has a longer tail to the right. It is also unimodal because it has one main peak.*

!![Box_Plot_Revenue](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/15a4da20-9913-4301-99e5-0c55174fe573)

*The box plot is a right skewed one as Q3-Q2>Q2-Q1 and the right whisker is longer than the left whisker.This plot also shows huge number of outliers to the right.*

```
Movies_clean['revenue ($)'].plot(kind='hist',bins=20)
plt.title('Revenue Distribution', pad=20);
```
!![Revenue_Distribution_Histogram](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/f9689ca4-891d-4b0c-b23e-56e6604e3e9c)

*This is a right skewed(positively skewed) histogram because it has a longer tail to the right. It is also unimodal because it has one main peak.*

### Question 1: Revenues generated across years for IMDB movies

To illustrate the revenue generated across the years in the dataset, I implemented this code:
```
Movies_clean.groupby('Movie Year')['revenue ($)'].sum().sort_values(ascending=False)
```

To understand the trend within the last ten years, I implemented the following code:
```
Movies_clean.groupby('Movie Year')['revenue ($)'].sum().tail(10).plot(kind='bar', width=0.5)
plt.xticks(rotation=45)
plt.title('Revenues Generated 2014 - 2023 for IMDB movies', pad=20);
```
!![Revenues_Generated_2014_and_ 2023_IMDB_movies](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/55ac0f0f-70b8-4cde-8712-9ab575b04252)

The bar chart shows a growth in revenue year on year from 2014 to 2022 but declines in 2023


### Question 2: What is the relationship between revenue and budget?

The Relational Plot (relplot) allows us to visualise how variables within a dataset relate to each other. The following code shows how I implemented the relational plot to visualise the relationship between movie revenue and budget.

```
sns.relplot(data=Movies_clean, x='budget_x ($)', y='revenue ($)')
plt.title("Relationship Between Revenue and Budget", pad=20);
```
!![Relationship_Between_Revenue_and_Budget](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/37db65b1-b480-48a9-88de-9a1af06b8af5)

The plot shows a linear relationship between revenue and budget.

To further understand the relationship between movie revenue and budget, I implemented regression plot that creates a regression line between the 2 variables and helps to visualize their linear relationships as follows:
```
sns.regplot(data=Movies_clean, x='budget_x ($)', y='revenue ($)',marker='+')
plt.title("Relationship Between Revenue and Budget", pad=20);
```
!![regression_plot_revenue_vs_budget](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/aae7ada9-228a-475b-9663-702e5da79f3c)

*This plot shows that there is a positive relationship between revenue and movie budget.This means that the revenue increases as the budget increases and vice versa*

I also calculated the correlation coefficient between the two variables using the following code:

```
round(Movies_clean['budget_x ($)'].corr(Movies_clean['revenue ($)']),2)
```
*The result shows that the correlation coefficient is 0.68 and this implies that there is positive linear correlation between the two variables*

### Question 3: Which country generated the highest revenue in movies?

```
Movies_clean.groupby('country')['revenue ($)'].sum().sort_values(ascending=False).head(1)
```

The result shows that AU is the country with the highest revenue of 9.425385e+11

### Question 4: What is the highest rated movie and which genre does it belong to?

This step subsets(assigns) the highest rated movie(s) into the dataframe named Highest_rated
```
Highest_rated = Movies_clean[Movies_clean['rate_ score']==Movies_clean['rate_ score'].max()]
```

This step resets the index of the dataframe Highest_rated and displays highest rated movie(s) and the genre(s)

```
Highest_rated[['names','genre']].reset_index()
```
!![highest_rated_movies](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/e66f3478-2820-4f2e-b0e8-5865789db203)

The table shown above lists all movies with the highest rating of 100 and their corresponding genre(s)

### Question 5: Does movie budget affect movie rating? Show this with your analysis?

```
Movies_clean.plot.scatter(x = 'rate_ score' , y ='budget_x ($)', s = 1)
plt.title("Relationship Between Rating and Movie Budget", pad=20);
```
!![scatter_budget_rating](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/2bbf87f5-1639-4d8a-b717-ab2804b22db2)


```
sns.regplot(data=Movies_clean, x = 'rate_ score',y='budget_x ($)')
plt.title("Relationship Between Rating and Movie Budget", pad=20);
```
!![regplot_rating_budget](https://github.com/muyiwafarinde/imdb-movies-python-analysis/assets/19453673/84632af2-54a1-4b6a-984d-5db109498e19)

This plot shows that there is a negative linear relationship between the budget and movie rating.

The following line of code gives the correlation coefficent value of the relationship between budget_x ($) and rate_ score 
```
round(Movies_clean['budget_x ($)'].corr(Movies_clean['rate_ score']),2)
```
*The result also shows a correlation coefficient of -0.21 which indicates a negative linear correlation between the two variables*
  
