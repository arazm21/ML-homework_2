# <span style="color: red;"> homework2 - fraud detection </span>

## handling missing values
one way to take care of missing values is to drop columns containing too many missing values, but i am not going to do that. instead, i will do the following - 

## feature engineering 
feature engineering in this case is pparticularly hard, because information is anonymised and hidden, but there are some things we can do.
1. we can add use the time column (TransactionDT) and make new columns to show weekday, month.
2. we can aggregate the information of users by id to calculate their averages, which might help us see inconsistencies in their behavior.

