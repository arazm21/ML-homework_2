# <span style="color: red;"> homework2 - fraud detection </span>

IEEE-CIS Fraud Detection is a more advanced machine learning task compared to our last homework. this time, there are a few things that we have to worry about that did not pose a problem before.
1. the columns are hidden and rescaled. obviously, this is done to protect the privacy of the clients, however it poses quite a few problems for us. for example, it makes feature engineering far more difficult, as it becomes nearly impossible to make intuitive guesses about how to manipulate data. there are just a few columns that is understandable to the human eye.
2. the ratio of the target is heavily skewed towards 0, which means that we will have to find a way to balance it or risk bad results.
3. there is much more data, which allows us to make far better predictions, however this comes at the price of time and storage, which means that we can not brute force our way through this task.

## handling missing values
one way to take care of missing values is to drop columns containing too many missing values, but i am not going to do that. instead, i will do the following - i will change the given rows with a big number, and will try other ways too.

## feature engineering 
feature engineering in this case is particularly hard, because information is anonymised and hidden, but there are some things we can do.
1. we can add use the time column (TransactionDT) and make new columns to show weekday, month.

## encoding
I will use two ways to encode non-numerical values - if the amount of unique variables is above a threshold, I use WOE, otherwise I use OHE.

## sampling
because the data is so imbalanced, I had to do something about the sampling of the data, because the target tends to skew one way otherwise. I decided not to use oversampling, as it causes too muchoverfitting. we could also use metrics that work better with situations such as this, or change the threshold to find the right balance between finding too few and too many frauds.
and,of course, do undersampling, which might cause some issues with underfitting, but i will take ethat tradeoff.

## RFE

I used two different approaches in the reduction of the amount of features
1. I decided to use RFE, to get rid of most of the useless features. I am the same classifier as the one at the end of the pipeline in all notebooks. for time related issues, I had to make big steps each time, which might cause some useful features to be lost.
2. I used a correlation filter first, to get rid of a lot of rows fast. after that, I used RFE, however this time i could afford to make the step size a bit smaller.

## optimising 

everything that I talked about above was without me mentioning specific parameters. that is because used bayesian search to optimise the hyperparameters, both in the classes that I made but also the hyperparameters in the xgboost classifier.
I run cross validation, splitting the data into 3, also running 3 parallel pipelines, in the end finding out the best numbers. T thought of first using something like a random forest to find out my hyperparameters and then xgboost separately, in the end I decided to cram everything into one, finding everything at the same time. 
the reason I used bayesian search is that it is faster than grid search, as it does not check all combinations. I will run this only once and then use those numbers for all future fits (like in autoML). 
keep in mind that a lot of the numbers can be increased and while yield better resuts if done so, but because of time constraints I had no other choice.

## classifier

I have decided to try out the model on a few different classifiers, namely XGBoost, logistic regression, random forest, naive bayes.
the results will be visible in MLflow.

--------------------

despite these optimisation, I was running into an overfitting problem. while my train f1 was good, my validation was far off, therefore i changed a few parameters in my classifier.
I changed regularisaiton numbers, subsampling, max_depth, learning_rate.
------
    MLflow ზე უნდა შევქმნათ ექსპერიმენტი XGBoost_Training, რომლის შიგნით იქნება შესაბამისი run - ები: XGBoost_Cleaning, XGBoost_Feature_Selection და ა.შ.

i do not have separate runs, instead all of the information is in 1 single run, together. I did it because that's how i did it originally and did not have time to change it, however the info is there. (it might not be in xgboost, but you will find it in the other two experiments) 
in sumary, the end results were quite decent, even if the model went towards overfitting, the f1 score ended up around 0.77 on validation set, while roc-auc is 0.97