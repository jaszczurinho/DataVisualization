# Shopping Trends Analysis

## Overview
This repository contains Python code for analyzing shopping trends using the "shopping_trends.csv" dataset. The code utilizes the Pandas, Matplotlib, NumPy, and Seaborn libraries for data manipulation, visualization, and analysis. The analysis covers various aspects of shopping behavior, including descriptive statistics, expenses by gender, favorite accessories, payment methods, and more.

## Code Structure
- **Data Loading and Overview**
  - The dataset is loaded using Pandas, and the first six observations are displayed to provide a quick overview of the data.

    ```python
    import pandas as pd

    data = pd.read_csv("shopping_trends.csv")
    data.head()
    ```

- **Overall Dataset Information**
  - Basic information about the dataset is displayed, including the number of columns, rows, and the count of missing values for each column.

    ```python
    data.shape[1]  # Number of columns
    data.shape[0]  # Number of rows
    data.isna().sum()  # Number of missing values for each column
    ```

- **Descriptive Statistics**
  - Descriptive statistics are calculated for numeric columns and presented in a table.

    ```python
    numeric_data = data.select_dtypes(include='number')
    stats = numeric_data.describe().iloc[:, 1:].loc[["mean", "std", "min", "50%", "max"]]
    stats.Age = stats.Age.round()
    stats["Previous Purchases"] = stats["Previous Purchases"].round()
    stats
    ```

- **Shopping Expenses Analysis**
  - Shopping expenses are analyzed by gender using a boxplot.

    ```python
    import matplotlib.pyplot as plt
    import seaborn as sns

    plt.figure(figsize=(10, 6))
    sns.set_theme(style="ticks")
    sns.boxplot(x='Gender', y='Purchase Amount (USD)', data=data)
    plt.title('Shopping expenses')
    plt.show()
    ```

- **Gender-based Purchase Statistics**
  - Additional statistics are provided based on gender.

    ```python
    data.groupby(["Gender"])["Purchase Amount (USD)"].describe()[["25%", "50%", "75%"]]
    ```

- **Purchase Analysis by Gender and Category**
  - Mean purchase amount is calculated for each gender and category.

    ```python
    data.groupby(["Gender", "Category"]).mean()["Purchase Amount (USD)"].round(decimals=2).unstack()
    ```

- **Purchase Analysis in Alaska**
  - Purchase analysis is performed for Alaskans, including purchases by category and season.

    ```python
    data_alaska = data[data["Location"] == "Alaska"]

    palette_color = sns.color_palette('bright')
    sns.countplot(x="Category", data=data, hue="Season")
    plt.title("Purchases made by Alaskans in a given season ")
    plt.ylabel("Count")
    plt.legend(loc='upper right')
    plt.show()
    ```

- **Favorite Accessories in Alaska**
  - A pie chart displays the favorite accessories of Alaskans.

    ```python
    colors = sns.color_palette('muted')[0:8]
    plt.pie(keys, labels=labels, colors=colors, autopct='%.0f%%')
    plt.show()
    ```

- **Payment Method Preferences**
  - A bar plot illustrates Americans' preferable payment methods by age category.

    ```python
    plt.figure().set_figwidth(18)
    sns.countplot(data=data, x="Payment Method", hue="Age Category")
    plt.title("Americans' preferable payment methods")
    plt.show()
    ```

- **Purchase Analysis for Selected Locations and Low Ratings**
  - Analysis is performed for the first 10 locations alphabetically and ratings less than or equal to 3.

    ```python
    first10Loc = data.sort_values(['Location']).Location.unique()[:10]

    data[(data.Location.isin(first10Loc)) & (data["Review Rating"] <= 3)].groupby(["Item Purchased"]).count().Age
    ```

- **Average Review Rating vs. Number of Purchases**
  - A line plot depicts the average review rating in relation to the number of previous purchases.

    ```python
    plt.figure(figsize=(10, 6))
    sns.color_palette('muted')
    plt.plot(data.groupby('Previous Purchases')['Review Rating'].mean(), marker='o', linestyle='-')

    # Adding labels and title
    plt.title('Average review rating to number of purchases')
    plt.xlabel('Number of previous purchases')
    plt.ylabel('Average review rating')

    # Displaying grid
    plt.grid(1)

    # Displaying the plot
    plt.show()
    ```
