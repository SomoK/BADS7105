# Customer Segmentation

## Dataset
Supermarket, customer code, spending amount, quantity of goods, and other data. 
For the Customer Segmentation customer code, spending amount, quantity, and shopping hours count was used.

## Data Preparation
Used SQL to group the customers into 7 clusters based on TOTAL_VISIT, SPEND_PER_VISIT, SHOP_HOUR_PER_VISIT, QUANTITY_PER_VISIT the visualization was done using Google Big Query.

![customer segmentation](./Customer%20Segmentation.PNG)

### Group 1 
Is the largest group, having a relatively low number of visits, does not spend much nor purchase many items.

### Group 2 
Visit most often, spend a bit more than the first group.

### Group 3 
Similar to the first group but spent more time shopping without spending more.

### Group 4 
Does not visit often, spend a huge amount during each visit and does not spend a lot of time shopping.

### Group 5 
Visit once in a while, normal spending

### Group 6 
Similar to group five, likely to buy less expensive stuffs compare to group five

### Group 7 
Visit often and does not spend much, similar to group one but visit much often
