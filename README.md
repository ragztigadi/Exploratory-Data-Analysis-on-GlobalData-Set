# Exploratory Data Analysis (EDA) on Zomato Dataset

This project performs an exploratory data analysis (EDA) on the Zomato restaurant dataset to uncover insights about restaurant ratings, delivery options, cuisines, and other important factors. The dataset was merged with country codes to gain additional insights into the countries where Zomato is active.

## Project Overview

The Zomato dataset contains information about restaurants, including their ratings, cuisines, locations, delivery options, and other attributes. This analysis aims to explore:

- The most common cuisines and cities.
- The relationship between ratings and various restaurant features.
- Countries with the highest usage of Zomato.
- Distribution of restaurant ratings and delivery options.

## Dataset Description

The dataset consists of the following columns:

1. **Restaurant ID**: Unique identifier for each restaurant.
2. **Restaurant Name**: Name of the restaurant.
3. **Country Code**: Country code representing the country where the restaurant is located.
4. **City**: City where the restaurant is located.
5. **Address**: Restaurant's address.
6. **Locality**: Local area within the city where the restaurant is located.
7. **Longitude**: Longitude coordinates of the restaurant.
8. **Latitude**: Latitude coordinates of the restaurant.
9. **Cuisines**: Types of cuisines served at the restaurant.
10. **Average Cost for Two**: Average cost for a meal for two people.
11. **Currency**: Currency used for prices.
12. **Has Table Booking**: Whether the restaurant allows table bookings.
13. **Has Online Delivery**: Whether the restaurant offers online delivery.
14. **Is Delivering Now**: Whether the restaurant is currently delivering food.
15. **Switch to Order Menu**: Whether the restaurant has an online order menu.
16. **Price Range**: The price range of the restaurant.
17. **Aggregate Rating**: The aggregate rating of the restaurant.
18. **Rating Color**: The color associated with the rating.
19. **Rating Text**: Descriptive text based on the rating.
20. **Votes**: The number of votes the restaurant has received.
21. **Country**: The country where the restaurant is located.

## EDA Steps

### 1. Data Loading and Inspection

We started by loading the dataset and inspecting its structure. We also checked for missing values in the dataset.

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

df = pd.read_csv('zomato.csv', encoding='latin-1')
df.head()
```

### 2. Handling Missing Values

We identified columns with missing values and took necessary actions to handle them. In particular, `Cuisines` had 9 missing values.

```python
# Check for missing values
df.isnull().sum()

# List columns with missing values
[features for features in df.columns if df[features].isnull().sum() > 0]
```

### 3. Merging Data with Country Information

We merged the dataset with a country code dataset (`Country-Code.xlsx`) to provide better insights into the country-specific data.

```python
df_country = pd.read_excel("Country-Code.xlsx")
final_df = pd.merge(df, df_country, on='Country Code', how='left')
final_df.dtypes
```

### 4. Exploring Country-Specific Data

We examined the distribution of restaurants in various countries. A pie chart was used to visualize the top 3 countries with the most restaurants in the dataset.

```python
country_val = final_df['Country'].value_counts().values
country_names = final_df['Country'].value_counts().index

# Pie chart for top 3 countries
plt.pie(country_val[:3], labels=country_names[:3], autopct='%1.2f%%')
```

#### Observations:

- The top 3 countries with the maximum number of restaurants are:
  1. India
  2. United States
  3. United Kingdom

### 5. Rating Distribution and Insights

We grouped restaurants by their **Aggregate Rating**, **Rating Color**, and **Rating Text**, then visualized the distribution of ratings.

```python
ratings = final_df.groupby(['Aggregate rating', 'Rating color', 'Rating text']).size().reset_index().rename(columns={0: 'Rating count'})
```

#### Observations:

- Ratings are categorized into the following groups:
  - **Excellent**: 4.5 to 4.9
  - **Very Good**: 4.0 to 4.4
  - **Good**: 3.5 to 3.9
  - **Average**: 2.5 to 3.4
  - **Poor**: 0.0 to 2.4

We used a **count plot** to visualize the distribution of ratings based on their color.

```python
sns.countplot(x="Rating color", data=ratings, palette=['white', 'red', 'orange', 'yellow', 'green'])
```

#### Observations:

- The highest ratings are given by customers in India.
- A large portion of ratings is between 2.5 and 3.9.

### 6. Online Delivery Insights

We analyzed the **Has Online Delivery** feature to identify which countries offer the most online delivery options. The countries with the highest number of restaurants offering online delivery are **India** and the **UAE**.

```python
final_df[['Country', 'Has Online delivery']].groupby(['Country', 'Has Online delivery']).size().reset_index()
```

### 7. City Distribution

We visualized the top 5 cities with the most number of restaurants using a pie chart.

```python
city_values = final_df['City'].value_counts().values
city_labels = final_df['City'].value_counts().index
plt.pie(city_values[:5], labels=city_labels[:5], autopct='%1.2f%%')
```

### 8. Top 10 Cuisines

We analyzed the **Cuisines** column to identify the most popular cuisines across the dataset. The top 10 cuisines were visualized using a pie chart.

```python
cuisines_values = final_df['Cuisines'].value_counts().values[:10]
cuisines_labels = final_df['Cuisines'].value_counts().index[:10]

# Plot top 10 cuisines
plt.figure(figsize=(8, 8))
plt.pie(cuisines_values, labels=cuisines_labels, autopct='%1.2f%%')
```

#### Observations:

- The top 10 most popular cuisines are:
  1. North Indian
  2. Chinese
  3. Fast Food
  4. Cafe
  5. Bakery
  6. Mughlai
  7. South Indian
  8. Italian
  9. Continental
  10. Thai

## Requirements

- **pandas**: For data manipulation and analysis.
- **numpy**: For numerical operations.
- **matplotlib**: For data visualization.
- **seaborn**: For statistical data visualization.

## Conclusion

Through this EDA, we gained valuable insights into restaurant ratings, cuisines, cities, and countries. We identified the most popular cuisines and cities, the distribution of ratings, and online delivery availability across countries.

## Future Work

- Further data cleaning and imputation for missing values.
- Building a predictive model to estimate restaurant ratings based on various features.
- More in-depth analysis of restaurant price ranges and their relationship with ratings.
