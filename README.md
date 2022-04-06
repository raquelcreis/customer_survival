# Customer Survival - Churn Prediction

***

Churn rate is an essential metric to provides clarity on how well the business is retaining customers. An explicit customer churn represents the rate at which your customers are cancelling their subscriptions. On the other hand, an implicit churn is a common situation when it comes to retail selling. It's basically when the churner stops using the business or platform. In some cases, the customer don't even know they are churning.


The purpose of this notebook is trying to predict users who will churn by checking previous historical data. I hope this analysis help to increase the customer retention in your business! 🚀 

So let's get started 🤓

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

***

## Cut Off Date

It is not explicit how long it takes for a customer to become a churner. To define this period of time of inactivity that determines if a customer is a churner or not, first we have to calculate the interval in days between events.

![output](https://user-images.githubusercontent.com/64446494/161770200-c0cf00bd-7fac-45ba-9e6c-1f0ef908281e.png)

Based on the values found in the picture above, we'll calculate for a range of intervals how many false positive will have for each value.

![Screen Shot 2022-04-05 at 10 59 30](https://user-images.githubusercontent.com/64446494/161770927-10f3f7c3-ca66-4735-9b75-e60e09b723e9.png)

For this dataset, we'll choose 90 days of inactivity to classify a customer as churner.

***

## Feature Engineering

- First, the column events was transformed categorical to numeric. 'contact' as 1, 'small_purchase' as 2 and 'big_purchase' as 3. 

- Created column 'eventNumber' to show the sequence of events per user.

- Created column 'numEventsMonth' to count events per month and user.

- Created column 'purchaseMonth' to count purchase events per month and user.

- Created column 'contactMonth' to count contact events per month and user.

- Created column 'numEventsLast7D' to count contact events on the last 7 days

## Churn Classification

Since there are many rows per user, we won't classify a user as churner but as a churner event, instead. 

![Screen Shot 2022-04-05 at 11 21 15](https://user-images.githubusercontent.com/64446494/161775501-b8686019-f243-427a-8ad1-2e4250016d6a.png)

If it is not the last event of the user, that means he/she did not churn in this date range (he/she returned to the platform, he/she an active user). The last events are the ones that we are not sure if they will be a churn event and this customer won't return. Therefore, all the events that are not the last events, will be classified as 0 (it is not a churn event). In constrast, the model will predict if the last event is a churn event or not. 
If a last event has a interval higher than the cut off date (90 days of inactivity), it will be classified as 1. If it is under 90 days, it will be classified as 'last'.


![Screen Shot 2022-04-05 at 12 46 22](https://user-images.githubusercontent.com/64446494/161793607-3423cdf8-808c-4b1f-b606-ad159188a409.png)

In this scenario, we have 103069 not churn events, 315 churn events and 719 events that we don't know how to classify yet. 


## Undersampling & Oversampling

The dataset was divided in train, validation and test sets. The test set will be the events that weren't classified (719). The rest of the dataset will be divided 30% for validation and 70% for train.

Since the dataset is unbalanced, we'll have to undersample the majority class and oversample the minority. 
