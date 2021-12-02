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

![Alt text](Images/BoxPlotsStates.png?raw=true "Box Plots for State Subgroups")

While their were a few that seemed like their were above or below the average nothing stood out as definitive.  
