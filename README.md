# Project 2 - Rise Realty Consultation--Ames Housing Data (and Kaggle Challenge)

### Introduction

For this project I have designed a scenario where I have been hired by a real estate company, Rise Realty, to assist in maximizing profits. Rise is a moderately sized brokerage with roughly 10-12 agents and would like data supported insight on how to best maximize profit potential. While Rise does have a lengthy database of potential clients, all they have is different metrics on the property without a sale price (the kaggle test set). They recognize they do not have endless resources available so have contacted me with hopes of receiving recommendations on what areas, Neighborhoods specifically, should their agents and office focus their energy towards. 

### Problem Statement

In order to keep our project properly structured and organized, let's create a succinct problem statement for our ultimate goal. We know that at the end of the project we want to be able to recommend to Rise which neighborhoods provides the best business opportunites for them. So let's format our problem statement as follows: **What is the correlation between neighborhoods and the potential earning opportunities for Rise and their agents? Is there a neighborhood that best provides potential sales opportunities?**

### Problem Solving Methods

In order to be able to accurately provide insight to Rise, I will initially first have to create a regression model that can offer reasonable predictions of property sale price for their client data. This will be the bulk of the project as we will see. Throughout the notebooks I will detail each step of the way the steps involved in the model building process. Once a sufficient model has beeen created, I will then apply that model to Rise's client database, predict the sales price, and then perform further analysis with this new information. I will provide a breakdown of why I recommend the neighborhoods that I do and the specific thinking that is best suited for Rise and their goal of profit maximization. 

### Data Dictionary

As always we want to be familiar with our data and what it is telling us. The best way to do this is to read through the data dictionary. Since there are an extensive amounts of columns within our dataset we have been provided the following link as our reference: http://jse.amstat.org/v19n3/decock/DataDocumentation.txt **Make sure to read carefully and note the differences in column types and what they represent!**

### Cleaning and EDA

Cleaning the data required us to initially check for any null or missing data. We did encounter quite a bit of Null values on the surface, but after consulting our data dictionary, we saw that these Null values were actually intended to just represnt a "No feature" entry. (ex: NA in Pool QC simpley meant there was No Pool on the proerty). This allowed us to simply replace the Null value with a string 'None' to relay to us the correct category it belonged to. The next null challenge was numerical based, while we couldn't make too many inferences from the data dictionary, I figured it was best to play it moderately safe and impute the Null values with the column means respectively. 

Next step in this process was identifying any outliers. It's easiest to do this through .describe summaries as well as boxplot visualizations. We were most interested in price so started there and uncovered outliers present for the sale price for some homes, but were able to convince ourselves these data points should remain as they more accurately represent the housing data across the varying counties instead of if we had removed them. If we remove them it would be very difficult to create an accurate model that would correlate across different counties where home prices are quite variable.

From our initial scan and cleanup, it was clear that given this extensive data set and meaningful data points, we should be able to effectively answer our original problem statement. But we will have to determine first what features we will want to use to create the model in order to be able to provide Rise an answer to their problem. After we have our predicted prices and then conducted further analysis I am confident we can fully provide insightful recommendations to Rise Reality and assist in maximizing profits.
 

### Preprocessing and Feature Engineering

This stage was the first steps in determing our X's and our y's. For y it was easy, we knew we wanted to sale price as our target. But for X it required different techniques that would help us arrive at a handful of variables that made sense. Using seaborns heatmap we were able to isolate our sales price and see how the numerical columns correlated with price. We elected to take roughyl the top 8 or so from the list as our starting point. To avoid any collinearity issues we made sure to run pairplots for these variables and identified through these charts any variables deemed collinear which we know we can remove (one of them). Don't forget to perform the LINE assumptions for your variables as well! 

Next was creating interaction terms. This is easily done with PolyFeatures and as shown in the notebook can be done with poly.fit method. After this step we wanted to include the Neighborhood column into our features as we know that is one thing we believe to greatly affect price: "location, location, location". In order to do this we have to utilized one-hot encoding and the pd.get_dummies, which creates corresponding columns for each of the categorical options within the column. Then by using a 1 in the necessary column and 0s in the other columns, we can determine numerically how the neighborhood column will fit into our model selection process.

Now we are all set up to perform train test split which will allow us to fit our model onto the train set and then test it out on the test set. Doing so allows us to evaluate what sort of issues arise within our model. Most sensible way is to compare the r^2 scores (represent the degree/percent of variability of our target that is explained by our model). Doing so allows us to see if of model suffers from high bias (bad model, low scores all around) or more high variance issues (overfitted to train set, and won't respond well to new data). 

Last step in this stage was to scaled our data using StandardScaler. This method utilizies converting our columns into their appropriate z scores (${x- \mu \over \sigma}$) Now our data units will be in standard deviations away from the mean. So in order to do this we must instantiate StandardScalar, then allow it fit to our X trains and X tests in order to learn their means and std deviations.

### Model Tuning 

Model tuning gives us an opporunity to evaluate the features that we have selected/created based upon our EDA and preprocessing stage and determine if there are ways for us to improve upon our model. In the notebook we will cover different hyperparamter tuning methods and how they impact our model. We will utilize data regularization through Ridge and Lasso techniques aim to help us avoid overfitting (we'll talk about that in a bit) while fitting our model by adding in a penalty term that will effectively shrink our regression coefficients to make the model simpler. As our lecture points out "We are accepting more bias in exchange for decreased variance." Lasso does everything that Ridge can do but also includes feature selection, so will penalize less effective variable coefficients simultaenous to the regularization

After applying all the different regression techniques, we were able to decide that RidgeCV provided us the 'happiest medium' of a blend of not being overfit (low variance), but also being effective at predicting price (low bias). 

### Applying our Model to Client Data

Once we arrived at our sufficient model, we applied it to our client data (kaggle test) in order to generate predictions for our unknown sales price. We also confirmed through Root-mean squared error that our model outperformed the baseline model **always important**. Now with our predictions in hand we mapped it correspondingly to our original client dataframe and performed various analysis looking at average sale price by neighborhood, total sale price opportunities for a given neighborhood (sum of all predictions for that neighborhood), as well as the actual number of sale opportunities (count) for each neighborhood. Using this data we were able to provide the following recommendation to Rise Realty:

### Conclusions/Recommendations

My recommendation for Rise Realty is to dispatch their agents' efforts in the Neighborhood from the top 5 average sale price list that occurs first in the total sale price opportunity result. This way we will feel confident that we are chasing down the best opportunities while also not limiting ourselves to much smaller opportunity windows. 

I advise Rise Realty to focus the bulk of their efforts primarily on their Northridge Heights clients. Based on our data set and sales predictions, Northridge Heights is a harmonious opportunity of high commissions coupled with sizeable enough number of properties to sell. The next best neighborhood would be the closeby Northridge community, while it presents fewer opportunities, for a small-medium sized boutique such as Rise, it will be most worth their efforts. 

### Link to Presentation
https://drive.google.com/file/d/1tT-ZZ7NNFZZUzDBXmDGgDkbJ6h1kPNmB/view?usp=sharing

### Bonus!
Submit your model predictions (client data set predictions) to the class kaggle competition. Results are ranked based on the lowest root mean squared error!
