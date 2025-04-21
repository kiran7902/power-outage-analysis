# GridLock: Understanding the Factors Behind Long Outages

by Kiran Chandrasekaran (kiran7902@gmail.com)

---

## Introduction

Need to finish this part

---

## Cleaning and EDA

---

## Framing a Prediction Problem


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