# GridLock: Understanding the Factors Behind Long Outages

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
# Data Cleaning
The raw dataset contained information gathered over a large period of time and geographic locations. As a result, there were a few cleaning steps necessary to make sure the data could be analyzed later on. First, I had to deal with missing values for the outage duration column. 

 <iframe
 src="assets/missing_outage_durations.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
---

## Framing a Prediction Problem


---



---

## Baseline Model

CHANGE THIS:

For my final model predicting outage duration, I selected and engineered a total of 14 features. I kept basic information such as MONTH, CAUSE.CATEGORY, U.S._STATE, NERC.REGION, CUSTOMERS.AFFECTED, and DEMAND.LOSS.MW, as they provide important context about the timing, location, and cause of the outages.

I also engineered two features: START_HOUR, extracted from the outage start time to capture patterns by time of day, and IS_SUMMER, a binary indicator for whether the outage occurred in summer months (June–August), when outages are often longer due to heat-related demand stress.

Additional features were created to improve model performance, including LOG_CUSTOMERS_AFFECTED (a log transformation to address skewness), and CUST_AFFECTED_PCT_POP (proportion of population impacted), providing better scaling across different states.

I also included RES.PRICE, COM.PRICE, and IND.PRICE, reflecting economic costs of outages, and POPULATION, capturing how heavily populated areas might experience different outage dynamics. These features were selected based on domain knowledge and observed correlations during exploratory data analysis.
---

## Final Model

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       	Monthly electricity price in the commercial sector (cents/kilowatt-hour) |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---