# Customer Analysis

Within this repo we will find a full customer analysis encompassing:
- Splitting the data into cohorts.
- Retention analysis.
- Customer Lifetime Value Calculation and Analysis.
- RFM Analysis using Machine Learning.
- RFM Segmentation using Machine Learning.

## :: Exploratory Data Analysis

We have 9 month worth of data where each row represents one transaction. 
There is a total of 109.059 entries, however, after removing outliers, we will end up with 50.158 rows.

<p align="center">
  <img width="300" height="130" src="readme/data_overview.png">
</p>

Outliers will be removed, what will give us a total of rows. From left to right, a boxplot showing count and sum of transactions aggregated by user ID, and a histogram of the number of transactions aggregated by user ID. Most of the customers perfomed 10 or less transactions.

<p align="center">
  <img width="700" height="300" src="readme/outliers.png">
</p>


## :: Cohort and Retention Analysis

Cohorts are groups of customers that share some similar characteristics. When it comes to retention and churn analysis, we usually aggregate users by joining date. For example, users that joined in the same month will fall in the same cohort. 

<p align="center">
  <img width="700" height="350" src="readme/retention.png">
</p>

On the left we have the number of unique customers per cohort and on the right the retention cohort. Some conclusions are:
- 1. The number of users is declining month by month. 
- 2. First month (period) does not add up 100% this is due to the fact that there are users that didnt perform any purchase the month they joined.
- 3. Retention decreases fast in the first 3 months going from ~80% to ~15%.

## :: RFM Analysis with Machine Learning

RFM analysis is a marketing technique used to quantitatively rank and group customers based on the recency, frequency and monetary total of their recent transactions to identify the best customers and perform targeted marketing campaigns.

RFM analysis ranks each customer on the following factors:

- **Recency**  How recent was the customer's last purchase? Customers who recently made a purchase will still have the product on their mind and are more likely to purchase or use the product again. Businesses often measure recency in days. But, depending on the product, they may measure it in years, weeks or even hours.
- **Frequency** How often did this customer make a purchase in a given period? Customers who purchased once are often are more likely to purchase again. Additionally, first time customers may be good targets for follow-up advertising to convert them into more frequent customers.
- **Monetary Value** How much money did the customer spend in a given period? Customers who spend a lot of money are more likely to spend money in the future and have a high value to a business.
- **Other Metrics** You could add additional metrics to tailor the model and align it as much as possible to the business.

In our analysis, we create the following table:
- recency: When was the last purchase in days.
- frequency: Number of transactions. 
- price_sum: Addition of all transactions' amounts.
- price_mean: Average of all transactions' amounts.
- avg_time_buy: Average number of days between transactions.

<p align="center">
  <img width="400" height="175" src="readme/rfm_table.png">
</p>

### :: Machine Learning Model

This machine learning model attempts to understand the each user behavior, it wont be used to predict what the user will be doing next, but to understand what each customer should have done giving its past behaviour. 

Since the first months of the year is where we have most events, we will take January, February and March for training and the April as our test data.

The model is a Random Forest Classifier, gives a 90% accuracy for test data. 

We observe that recency is the most important feature while avg_time_buy does not add too much to the model.

<p align="center">
  <img width="500" height="350" src="readme/feature_importance.png">
</p>

Once we have the model trained, we use it to predict what should have happened in the fourth month. Lastly we compare the prediction `y_pred` with the reality `y_true`. This will help us to answer the previously stated business questions.

### :: Business Questions

**Which customers had a high probability to transact but didnt do anything in the next 30 days.**

We compare `y_pred` and `y_true`. In other words we check those users who should have transacted according to our model, but actually, stayed dormant. By creating a tailored marketing campaign we could get these users attention and enhance the engagement. 

<p align="center">
  <img width="550" height="80" src="readme/bq1.png">
</p>

**Which customers have recently transacted but are unlikely to do it again.**

By comparing `recency` with `y_pred` and `y_true` we check what users were likely to use the service but didn't, this information could be useful to understand what could be happening in the industry as well as what we should change to make us more appealing to customers.  

<p align="center">
  <img width="550" height="160" src="readme/bq2.png">
</p>

## :: RFM Segmentation with Machine Learning

blah

## :: Customer Lifetime Value

CLV or CLTV is the total profit generated from one client over their lifetime, after COGS. It is a metric highly used by subscription base companies such as Ecommerce or SaaS. It is cheaper to retain a customer than to acquire a new one.

This metric is often compared with CAC (Customer Acquisition Cost). LTV should always be higher than your CAC, otherwise you are losing money. Simply put, what you pay to acquaire a customer should be less than what the customer will bring in terms of revenue. 

The following data is used to compute LTV:
- Number of clients in the cohort.
- Number of orders per client.
- Number of transactions done by the cohort of clients.
- Average order value.
- Gross Margin Percentage: How much you make after COGS are deducted.
- Churn Rate: How many customers dont come back.
- Average Lifetime in months: 1 divided by churn rate.

### Churn Rate
The number of clients that leave after using your service. The lower the rate the better. It is cheaper to sell to already existing users than acquiring new ones, thats why churn rate is an important metric to keep track of.

The challenge when it comes to analyzing churn rate is what values to use, not the equation impletentation (it is a simple percentage difference). The values (or the churn definition) that will be used should make sense.

8% is usually the threshold when it starts to become a problem. However, this number highly depends on the sector and industry.

Things to keep in mind when computing churn rate:
- Do not mix different products. You should be computing churn rates per product.
- For seasonal products. Compare YoY instead of MoM.
- If possible compare customer churn rate to revenue churn rate. This could give additional insight regarding the direction of the company.

### Lifetime value analysis

Lifetime value calculation may vary from business to business but in general termns it would be something like: 

`Average Total Order Amount * Average Number of Purchases Per Timeframe * Retention Rate`. 

For our example, however, we will be using the following equation:

`CLTV = (Transactions * Avg Order Value * Gross Margin * Avg Lifetime) / Number of Customers`.

The excel file could be found in the data folder.

<p align="center">
  <img width="700" height="500" src="readme/excel_ltv.png">
</p>


