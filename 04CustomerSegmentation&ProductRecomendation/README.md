# Customer Segmentation

## Definition
Customer segmentation is the process of dividing customers into groups based on shared characteristics and behaviors. By doing so, companies can better understand their customers' needs and tailor their marketing strategies to each specific group, resulting in more effective sales efforts. This targeted approach not only enhances customer engagement but also fosters increased loyalty and more meaningful interactions.

## Benefit
By segmenting customers, businesses can glean deeper insights into their audience, fine-tune their marketing and sales strategies, and pinpoint key customer segments that warrant further investment. Additionally, it assists in refining marketing techniques and creating effective action plans.



## Case Study
## Business Understanding
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5a335f92-a9c7-44b8-923a-02ce7a7ad17c)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/6a46eee9-e8b4-45ac-8536-b82c60eba946)

## Objective
Consider a distribution company that sells an array of health products and operates out of five service locations across Asia. To enhance their customer outreach efforts, bolster personalized transactions, and elevate brand loyalty and customer lifetime value, the company undertakes:

- **Customer segmentation**: Aiming to interact with their customers more effectively.
  
- **Product recommendation**: With the goal of increasing revenue and engagement through personalized product recommendation.
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/d29e4d58-8b98-4730-b30d-8e68758455c4)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5b9fb79b-c9d7-4894-b662-3a7d6eb8f9d9)

## data Understanding
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8a255154-bbc1-4a42-99b0-491bd5f991dd)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5d0dbf43-517e-445a-83b3-834ae4a191aa)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/813fd5a5-f452-4a3d-921f-6a930fada614)

# Sanity Check

## Sales Transaction

### Missing Value 
The dataset consists of a total of 2,406,316 rows. Columns such as `total_amt`, `product_json`, `discount`, and `paid_amt` contain null values.

**Clean Data Action**: Remove rows with null values in columns `total_amt` and `product_json`.

### Incomplete Dataset 
Upon reviewing the sales transaction tables, it's evident that data from Y2021 for the months M6 – M12 is missing.

**Clean Data Action**: Exclude data from Y2021 when preparing the dataset for model development.

### Special Data Characteristic 
The `product_json` column has a special format. It contains data in json format detailing the products and the quantity purchased in each transaction.

**Clean Data Action**: Parse this column to represent each product in its own column alongside the respective quantity.

### Incorrect Format 
The `payment_date` column contains data that is not in the correct date format.

**Clean Data Action**: Convert the data in this column to a proper date-time format.

### Lack of Essential Data 
The dataset lacks crucial information like the price per product and details about customer membership.

**Clean Data Action**: (You need to specify what action to be taken here.)

# Assumption

- Focus will be on the total amount rather than the paid amount. It's observed that only 50K members paid more than zero from a total of 578K members.
- Recommendations will be based on the data from the most recent two months in 2023 for products in each cluster.

# Data Transformation

 ![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/0aabd07f-0390-4dd9-9981-f0b1fc74303a)

## 1. Customer Single View
The goal is to aggregate sales transaction data to formulate a customer single view. This will later aid in developing the customer clustering model.

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5040420e-8c07-40aa-a429-2cb1068e4aa6)


# Social Network Analysis
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/cc6370d0-acd3-44a4-a67f-f2b0716edf63)


![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/012ebecb-6f4c-4b08-85f7-a827f3e1ae6e)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/910ff40e-6aa0-4ea7-b8cc-aabcb287c391)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/639aa819-9705-495d-9909-de259406d9bb)
 

# Next Degree of Top Sponsors Analysis:

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/527c6d2f-0412-43d1-80f9-fc95dd102434)


- **Visual Representation**: The concentration of nodes around each top sponsor in the network graph depicts their profound influence on network expansion and their effectiveness in recruitment.
- **Key Players**: These top sponsors emerge as the backbone of our network. They have been successful in onboarding the maximum number of members.  
- **Delving Deeper**: By exploring the nodes directly connected to our top sponsors (essentially their immediate recruits or the next degree), we gain insights into:
  - Efficient recruitment strategies
  - Effective member support mechanisms
  - Potential patterns in member selection
- **Emulating Success**: Analyzing the techniques and patterns of our top sponsors and their recruits allows us to discern successful strategies. By adopting and adapting these strategies, we can replicate their success throughout the network.
  - ![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/ef92bce1-d8ff-490d-a6dd-414c400a594c)
  - 
  - ![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/ebc331d6-f6db-46ce-b9d5-19c9a9eacc63)
  - 
  - ![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/08ac5fe4-2026-4780-8510-e7afa4eaeb51)


# Customer Segmentation using K-Means Clustering

Using the data from the customer single view, segment the customers.

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/103a5586-0846-4281-91ba-aec0e934990a)

## Feature Selection for Customer Segmentation:

1. **Total Amount**: Cumulative spending of each customer.
2. **Total Quantity**: Overall number of items purchased by each customer.
3. **Frequency**: Count of transactions made by each customer.
4. **Visit Period**: Duration between the first and last transaction dates for each customer.
5. **Mean Time Between Purchase**: Average time interval between successive purchases. It's calculated as the total time span from the start date to the last purchase date divided by the total number of transactions.
6. **Basket Size**: Represents the average spending for every item bought by a customer.
7. **Order Size**: Average spending for each order placed by a customer.
8. **Visit Size**: Average quantity of items bought in each transaction by a customer.
9. **CLTV (Customer Lifetime Value)**: Predicted net profit from a customer throughout their lifetime relationship with the business.
10. **Discount Amount**: Cumulative discount received by each customer.
11. **Visit Center**: Count of unique centers or locations visited by each customer.
12. **%Qty by Price Range**: Percentage distribution of items purchased by a customer across different price brackets. This feature provides insights into the customer's spending habits across various price ranges.
13. **Member**: Count of individuals or entities that are under the direct influence or referral of the customer (often referred to as downlines in a referral or MLM system).

These features provide a holistic view of customer behavior, spending patterns, and engagement with the business, making them crucial for effective segmentation.
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/70d5f793-bcce-4a67-90d9-1b0eb7a1af95)

## Determine Optimal Number of Clusters and Kmean clustering (1st)
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/3d33b617-9ae0-4938-ad53-f4db23c76dc3)
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/f8f53bc6-5207-485f-b38a-9a0b0c5b8146)
Customer Segmentation
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8a80ee55-4dd8-45de-9d96-87913bf5d1b6)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/ff1cde91-4b44-42e5-b219-c71b8c9b48f9)

Understanding customer behavior each cluster from  point of distribution  
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/1baf134f-5f53-4e7a-ac19-c990f4350f89)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/b0e1c864-e9b1-4bb9-9dff-b2778785cf4d)

Segmentation: Cluster 2 Analysis
K-means Methodology - 2nd
To gain more precise insights and distinguish sharper characteristics:

We reapplied the K-means clustering algorithm for the second time on our dataset. This repeated approach was implemented to extract finer details and identify more distinct groupings, enabling an enriched segmentation perspective.

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/647d8211-7382-4d1e-b3b4-02e8b6933d46)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/060cb591-30f0-40ca-82eb-dd3fcefcd9a4)

# Random Forest Classification for Cluster Prediction
This project focuses on leveraging the power of the Random Forest algorithm to predict data clusters and analyze cluster movement over different time frames.
## Overview
Random Forest is a renowned ensemble learning technique that constructs multiple decision trees during training and outputs the class that is the mode of the classes (classification) of the individual trees. By leveraging subsets of data observations and variables, the Random Forest model achieves enhanced accuracy and stability in its predictions.

In the context of this project, we've previously applied clustering to segment our data. The results from this clustering serve as input for our classification model. The overarching goal is to predict which cluster a new data point would belong to and to understand the migration or movement of these data points between clusters over time.

## Purpose

The main objectives of this project include:

1. **Predictive Cluster Analysis**: Utilize the Random Forest classifier to predict the cluster label for incoming or new data.
  
2. **Cluster Movement Analysis**: Analyze the transition of data points from one cluster to another over specific time intervals, providing insights into the evolving behavior and characteristics of the segmented groups.

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/a930ff72-734a-4d76-9dc3-f7a1b2fe3960)


# Product Recommendation

## Definition
At its core, a product recommendation is a filtering mechanism designed to predict and display items a user might wish to purchase. While the system may not always hit the mark, it's deemed successful if it aligns with user preferences.

## Benefit
- **Increased Sales and Revenue**
- **Enhanced Customer Engagement**
- **Improved Customer Retention**
- **Understanding and anticipating customer needs**


# Product Recommendation System

In the ever-evolving retail landscape, it is paramount to understand customer behavior and preferences in order to optimize sales. Our product recommendation system is designed to achieve this very goal by targeting a specific set of customers to enhance their shopping experience and, in turn, boost our sales.

## 🎯 Target Audience

**Customers purchasing a singular item.** 

These are individuals who, at the moment, opt for a single product. Our system will target these customers, aiming to introduce them to complementary or similar products that align with their purchase preferences and behaviors.

## 📈 Objective 

**Up-selling** 

Our primary goal is to up-sell by making tailored product recommendations. By doing so, we aim to:
- Increase the average cart value of our customers.
- Improve the overall customer experience by introducing them to products they might like.
- **Achieve a minimum of 10% increase in sales of Target Audience** by implementing the recommendation system effectively.
- 
## 🚀 Strategy

**Product Recommendation to Target Audience**

Our approach is to leverage data-driven insights and machine learning models to understand our customers better and recommend products that are most relevant to them. The strategy involves:
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/4638222c-3b85-48e2-917b-c88111ff3aed)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/8a6d8443-a36a-490d-93cf-80cf78d89859)

## Market Basket Analysis — Association Rules 

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/a767e550-a324-468a-8cd0-928ab58c938c)

Collaborative Filtering

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/f07da264-633c-4811-af74-49688d4b2582)

![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/a26d6260-121b-4d19-9d48-40ddab774a33)


The latent space : user & item
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/598585f4-d68b-46da-b5ce-c4c6498c86df)

## Business Impact of Product Recommendation

In today's highly competitive retail landscape, maximizing sales and enhancing the shopping experience are paramount. Product recommendations can significantly contribute to achieving these goals, primarily by guiding customers towards making additional purchases based on their preferences.
## 🎯 Objective
**Boost sales by at least 10%.** Our primary focus is customers who are purchasing only a single item. By recommending complementary or similar products, we aim to enhance their shopping journey, encouraging them to consider additional purchases.
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/835fade8-9058-4cc9-8385-c0b7a2e21d26)




