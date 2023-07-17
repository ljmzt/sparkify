# Sparkify

### Purpose
This is the capstone project for Udacity Data Scientist Nanodegree. The purpose is to predict whether a user will "churn" or not. "churn" here is defined as users that confirm cancellation. The dataset simulates a song streaming service and is about 12GB. The accompanying medium post is [here](https://medium.com/@mazhitu/sparkify-dataset-to-churn-or-not-bbe7dd868f80). 

### My Plan
1. The dataset is too large to hold on one laptop. But I have two laptops at home so I set up a spark environment on these two machines to perform ETL task. The output is converted back to a pandas dataframe and is only ~15Mb. See spark-setup.md for details about how to set up a spark environment.

2. After doing ETL, I then perform standard data cleaning, preprocessing, EDA, feature engineering and modeling with this dataframe, which can be done on a single machine.

### Main Files for Grading
1. ETL-pyspark-full_dataset.ipynb: The notebook that performs ETL task using pyspark.

2. modeling-final-full_dataset-new-no_state.ipynb: The final notebook to construct the model.

### Other Files
1. EDA-2-pyspark-larger_dataset.ipynb: EDA notebook using 10% randomly selected user

2. modeling-explore-full_dataset.ipynb: Explore models using the full dataset.

3. shap-analysis.ipynb: Use shap to do some feature importance analysis. 

### Some Technical Notes
1. Things related to spark is in spark-setup.md. I also tried to do the so-called pandas-on-spark, see test-spark2.ipynb. But there are too many unexpected outcomes so I essentially give up and stick with native spark-sql.

2. The NaN userId is '' in the mini dataset but 1261737 in the full dataset. Both mean the users are logged out and not actual NaN.

3. In pyspark, NaN and Null are different.

4. The state feature seems important if I just calculate the mean churn rate for each state. But it doesn't improve the fit at all when building machine learning models, which is somewhat strange. Adding it also makes the training slow, so at the end of day, I give up on it.

5. In terms of feature engineering, please see EDA-2-pyspark-larger_dataset.ipynb for all the details.

### Summaries of Results
I finally obtain an f1-score of about 0.85 for Randomforest and Xgboost models. ~0.80-0.82 for logistic regression and svc. Stacking doesn't make it higher in this specific dataset. In terms of business insight, it seems it's better to do something with new users (with small max_ts, total_time), paid users (paid_level=1), users with more roll_advert, downgrade and thumbs down actions.

### Libraries used 
* python==3.9.7
* seaborn==0.12.2
* pyspark==3.4.1
* pandas==2.0.3
* matplotlib==3.7.1 
* scikit-learn==1.3.0 
* scipy==1.11.1
* numpy==1.24.4
