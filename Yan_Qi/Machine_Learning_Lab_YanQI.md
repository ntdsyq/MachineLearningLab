# Machine Learning Lab

This lab is aimed to walk you through the complete workflow of a machine learning project; from data wrangling, exploratory data analysis (EDA), model training and model evaluation/comparison. 

You will work with your machine project teamates for this lab and your team needs to decide whether to use either R or Python as the main programming language. **Each team memeber needs to work on his/her own submission.**

We will use Github for team collaboration and it can be broken down into following steps:

1. The team leader creates a public Github repository under his/her account first.

2. All the other team members fork the repo so you will have a COPY of the repo under your account

3. Git clone YOUR OWN repo otherwise you won't be able to push later.

4. Create a subfolder under your name and finish your code. Push the changes to Github. *Note*: you might want to put csv file and system file in the gitignore.

5. Go to the Github page of YOUR OWN repository and click the "Pull Request" tab. You can find the details [here](https://help.github.com/articles/creating-a-pull-request-from-a-fork/)

6. Submit the pull request so you can see it under team leader's repository.

7. Pair review each other's code before merging it to the master branch.


**Homework**

- To sync all the changes made by your teammates to your own repository, follow the instructions under the "Keeping Your Fork Up to Date" section [here](https://gist.github.com/Chaser324/ce0505fbed06b947d962).

- To understand fork, pull request and branch better, review [this video](https://youtu.be/_NrSWLQsDL4) in 1.25X speed.


## Part I: Preprocessing and EDA

- The data comes from a global e-retailer company, including orders from 2012 to 2015. Import the **Orders** dataset and do some basic EDA. 
- For problem 1 to 3, we mainly focus on data cleaning and data visualizations. You can use all the packages that you are familiar with to conduct some plots and also provide **brief interpretations** about your findings.

### Problem 1: Dataset Import & Cleaning
Check **"Profit"** and **"Sales"** in the dataset, convert these two columns to numeric type. 


### Problem 2: Inventory Management
- Retailers that depend on seasonal shoppers have a particularly challenging job when it comes to inventory management. Your manager is making plans for next year's inventory.
- He wants you to answer the following questions:
    1. Is there any seasonal trend of inventory in the company?
       Yes, there is a seasonal trend of inventory. From the order quantity
       over 4 years, we observe similar trends from Jan to Dec. Sales generally increases from year beginning to year end. There are a few peaks, first in Jun, second in Sep, and last and biggest peak in Nov/Dec. Sales also increased over the years with 2015 sales higher than 2014 and 2014 higher than 2013 etc.

    2. Is the seasonal trend the same for different categories?
       Yes, similar seasonal trend exists for all three categories, as evident from the side by side linecharts. 

- ***Hint:*** For each order, it has an attribute called `Quantity` that indicates the number of product in the order. If an order contains more than one product, there will be multiple observations of the same order.


### Problem 3: Why did customers make returns?
- Your manager required you to give a brief report (**Plots + Interpretations**) on returned orders.

	1. How much profit did we lose due to returns each year?
	   We lost in each year due to returns:
	   2012: $17477.26
	   2013: $9269.89
	   2014: $17510.63
	   2015: $17112.97   

	2. How many customer returned more than once? more than 5 times?
       20 customers returned more than once. No one returned more than 5 times.

	3. Which regions are more likely to return orders?
	   The three regions with the highest likelihood to return orders along with the probability of return are:
	   Southern US: 5.4% 
	   Southern Europe: 5%
	   Eastern US: 4.9%


	4. Which categories (sub-categories) of products are more likely to be returned?
	   Most returns are from the Office Supplies category. Orders from each category has similar probabilities (4.2%) to be returned. See probabilities of return (# of returned orders / total # of orders) below:
	   Technology: 4.3%; Furniture: 4.3%; Office Supplies: 4.1%

	   Among all subcategories, the largest number of returns come from Storage, Art, Binders. The sub category with the highest probability of return are Labels (5.2%), Tables (4.6%) and Accessories (4.5%) 

- ***Hint:*** Merge the **Returns** dataframe with the **Orders** dataframe using `Order.ID`.


## Part II: Machine Learning and Business Use Case

Now your manager has a basic understanding of why customers returned orders. Next, he wants you to use machine learning to predict which orders are most likely to be returned. In this part, you will generate several features based on our previous findings and your manager's requirements.

### Problem 4: Feature Engineering
#### Step 1: Create the dependent variable
- First of all, we need to generate a categorical variable which indicates whether an order has been returned or not.
- ***Hint:*** the returned orders’ IDs are contained in the dataset “returns”


#### Step 2:
- Your manager believes that **how long it took the order to ship** would affect whether the customer would return it or not. 
- He wants you to generate a feature which can measure how long it takes the company to process each order.
- ***Hint:*** Process.Time = Ship.Date - Order.Date


#### Step 3:

- If a product has been returned before, it may be returned again. 
- Let us generate a feature indictes how many times the product has been returned before.
- If it never got returned, we just impute using 0.
- ***Hint:*** Group by different Product.ID


### Problem 5: Fitting Models

- You can use any binary classification method you have learned so far.
- Use 80/20 training and test splits to build your model. 
- Double check the column types before you fit the model.
- Only include useful features. i.e all the `ID`s should be excluded from your training set.
- Note that there are only less than 5% of the orders have been returned, so you should consider using the [createDataPartition](https://www.rdocumentation.org/packages/caret/versions/6.0-80/topics/createDataPartition) function from `caret` package and [StratifiedKfold](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html#sklearn-model-selection-stratifiedkfold) from sklearn when running cross-validation.
- Do forget to `set.seed()` before the spilt to make your result reproducible.
- **Note:** We are not looking for the best tuned model in the lab so don't spend too much time on grid search. Focus on model evaluation and the business use case of each model.


### Problem 6: Evaluating Models
- What is the best metric to evaluate your model. Is accuracy good for this case?
  Confusion matrix, precision and recall, are good metrics for evaluating the model. Classification accuracy is not a good metric. With the highly imbalanced classes, the accuracy can still be high even if the minority class is all misclassified. 

- Now you have multiple models, which one would you pick? 
  I will pick the model with higher recall. Here we are primarily interested in the model's ability to predict returns correctly (proportion of true returned predicted as returned. )

- Can you get any clue from the confusion matrix? What is the meaning of precision and recall in this case? Which one do you care the most? How will your model help the manager make decisions?
- **Note:** The last question is open-ended. Your answer could be completely different depending on your understanding of this business problem.

### Problem 7: Feature Engineering Revisit
- Is there anything wrong with the new feature we generated? How should we fix it?
- ***Hint***: For the real test set, we do not know it will get returned or not.
The feature "the number of times a product is returned" is based on the response variable we are trying to predict. 