# Customer Movement Analysis 

Status:
*New******-> Appeared in current month but not the previous month nor the month before  
*Repeat******-> Appeared in current month and the previous month  
*Reactivated******-> Appeared in current month not the previous month but did appeared the month before  
*Churn******-> Does not show up in current month but did show up in previous month  

## Dataset
Supermarket, date customer came in to purchase, customer code, and other data. 
For the Customer Movement Analysis only purchase date and customer code was used.

## Data Preparation
Used SQL to count customer in each status for each month in the dataset done using Google Big Query.

## Result
Visualization made using Google Data Studio which can import the data from Google Big Query after the preparation.
![customer movement graph](./customer movement graph.png)