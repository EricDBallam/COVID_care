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

![Alt text](Images/WeekCountStateMap.png?raw=true "Weeks above or below the national average") 
