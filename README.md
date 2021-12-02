# COVID Access to Care
An analysis of COVID-19s impact on access to medical care for different groups in the United States. Using the CDC's Indicators of Reduced Access to Care Due to the Coronavirus dataset we will determine the most affected patient populations and predict when access to care is expected to return to normal. 

------------------

### Dataset
The CDC's Indicators of Reduced Access to Care Due to the Coronavirus dataset is a ongoing sort survey aimed at assessing different demographics to facilitate rapid responses from federal agencies.

[Indicators of Reduced Access to Care Due to the Coronavirus](https://data.cdc.gov/NCHS/Indicators-of-Reduced-Access-to-Care-Due-to-the-Co/xb3p-q62w)

------------------

### Exploratory Data Analysis
The CDC data consisted of a number of survays carried out in 2020 and 2021, each approximatly 2 weeks appart. The survays look at the social and economic impacts of Covid-19, specifically how COVID-19 delayed or prevented people from seeking medical care. The survay looked at 70 different socioeconomic and regional groups and provided a value for reduced access to care as well as a confidence interval. 

The data was well structed but did require some cleaning and orginization which can be found in the EDA notebook. 

### Most affected patient populations
The first question tackled was finding the most affected patient populations. To do this I first started with a visual analysis, by simply plotting the Box Plots for each subgroup I hoped to find some groups that clearly stood out. 

![Alt text](Images/BoxPlotsNonStates.png?raw=true "Box Plots for Non State Subgroups")

While their were a few that seemed like their were above or below the average nothing stood out as definitive. Next I looked into incorperating the confidence interval as part of this analysis. Plotting them over the time period proved enlightening. 

![Alt text](Images/CAvUTciRange.png?raw=true "CA vs UT CI range")

These plots showed why the Box plots failed to stand out. The downward trend means the quartile range of nearly every subgroup will overlap. However plotting the CI ranges did show another way to approach this problem. By using a two sample dependant t test we could leverage the CI to determine which subgroups with significantly above or below the national average. This also proved problematic as the data set didn't provide the standard deviation or the sample populations making the t test analysis impossible. 

![Alt text](Images/MalevFemaleciRange.png?raw=true "Male vs Female CI range") 

Instead I decided to simply count where the CI ranges did not overlap with the CI range for the national average. Even without being able to perform the t test it's a safe to assume the means of two groups are differnt if they CI ranges do not overlap. 

![Alt text](Images/WeekCountNonState.png?raw=true "Weeks above or below the national average") 

This gave a clear picture of which groups where most and least impacted by COVID-19. For a full analysis see the Visualization notebook. 

### Predicting a return to normal
The second question was predicting when access to care would return to normal. For this we turn to time series modeling, however with 70 subgroups and only 24 datapoints we simply had too many features to leverage in modeling. Also from our visalization of the data we could easily tell that many of the features would be highly correlation. 

All subgroups had the same number of datapoints so Pearsons was used to determine which time series were strongly related. Pearsons returns a value between -1 and 1 with 0 being no correlation. This analysis showed all but 6 time series were strongly related to the national average (Persons score over 0.7) and so the majority were removed leaving us with a vastly simplified dataset to model on. 

Before modeling could begin it was necessary to define what was meant by normal. The dataset and associated documentation did not define it but did refer to a 2019 study on reduced access due to costs. A search of the CDC database didn’t turn up the 2019 report but did have one covering 1997 to 2007. This data showed all groups holding relatively steady between 10 and 15% reduced access to care. Based on this 15% was selected as the target for the national average. 

Next our error metric and time steps were defined. Without seeing any special needs for error RMSE was selected for all models. The time steps in the data averaged out to about one data point every two weeks so this was chosen as the time between each of our predictions. 

Our data consisted of seveal time series, one for the national average and the 6 features not removed by our correlation analysis. Thus I decided to go with a vectored autoregression model. For this model the only paramater to tune is the number of lag variables, to select this I performed a simple grid search. 

![Alt text](Images/lags.png?raw=true "Number of lag variables") 

An RMSE analysis between the training and test data showed that 6 lag variables yeiled the best results.

![Alt text](Images/VAR.png?raw=true "VAR modeling") 

This showed less then encouring results. The VAR model clearly under predicts our validation data. This was using the best lag varable so we will continue with our analysis. For now we will use the model to predict future values and try to determine when access to care will return to normal. Which, according to the CDC, is around 15%. Retraining the model on the full dataset and making predictions into the future was pretty reasonable. 

![Alt text](Images/VARpredict.png?raw=true "VAR predict")

This model indicates a return to normal by the beginning of September. 

### Model validation
Data for this project was collected in March 2021 and only contained data from before February 2021. In April 2021 data was recollected, which now contained data up to June 2021. This allowed me to compare the results of my VAR model against a few months of data. 

![Alt text](Images/VARpredictvsActual.png?raw=true "VAR vs Actual data")

The predictions from our VAR model seem pretty reasonable, even if it still under predicts the actual data. The CDC data lags behind current events pretty signifcantly, at the time of writing this notebook we have seen the results of the Delta varriant and the hospital closures that have come with it. That could mean access to care was reduced during that time and might track with the peak in the VAR predictions. 

#### Future work
Continuing work on this project their are a few things we could do to imporve the model performance

- Impute missing data
     - Their are a few gaps in the data, currently our VAR model doesn't take that into account which could be skewing the results. Simple imputation of the missing data might improve accuracy
     
- Removing the trend
     - With the original data from Febuary we didn't see much of a trend, however with the data from June we can now see that the data does have a strong downward trend. This could be removed with differencing which could help model performance
     
- Seasonality
     - The VAR predictions, as well as knowledge of the Delta varriant, could indicate a seasonal trend to the data. While we aren't currently seeing this in the CDC data it's something to look out for in future itterations of this project. 
