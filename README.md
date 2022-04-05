# Customer Survival - Churn Prediction

***

Churn rate is an essential metric to provides clarity on how well the business is retaining customers. An explicit customer churn represents the rate at which your customers are cancelling their subscriptions. On the other hand, an implicit churn is a common situation when it comes to retail selling. It's basically when the churner stops using the business or platform.


The purpose of this notebook is trying to predict users who will churn by checking previous historical data.

***

## Dataset

The dataset has 104103 entries and 3 columns : uuid, date and event. There aren't non-null values and data type for all columns is object. 
The column uuid has 1034 unique values and events has 3 categories : contact, small purchase and big purchase.

![Screen Shot 2022-04-04 at 21 42 35](https://user-images.githubusercontent.com/64446494/161656118-b375707f-6bc8-40e5-9536-ddd558d96f11.png)


***

## Datetime Information

As the date is stored as an object, it is necessary to change it to date-time format. Therefore, more info can be extracted from the date column, such as month, year, week, etc.

The plot of events by month-year can be seen in the picture below:

![output](https://user-images.githubusercontent.com/64446494/161657678-ec0b0a23-5292-430a-8d02-9f5e42482912.png)

