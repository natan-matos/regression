<img align="right" width="100" height="100" src="img/rossmann-logo.png"/>

# Rossmann Sales Prediction Project

Regression Project

## 1. Business Problem
Sales forecasting refers to the process of estimating demand for or sales of over a specific period of time. It can improve financial performance of any kind of business and in a major company as Rossmann it is particular import because any variation in demand could impact all stores at once. A accurate sales forecast could reduce stock and wast, as well as in-store availability issues.

Rossmann is one of the largests drug store chains in Europe owning over 3,000 stores across Germany, Poland, Hungary, the Czech Republic, Turkey, Albania, Kosovo. This problem set was coming from a Kaggle competition driven by Rossmann itself in 2015. The main goal was to predict each store daily sales for up to six weeks in advance. The dataset contains sales from 1115 stores in diferent days from 2013-01-01 to 2015-07-31, and other factors such as holidays, promotions and competion.

As part of "DS em Produção" course in "Comunidade DS" this problem was incremented with some more **fictional data** as is descripted below:

>Pretend you are a data scientist in Rossmann company. Suddenly a lot of messages pops up at your screen, reading all you figured 	out they all have the same demading: the store sales prediction for 6 weeks in advance. It seems all store managers were asked by the CFO of the company to have this prediction by the end of the month. You go talk to the CFO to undertand what is the real problem and if this prediction it is the bst solution. CFO tells you that Rossmann is going throught an expansion and needs to estimate accurately the amount of money they will loan from banks, they plan to use the part of the income from the next 6 weeks as part of the expansion investment. After that you agreed that the prediction makes sense and you can create a unique solution for all the stores. He went thrilled by the idea and also ask you to create a way the solution could be accessed by any store manager at any time and returns intantaneously the sales prediction for the store, so they can use in other occasions.

| Problem | Root Cause | Main Question |
| --- | --- | --- |
| How much money company need to loan | Company expansions | What will be the sales day by day in each store for next six weeks? |

## 2. Business Assumptions
- Stores with no competition distance information does not have nearby competitors, so we will consider a distance to high to be relevant.
- Some stores have a nearby competitor but do not have competition_since_month/year information. Since we consider the date when a new competitor is installed very important to this problem, we will consider this date the dale as sales date.
- Same consideration above for columns promo2_since_week/year.
- It was not been considered days with sales equal to 0 or store closed.
- We considered that customers it is a variable unavailable in the moment of prediction, so it was removed from dataset.


## 3. Solution Strategy
### 3.1. Final Product
- Report in a csv file with sales day by day for each store of the company.
- Telegram bot accessed by API

### 3.2. Tools
- Python, Jupyter Notebook, Render, Telegram, Git, API Flask.

### 3.3. Process
The solution process is based in CRISP-DM methodology, which stands for Cross Industry Process - Data Mining. It was developed by a consortium of over 200 interested organizations and it is flexible to suit many analytical methods such as Data Science.Launched in 1999 it is until today by far the most widely-used analytics process standard. It is originally composed by six phases, but the version used here in this project it is extended to ten.

<img src="img/CRISP-DS.png" style="zoom:100%;" />

* **Step 01:** Data description: clean data and use descriptive statistics to identify errors and unusual behaviours.
* **Step 02:** Feature engineering: derivate new attributes to better represent problem.
* **Step 03:** Variable filtering: remove unnecessary rows and columns and not available features.
* **Step 04:** Exploratory data analysis: find insights and understand the impact of variables on model learning.
* **Step 05:** Data preparation: prepare the data so that the Machine Learning models can learn the specific behavior.
* **Step 06:** Feature selection: selection of the most significant attributes for training the model.
* **Step 07:** Machine learning modelling: test different models, apply cross validation and compare real performance.
* **Step 08:** Hyperparameter fine tunning: choose the best values for each of the parameters of the model selected from the previous step.
* **Step 09:** Error evaluation and interpretation: convert the performance of the Machine Learning model into a business result.
* **Step 10:** Deploy model to production: publish the model in a cloud environment so that other people or services can use the results to improve the business decision.
    
# 4. Data Collect

- **Dataset was collected from Kaggle: [here](https://www.kaggle.com/competitions/rossmann-store-sales‘)**
    
	It contains sales for 1,115 Rossmann stores. Some stores in the dataset were temporarily closed for refurbishment.
    
- **The dataset has 19 attributes listed below:**

| Item | Description |
| --- | --- |
| id  | an Id that represents a (Store, Date) duple within the test set |
| store | a unique Id for each store |
| day_of_week | day of the week |
| date | date |
| sales | the turnover for any given day (this is what you are predicting)  |
| customers |  the number of customers on a given day |
| open | an indicator for whether the store was open: 0 = closed, 1 = open |
| promo | indicates whether a store is running a promo on that day |
| state_holiday | indicates a state holiday. Normally all stores, with few exceptions, are closed on state holidays. Note that all schools are closed on public holidays and weekends. a = public holiday, b = Easter holiday, c = Christmas, 0 = None |
| school_holiday |  indicates if the (Store, Date) was affected by the closure of public schools |
| store_type | differentiates between 4 different store models: a, b, c, d |
| assortment | describes an assortment level: a = basic, b = extra, c = extended |
| competition_distance | distance in meters to the nearest competitor store |
| competition_open_since_month | gives the approximate year and month of the time the nearest competitor was opened |
| competition_open_since_year | gives the approximate year and month of the time the nearest competitor was opened |
| promo2 | Promo2 is a continuing and consecutive promotion for some stores: 0 = store is not participating, 1 = store is participating |
| promo2_since_week | describes the year and calendar week when the store started participating in Promo2 |
| promo2_since_year | describes the year and calendar week when the store started participating in Promo2 |
| promo_interval | describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store |


- All rows missing competition_distance also missed competition_since_month/year
- Column 'is_promo' is created to tells if the day is in promo or not.
    
## 5. Top 5 Insights

### 5.1. Hypothesis Mindmap
A mindmap of hypothesis was created together with business team (marketing, saled, product) to create hypothesis about the problem. This step is important to look for important insights that will help find the best solution.

<img src="img/MindMapHypothesis.png" style="zoom:100%;" />

### 5.2. Insights

From all hypothesis, it will only be taken in consideration those who have data available. Thus, hypothesis about customers or location were not added to the analysis. A total of 10 hypothesis we evaluated and at the end we could validate 5 important insights.

| Insight 01 - Stores with the closest competitors sell more |
| --- |
| <img src="img/hyp01.png" style="zoom:60%;" /> |

| Insight 02 - Stores with long time competitors sell less | Insight 03 - Stores with longer time in promotion sell less |
| --- | --- |
| <img src="img/hyp02.png" style="zoom:60%;" /> | <img src="img/hyp03.png" style="zoom:60%;" /> |

| Insight 04 - Stores sell less with by the year | Insight 05 - Stores sell less in the second semester of the year. |
| --- | --- |
| <img src="img/hyp04.png" style="zoom:60%;" /> | <img src="img/hyp05.png" style="zoom:60%;" /> |


# 6. Machine Learning Model Applied

After modelling data using encoding and transformation, Boruta was used as a method of feature selection. The variables relevant to model according to Boruta were.

['store', 'promo', 'store_type', 'assortment', 'competition_distance', 'competition_open_since_month',
 'competition_open_since_year', 'promo2', 'promo2_since_week', 'promo2_since_year', 'competition_time_month',
 'promo_time_week', 'day_of_week_sin', 'day_of_week_cos', 'month_cos', 'day_sin', 'day_cos', 'week_of_year_cos']

We also add 'month_sin' and 'week_of_year_sin' since they are related to their cosseno variables pairs.

A total of five models were tested:
* Average
* Linear Regression
* Lasso
* Random Forest
* XGBoost

To calculated real performance, cross validation methos was used. Since is a time based problem, we could not separate validation and training parts randomly. Thus, the final six weeks of data were separate for test only, and the rest were used in cross validation. The K number is the number of parts the cross validation data is separated. Note that for every unit added in K number the previous validation data is added to training and a new six weeks of data is agregated for validation.

<img src="img/cross-validation.png" style="zoom:100%;" />

After cross validation, the real model performance could be observed as below:

<img src="img/comparison-algorithms.png" align="center" style="zoom:100%;" />

Note that the Average Model have better performance than linear models, indicating that phenomenon is complex. Performance for Random Forest is slightly better than XGBoost Regressor, but the model chosen was XGBoost. The reason for this is simple, Random Forest generated a much larger model and for now the gain in memory use is better than a slightly increase in performance.


# 7. Model Performance

The strategy chosen for fine tunning was Random Search, since this is the first CRISP cycle and we want to deliver a first version of solution as soon as posible. After five iterations the best set of parameters were found:

```
param_tuned = { 
		n_estimators': 3000,
		'eta': 0.03,
 		'max_depth': 5,
  		'subsample': 0.7,
   		'colsample_bytree': 0.7,
    		'min_child_weight': 3
    		}
```

Training the model again with the hyperparameters and cross validation we could find the final performance of trained model. This trained model was saved by pickle module to be send to production later.

<img src="img/tunned-performance.png" align="center" style="zoom:100%;" />

Plotting the feature importance, we can also observed what are the variables more important to the model.

<img src="img/feature-importance.png" align="center" style="zoom:100%;" />

# 8. Deployment
After validation by business team the model was send to production, which means the model need to be available to final user. Throughout an API called 'handler.py created with Flask module, the model saved in previous step and the Rossmann class are requested. Rossmann class is responsable for data preparation and transformation. Deployment arquitecture is represented below.

<img src="img/deploy-arquitecture.png" align="center" style="zoom:75%;" />

Below you can see the telegram app working:

<img src="img/telegram-app.gif" align="center" style="zoom:75%;" />

# 9. Business Results

Considering MAE the best metric to explain performance to business team in absolutes terms, we have calculated worst and best scenario by adding or subtracting MAE from prediction. MAPE is the MAE in percentagem that helps explain relative error.

<img src="img/sample-final-table.png" align="center" style="zoom:100%;" />

[See entire table here in csv](output/stores-prediction.csv)

As explained in the business statement part of sales for next 6 weeks will be use as investment in company expansion. Comparing sales from XGBoost model and the average model using test data we have:

|Total Sales Baseline Model | Total Sales XGB Model | Real Sales | Difference Baseline | Difference XGB model |
| --- | --- | --- | --- | --- |
| $276,978,801.43 | $286,922,284.67 | $289,571,750.00 | $12,592,948.57 | $2,649,466.00 |

Thus, the differente between the baseline model representing how sales would be calculated if the model did not exist and de model's prediction will be $9,943,482.57. Assuming all sales for the next 6 weeks would be used as the expansion investment, this will be the amount of money company will avoid to loan from the bank.


# 10. Conclusion
We can conclude that daily sales in Rossmann stores can be predicted by the model trained in this project and business team has validated the model to go to production. The phenomenon is time based and it was adapted for a forest regressor algorithm, the trained mode has achieved an 958.72 RMSE after cross validation and fine tuning. The total amount of money the project could save from loan if is implementeded would be $9,943,482.57. Some valuable insights can be further checked for other improvements, such as the extended promotions.

# 11. Next Steps

- Separate train and test data since from the very beginning of the project to avoid data leakage.
- Look for external data such as weather, national events, macro indicators and etc.
- Try to group stores by region.
- Adjust competition_open_since_year/month to a single date for each store.
- Try bayesian search method in fine tunning step.
- Add a daily chart together with the prevision in telegram app.


# 12. References

[https://www.kaggle.com/competitions/rossmann-store-sales](https://www.kaggle.com/competitions/rossmann-store-sales)

[https://www.theretailbulletin.com/retail-solutions/case-study-rossmann-successful-supply-chain-coronavirus-crisis-management-11-12-2020/](https://www.theretailbulletin.com/retail-solutions/case-study-rossmann-successful-supply-chain-coronavirus-crisis-management-11-12-2020/)

[https://www.forbes.com/sites/metabrown/2015/07/29/what-it-needs-to-know-about-the-data-mining-process/?sh=6fe236bb515f](https://www.forbes.com/sites/metabrown/2015/07/29/what-it-needs-to-know-about-the-data-mining-process/?sh=6fe236bb515f)

[https://www.kaggle.com/competitions/rossmann-store-sales/discussion/17229](https://www.kaggle.com/competitions/rossmann-store-sales/discussion/17229)
