# Yulu Bike Sharing Demand Analysis 🚲

This project analyzes the bike-sharing dataset to understand the factors influencing rental demand. We perform exploratory data analysis (EDA), outlier treatment, and hypothesis testing to uncover patterns and relationships between variables.

---

## 📂 Dataset

The dataset used in this analysis is `bike_sharing.csv`, which contains hourly rental data for a bike-sharing system.

**Key Features:**
- **datetime**: Date and time of the observation
- **season**: Season (1:spring, 2:summer, 3:fall, 4:winter)
- **holiday**: Whether the day is a holiday or not
- **workingday**: Whether the day is a working day or not
- **weather**: Weather conditions (1:Clear, 2:Mist, 3:Light Snow/Rain, 4:Heavy Rain/Snow)
- **temp**: Temperature in Celsius
- **atemp**: "Feels like" temperature in Celsius
- **humidity**: Relative humidity
- **windspeed**: Wind speed
- **casual**: Number of non-registered user rentals
- **registered**: Number of registered user rentals
- **count**: Total number of rentals

---

## 🧹 Data Cleaning and Preprocessing

- **Duplicates**: Removed duplicate records to ensure data integrity.
- **Outliers**: Identified outliers in numerical features (`temp`, `atemp`, `humidity`, `windspeed`, `casual`, `registered`, `count`) using boxplots. Outliers were then removed using the Interquartile Range (IQR) method, resulting in a cleaner dataset for analysis.
  - **Before Outlier Removal**: `(10886, 14)`
  - **After Outlier Removal**: `(8645, 14)`

---

## 📊 Exploratory Data Analysis (EDA)

### Numerical Features

Histograms and distribution plots revealed the following:
- **Temperature (`temp` & `atemp`)**: Both are normally distributed and highly correlated, indicating that actual and "feels like" temperatures are very similar.
- **Humidity**: The distribution is skewed to the right, suggesting that most days have high humidity.
- **Windspeed**: The distribution is bimodal, with a large number of observations at lower wind speeds.
- **User Types (`casual` & `registered`)**: Both are right-skewed. The count of **registered** users is significantly higher than **casual** users.
- **Total Count (`count`)**: The total rental count is also right-skewed, indicating most days have a moderate number of rentals with a few high-demand days.

### Categorical Features

Countplots and pie charts showed:
- **Season**: The data is fairly evenly distributed across all four seasons, with **Fall** having the most records.
- **Holiday**: The vast majority of records are on non-holidays.
- **Working Day**: There are more working days than non-working days.
- **Weather**: Most rentals occur in **clear or partly cloudy weather (Type 1)**.

---

## 🤝 Correlation and Feature Selection

A correlation heatmap was generated to understand the relationships between variables.

- **`count`** has a strong positive correlation with **`registered`** users.
- **`temp`** and **`atemp`** are very highly correlated (close to 1.0).

To avoid multicollinearity, highly correlated features were removed.
- **`atemp`** was dropped due to its high correlation with `temp`.

---

## 🧪 Hypothesis Testing

We conducted several statistical tests to validate our hypotheses. The significance level (α) was set to **0.05**.

### 1. Is there a significant difference in bike rides between Weekdays and Weekends?

- **Null Hypothesis ($H_0$)**: The mean number of bike rides on weekdays is equal to the mean number of bike rides on weekends.
- **Test**: Independent Two-Sample T-test
- **Result**:
  - **t-statistic**: `-0.6213`
  - **p-value**: `0.5344`
- **Conclusion**: Since the p-value > 0.05, we **fail to reject the null hypothesis**. There is no statistically significant difference in the number of bike rentals between weekdays and weekends.

### 2. Is the demand for bicycles the same for different Weather conditions?

- **Null Hypothesis ($H_0$)**: The mean number of bike rentals is the same across all weather conditions.
- **Test**: One-Way ANOVA
- **Assumptions**:
    - **Normality**: Shapiro-Wilk test showed that the data is **not normally distributed** for each weather category (p < 0.05).
    - **Equality of Variances**: Levene's test showed that the variances are **not equal** (p < 0.05).
- **Result**:
  - **F-statistic**: `65.53`
  - **p-value**: `5.48e-42`
- **Conclusion**: Since the p-value < 0.05, we **reject the null hypothesis**. The demand for bicycles is significantly different across various weather conditions. *Despite the violation of assumptions, the ANOVA result is highly significant, indicating a strong relationship.*

### 3. Is the demand for bicycles the same for different Seasons?

- **Null Hypothesis ($H_0$)**: The mean number of bike rentals is the same across all seasons.
- **Test**: One-Way ANOVA
- **Assumptions**:
    - **Normality**: Shapiro-Wilk test showed that the data is **not normally distributed** for each season (p < 0.05).
    - **Equality of Variances**: Levene's test showed that the variances are **not equal** (p < 0.05).
- **Result**:
  - **F-statistic**: `236.95`
  - **p-value**: `6.16e-149`
- **Conclusion**: Since the p-value < 0.05, we **reject the null hypothesis**. The demand for bicycles is significantly different across the four seasons. *The strong result from the ANOVA test confirms this relationship, even with the violated assumptions.*

### 4. Are Weather conditions significantly different during different Seasons?

- **Null Hypothesis ($H_0$)**: There is no association between weather conditions and seasons.
- **Test**: Chi-Square Test of Independence
- **Result**:
  - **Chi-square statistic**: `49.16`
  - **p-value**: `1.55e-07`
- **Conclusion**: Since the p-value < 0.05, we **reject the null hypothesis**. There is a significant association between weather conditions and seasons.

---

## 💡 Key Insights & Recommendations

- **Weather is a Major Factor**: Bike rental demand is highest in clear weather and drops significantly as weather conditions worsen.
- **Seasonality Drives Demand**: Demand varies significantly by season, with fall and summer seeing the highest usage.
- **Registered Users are the Core**: Registered users form the backbone of the rental service, contributing the most to the total count.
- **Weekday vs. Weekend**: There is no significant difference in the average number of rentals between weekdays and weekends, suggesting a consistent user base throughout the week.
- **Operational Strategy**:
  - **Inventory Management**: Bike availability should be adjusted based on seasonal and weather forecasts.
  - **Marketing Campaigns**: Promotions could be targeted during seasons with lower demand (like winter) or aimed at converting casual riders to registered members.
  - **Weather Alerts**: Integrating weather alerts into the service's app could help manage user expectations and improve safety.
