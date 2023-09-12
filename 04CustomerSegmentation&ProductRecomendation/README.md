# Customer Segmentation

## Definition
Customer segmentation is the process of dividing customers into groups based on shared characteristics and behaviors. By doing so, companies can better understand their customers' needs and tailor their marketing strategies to each specific group, resulting in more effective sales efforts. This targeted approach not only enhances customer engagement but also fosters increased loyalty and more meaningful interactions.

## Benefit
By segmenting customers, businesses can glean deeper insights into their audience, fine-tune their marketing and sales strategies, and pinpoint key customer segments that warrant further investment. Additionally, it assists in refining marketing techniques and creating effective action plans.

# Product Recommendation

## Definition
At its core, a product recommendation is a filtering mechanism designed to predict and display items a user might wish to purchase. While the system may not always hit the mark, it's deemed successful if it aligns with user preferences.

## Benefit

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

## 1. Customer Single View
The goal is to aggregate sales transaction data to formulate a customer single view. This will later aid in developing the customer clustering model.


## Data Transformation
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/0aabd07f-0390-4dd9-9981-f0b1fc74303a)

## Customer single view
![image](https://github.com/Hakulani/CustomerAnalytics/assets/61573397/5040420e-8c07-40aa-a429-2cb1068e4aa6)



