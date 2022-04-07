# Implicit Churn Prediction

***

Churn rate is an essential metric to provide clarity on how well the business is retaining customers. An explicit customer churn represents the rate at which your customers are cancelling their subscriptions. On the other hand, an implicit churn is a common situation when it comes to retail selling. It's basically when the churner stops using the business or platform. In some cases, the customers don't even know they are churning.


The purpose of this notebook is try to predict users who will churn by checking previous historical data. I hope this analysis help to increase the customer retention in your business! 🚀 

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

![Screen Shot 2022-04-07 at 11 18 44](https://user-images.githubusercontent.com/64446494/162220850-4c418e18-f422-48fb-9a41-d54796d8e12d.png)


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

The sampling strategy chosen for undersampling was 1/50.

![Screen Shot 2022-04-07 at 10 50 46](https://user-images.githubusercontent.com/64446494/162215022-a717f13e-1f75-4750-9f34-1da210efb5c4.png)

The sampling strategy chosen for oversample was 1/1.

![Screen Shot 2022-04-07 at 10 53 00](https://user-images.githubusercontent.com/64446494/162215317-a4bd0d28-728e-4ff7-ba4e-a1d76300cdef.png)

## Model Validation

For this notebook, 5 models were chosen for training and validation. 

![Screen Shot 2022-04-07 at 11 02 32](https://user-images.githubusercontent.com/64446494/162217521-ba926d5f-85ed-47a7-8b5e-ba75fcb90af9.png)

The model chosen will be the model with the best prediction score. 

![Screen Shot 2022-04-07 at 11 05 38](https://user-images.githubusercontent.com/64446494/162218069-ad89f6a1-111d-4369-ae40-a00812042cf5.png)

In this case, the best model is AdaBoost and in the picture below there is the confusion matrix using train and valid set:

![Screen Shot 2022-04-07 at 11 19 32](https://user-images.githubusercontent.com/64446494/162220964-7351ce20-f408-4a04-9938-e717014b0389.png)

## Results

Even with the usage of undersampling and oversampling techniques, there is a high amount of false positive due to dataset imbalance. 

Applying this model on test dataset, 111 in 719 users were classified as churner.

## References

- https://www.custify.com/blog/customer-churn-guide/
- http://didawiki.cli.di.unipi.it/lib/exe/fetch.php/dm/1.crm_churn_2015.pdf
- https://www.optimove.com/blog/how-to-perform-customer-survival-analysis#:~:text=Customer%20survival%20analysis%2C%20also%20known,customer%20acquisition%20and%20retention%20activities

Thanks for reading this study! Please, feel free to contribute this code 🤓
I intend to keep evolving a little bit every day! 🚀



