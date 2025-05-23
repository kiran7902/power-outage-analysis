# GridLock: Predicting Power Outage Durations
by Kiran Chandrasekaran (kirancha@umich.edu)
---

## Introduction

For this project, I looked at a power outages dataset from Purdue University that looks at data from January 2000 to July 2016. This dataset contains 1476 rows and 56 columns. First, I will clean the data and perform exploratory analysis. Next, I will focus on answering this question: 

What factors are most associated with the duration of power outages, and can we accurately predict outage duration based on those factors?

I picked this question because understanding what drives longer outage durations is important for the people affected, as well as utility companies and policy makers. Quicker restorations reduce economic losses and also improve life for those affected. 

In order to do this analysis, I focused on the following columns: 

| Column Name             | Description |
|:------------------------|:------------|
| CUSTOMERS.AFFECTED       | Number of customers affected by the outage |
| RES.PRICE                | Monthly electricity price in the residential sector (cents/kilowatt-hour) |
| COM.PRICE                | Monthly electricity price in the commercial sector (cents/kilowatt-hour) |
| IND.PRICE                | Monthly electricity price in the industrial sector (cents/kilowatt-hour) |
| POPULATION               | Population in the U.S. state in the given year |
| MONTH                    | Month when the outage started (1 = January, 12 = December) |
| CAUSE.CATEGORY           | Broad category describing the primary cause of the outage (e.g., Weather, Equipment Failure) |
| U.S._STATE               | U.S. state where the outage occurred |
| NERC.REGION              | NERC (North American Electric Reliability Corporation) region of the outage |
| DEMAND.LOSS.MW           | Estimated electric demand loss due to the outage (in megawatts) |
| START_HOUR               | Hour of the day when the outage started (0–23) |
| IS_SUMMER                | Binary indicator if the outage occurred during summer months (June–August) |
| LOG_CUSTOMERS_AFFECTED   | Natural logarithm of customers affected (to reduce skewness) |
| CUST_AFFECTED_PCT_POP    | Percentage of the state’s population that was affected by the outage |
| OUTAGE.DURATION          | Duration of outage events (in minutes) |

---

## Data Cleaning and Exploratory Data Analysis
#### Data Cleaning
The raw dataset contained information gathered over a large period of time and geographic locations. As a result, there were a few cleaning steps necessary to make sure the data could be analyzed later on. First, I had to deal with missing values for the outage duration column. Below is a graph of all missing data amounts for outage durations:

 <iframe
 src="assets/missing_outage_durations.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
---

You can see both the percent missing and the actual number missing by hovering over each bar. For example, California has 12 missing values but only 5.7% missing values. I decided to drop all rows with missing values since it was a very small percentage of total values and it wouldn't make sense to impute random durations with these rows. Furthermore, I decided to drop all rows with values of 0 for outage duration since this isn't really possible and these are probaly just missing values. 

As far as other missing column data goes, I checked all the columns that had missing values and saw that the only columns that were missing values that I planned on using were the price columns, all of which were missing 12 values. Since this is such a low number, I decided to drop all of these rows. These also turned out to be the same 12 rows, so I just dropped 12 rows. 

Next, I combined the date and time columns into timestamp columns for both start and restoration times using the pd.to_datetime function. 


Next, I dropped all non-relevant columns from the dataset and only kept the following columns: 'CAUSE.CATEGORY', 'CLIMATE.CATEGORY', 'OUTAGE.START.DT', 'OUTAGE.END.DT', 'CUSTOMERS.AFFECTED', 'TOTAL.PRICE', 'RES.PRICE',  'COM.PRICE',  'IND.PRICE', 'POPULATION', and 'OUTAGE.DURATION'. After this, the dataframe looked like this: 

 <iframe
 src="assets/outages_cleaned_df.html"
 width="800"
 height="400"
 frameborder="0"
 ></iframe>

#### Univariate Analysis

 First, I created a histogram of only outage durations to examine how outages varied in length. I found that most are very short, but there are some outliers that lasted much longer. The longest power outage in the database lasted over 75 days. 

 <iframe
 src="assets/distribution_of_outage_durations.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


#### Bivariate Analysis

 Next, I looked at outage duration by the price of electricity. I found that on average, the more expensive electricity was, the shorter the outages were. 

 <iframe
 src="assets/price_bins_and_duration.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 For another bivariate analysis, I looked at outage durations by cause categories. I found that severe weather and fuel supply warnings led to the most severe outages by length. This makes sense as without fuel it would hard to resolve issues, and severe weather can make it hard to get power back if the damage is very severe. 

 <iframe
 src="assets/outage_duration_by_category.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>


#### Interesting Aggregates

<iframe
 src="assets/state_data_1.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 I chose to look at data on a state by state basis. I aggregated all data for each state and then looked at numbers in 3 colums: total outage count, average outage duration, and average customers affected. This was interesting to look at, and was definitely more impactful for some states with more data than others. For example, the average outage duration in Florida was higher than most others states, which I would guess is because of the severe weather there like hurricanes. Michigan and Wisconsin also had very high average durations, which is probably both due to weather as well as more rural areas with worse repair times.

#### Imputation

 I decided not to impute values because there was such a small percentage of missing values within the columns I was most interested in. Instead, I just decided to drop rows with missing data and work with rows that had data. I discussed more of the specifics of missing values above in the 'OUTAGE.DURATION' column. 

 ---

# Framing a Prediction Problem

In this project, I am working on a regression problem. Specifically, I am trying to predict the duration of pwoer outages (in minutes) based on various factors. These factors are all known at the time an outage starts, including price of electiricity in the area, which state the outage is occuring in and its population, the time the outage is happening. Customers affected would be hardest to find out, but is still known at the time of an outage as the company knows how many houses/apartments lost power and can estimate how many people are actually affected. 

I chose outage duration because it is very important to predict as greater outages usually lead to public safety concerns as well as economic losses. To evalulate performance of my model, I am using Root Mean Squared Error (RMSE) as the primary metric. I chose this because it penalizes larger error more heavily and in this case small errors are much better than very large errors (I wouldn't want to predict an outage of 10 hours when it only lasts 10 minutes). I am also going to use R^2 value which will also tell me how well of a predictor my model is (closer to 1 is better).

---


# Baseline Model

For my baseline model, I built a linear regression model that used features that were more straightforward. After initially trying to predict raw outage duration wiht my model and being unsuccessful (R^2 < 0.01), I decided to log transform outage duration to see if that would improve my model. I did this because there are some huge outliers within outage duration that completely throw off the model. The RMSE was 2.122 log-minutes, which means that on average our model is off by e^2.1 or 8.17 times. This is not great, because it means that if the outage duration was 100 minutes, this model may predict anywhere between 12 minutes and 800 minutes. 

After training the model, I measured its performance using two metrics: R^2 and RMSE. The R^2 score was very low at 0.038 which means that only about 4% of the variation which is not a good result. 


<iframe
 src="assets/baseline_model_scatter.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 This scatter plot shows that the scatter plot leaves a lot to be desired as many of the actual values are very far from the predicted values. 

---

# Final Model

To build my final model, I expanded on my baseline linear regression model by engineering new features and tuning hyperparameters to improve prediction accuracy. First, I created two new features. First, I created 'CUSTOMERS_AFFECTED_per_POPULATION' which I calculated by dividing the number of customers affected by the population of a given state. Next, I also created a new 'TOTAL.PRICE' feature that was the average of industrial, residential, and commercial prices. This was different than the existing 'TOTAL.PRICE' feature that was just the average of the entire area, not the average of these three rates. 

Similar to the baseline model, I once again applied a log transformation to the target variable ('OUTAGE.DURATION') because it is highly skewed. This stabilized variance and made the relationship between the predicting variables and the target more linear, which is important for Ridge Regression. For preprocessing, I built a pipeline that log-transformed the numerical features, scaled tem with a StandardScaler, and one-hot encoded the categorial feature 'CAUSE.CATEGORY'. 

For hyperparameter tuning, I used GridSearch CV to search over different values of Ridge regularation strength. After tuning, the best alpha was found to be 0.1. 

<iframe
 src="assets/final_model_scatter.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

 This scatter plot shows much improvement over the baseline, as predicted log-durations are more often closer to the actual durations. 

This final model achieved an R^2 score of 0.401 and a RMSE of 1.665 in log-minutes which was lower than the baseline model that was above 2 log-minutes. This R^2 score means that our model explains baout 40% of the variation in the log-transformed outage durations. This is a meaninful improvement over the baseline model that had an R^2 relatively close to 0. Although this model is far from perfect, it is still much better than the original baseline model for predicting durations based on customer impact, energy prices, population size, and outage causes. 

---






















