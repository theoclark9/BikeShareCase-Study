# BikeShareCase-Study
(Important to Note: This project had a theoretical dataset and business task, as it was for a class I took at Pitt)

The city of Ourra has recently hired me as a Data Science Consultant for the department 
of transportation. A newly established bike sharing system, Drpia, is in need of accurate 
predictions in order to properly meet the supply for the demand of rental bikes in Ourra. This is 
where my skills are in demand, specifically my statistical learning methods and modeling 
techniques. The provided dataset for bike rental counts (train.csv) has 14 predictor variables,
(Date, Hour, Temperature, Humidity, Wind, Visibility, Dew, Solar, Rainfall, Snowfall, Seasons, 
Holiday, Functioning & ID). Given the dataset, the task was to predict for bike rental count on a new 
dataset with high accuracy. There were no missing data values in either dataset, but the 
‘Functioning’ variable was almost certainly an oddity of the data, as whenever it was set equal to 
‘No’ there were zero bike rental counts on those observations. This makes logical sense, as when 
the bikes are not available for rental, I would expect the rental count to be equal to zero. If I kept 
this variable in a particular model, there would be no way for the model to understand that the 
‘Functioning’ variable being equal to ‘No’ is the only variable that matters when the bike rental 
count is equal to zero. In order to avoid these observations negatively affecting the prediction
accuracy, I removed them from the train.csv dataset. Additionally, ‘ID’ and ‘Date’ were not very 
useful in modeling. ‘ID’ was not very useful and held no information for the bike rentals besides 
being a unique identifier, so I chose to remove this variable altogether from the data. I chose to 
convert the ‘Date’ variable into a variable that indicated whether the bike was rented on a 
weekend or weekday, called ‘Weekend’.

In general, the rental bike count had an average of about 730 bike rentals for a given day 
when the bikes were available for rental. Exactly what one might think, the average bike rental 
count on the weekend was about the same as the average count on weekdays. This result 
suggests that people use bike rentals not only for leisure, but for traveling, 
commuting, and other types of general transportation. Additionally, the most 
common time of day to rent a bike was 7:00am. This furthermore suggests that 
people use bike rentals for transportation, and possibly even more often than for 
leisure. Intuitively, there were more total rentals on warmer months of the year, as 
seen in figure 1 (on the left). After further analysis discussion, it will be even more 
obvious that temperature is one of the most important variables in predicting rental 
counts on a particular day. 

I explored several different approaches in order to find a model with a 
respectable predictive accuracy on the testing portion of the data. The predictive 
accuracy for each model was evaluated using mean squared error (MSE), using a 
partition (~75%) of the dataset as the testing set and the rest as the training set.
The results of each model is shown in figure 2 (on the right). When analyzing the 
results of the forward stepwise regression, generalized additive, ridge regression, 
lasso regression, and principal components models, the predictive accuracies were 
astonishingly similar to one another, all near a MSE of 191,000. The two decision 
tree models performed best in terms of predictive accuracy, by a large margin. The 
random forest model had a MSE of 36,099.47 while the bagging model had a MSE of 30,787.32. 
The bagging model edges out the random forest model in terms of predictive accuracy in this 
case, with a slightly lower mean squared error. This was the main reason I chose to use bagging 
decision trees as my final model, as it had the highest prediction accuracy.
To measure variable importance for the decision tree models, I prioritized the increase in 
MSE of the predictions (%IncMSE), which are estimated using out-of-bag cross-validation, as a 
result of one of the dependent variables being permuted. When evaluating variable importance in 
decision trees, %IncMSE is the more powerful and informative of a measure when compared to 
IncNodePurity. The largest %IncMSE for both the random forest and bagging models was 
‘Hour’, which represents the hour of the day. This information means that the time of day gives 
the best prediction and contributes most to the bagging and random forest models. The 
importance measure (%IncMSE) doesn't have a coefficient-style interpretation, and it is typically 
subjective to the model and the metrics to assess the model fit at each iteration. The bagging 
decision tree although, had ‘Temperature’ as the second most important variable that nearly 
approaches the %IncMSE measure for ‘Hour’, while in the random forest it is the fourth most 
important variable and is not close to ‘Hour” in %IncMSE measure. The least important variable
according to %IncMSE was ‘Snowfall’, in both the bagging and random forest models. Logically 
this makes sense, as the snowfall would most likely come in the winter season and certainly 
come with lower temperatures, two variables already included in the model.
Bootstrap aggregating (Bagging) does a remarkable job of reducing the variance of a 
statistical learning method, in this case for decision trees. While bagging was the model chosen 
for predictive purposes, there are a few downsides to the utilization bagging decision trees. There 
is no single tree with a specific set of rules/axioms in bagging and therefore it becomes unclear 
which variables are actually more important than others. This causes a blurred line in the 
evaluation of variable importance, which I previously discussed. One might not trust this 
variable importance in real-world applications, and therefore the random forest might be a better 
choice in this light.

The most challenging aspects of the bike rental dataset was the presentation of the ‘Date’ 
variable. In order to change this variable into a binary variable named ‘Weekend’ in R, it 
was necessary to use the lubridate package and its functions in order to transform it into a day of the 
week variable. Next, it was necessary to create an if else loop, within a for loop, in order to go 
through the entire data frame and label each observation as either a weekend or weekday. This 
was definitely a challenge, but creating a variable that proved to be useful in models and important 
within my final model was worth the challenge. If my job depended on this 
model, I would be relatively confident but not overly confident. In order to improve the final 
predictions, one could explore more types of decision trees, as this was clearly the right 
approach to finding the most accurate predictions. Boosted trees, bayesian 
additive regression trees, along with any other types of decision tree models could fit this scenario. 
The generalizability of this model is favorable, as it predicts with a low MSE on new data, as 
shown in the analysis. In terms of additional predictor variables, more weather variables could never hurt. 
I do believe this model has adequate predictive accuracy, but there is much room for improvement with 
various models and possible additional predictor variables to be added. Overall, it can be safely concluded 
that time and weather play important roles in the demand for bike rental, with emphasis on time of 
day and temperature.
