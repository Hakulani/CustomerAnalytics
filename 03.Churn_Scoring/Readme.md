# Churn Scoring 
## Customer Churn Analysis

**What is Customer Churn?**  
Customer churn refers to the scenario when customers cease their engagement with a service or product. The frequency of churn can be analyzed on different timelines - monthly, quarterly, or annually. This phenomenon can drastically impact a company's revenue streams, stymie growth, and escalate the costs related to customer acquisition.

**Importance of Customer Churn Scoring:**  
Customer Churn Scoring uses behavioral patterns to predict which customers are most likely to disengage. Such predictive insights empower businesses to intervene proactively, potentially preventing customers from severing their ties.

**Our Approach:**  
In our analysis, we employed a mix of analytical models and sampling techniques. The primary objective was to discern which combination of these methods yielded the most accurate predictions for our dataset.

**Case Study Insights:**  
The data foundation for our models is an e-commerce dataset comprised of 5,630 entries spread across 20 columns. The dataset encapsulates the following particulars:

## Dataset Overview
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/dd11bba8-93ce-4dd2-b115-51ec7ca0bcc8)

## Data Exploration
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/f5008e2e-de06-499a-a142-4e5a2bf4327e)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/39b43bc7-2f5c-4800-8fef-648fd833369e)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8a90b472-6b29-4bd3-9189-71a7d982449a)

## Data Transformation
- Drop the missing value out.
- Transform the category variable to numeric variable by using one-hot encoding technique.
- Perform standard scaling 

<code>df = df.dropna()</code>
