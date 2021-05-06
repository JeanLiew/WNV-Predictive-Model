# Proj 4 Readme (West Nile Virus prediction)
This project was done by Allen, Jean and Mark.

## Background
The West Nile virus(WNV) is the leading cause of mosquito-borne disease in continental United States.  Cases of WNV occur during mosquito season, typically starting in the summer and continuing through fall.
Whilst most people display no symptoms, 1 in 5 people will develop a fever and may develop headaches, body aches and joint pains.
About 1 in 150 people infected will develop a severe illness affecting the central nervous system such as encephgalitis(inflammation of the brain) or meningitis.

Whilst there is no vaccine or specific antivral treatments for the West Nile Virus, over-the-counter pain relievers can be used to alleviate fever and provide some relief.  In severe cases however, patients often need to be hospitalized to receive support treatment.

The Centre for Disease Control and Prevention (CDC) has provided the team with Chicago data from 2007 to 2014, broadly grouped into 3 categories:
> 1. Spray data, recording dates and locations where fogging of mosquitos took place
> 2. Weather data, recording key weather statistics in the date range
> 3. Trap data, recording location of mosquito traps placed around the city, the number of mosquitos caught and whether these mosquitos carried the WNV.

## Problem statement
Our team will utilise the data to answer the following problem statements:
> 1. Predict the occurence of the WNV in 2008, 2010, 2012 and 2014 of WNV combining in the above datasets by building a classification model using accuracy as a score.
> 2. Identify the key features in the model that affect WNV and provide recommendations on how to use transmission.


## Data dictionary

Most used features
| Feature Name | Data Type | Description |
| ----| ---- | ---- |
| date | datetime64  | YYYY-MM-DD |
| nummosquitos | int | number of mosquitos caught in the trap |
| tavg | float | daily average temperature (F) |
| tmax | float | daily maximum temperature (F)|
| tmin | float | daily minimum temperature (F)|
| preciptotal | float | preciptation (inches) |
| avgspeed | int | average wind speed (mph) |
| resultspeed | int | resultant wind speed taking into account direction (mph) |
| resultdir | int | direction of wind (deg) |
| dewpoint | float | dewpoint temperature (F)|
| latitude | float | latitude from Geocoder|
| longitude | float | longitude from Geocoder|
| stnpressure | float | average daily station pressure|
| sealevel | float | average daily sealevel pressure|
| daylightmins | int | total number of minutes of daylight|
| year | int | year|
| month | int | month|
| day | int | day|
| wnvpresent | int | dummified value representing presence of WNV.  1 = Present, 0 = Not Present|
| species_X | int | dummified value representing presence of species_X in trap. 1 = Present, 0 = Not Present|

Additional features can be found in noaa_weather_qclcd_documentation.pdf.

## Methodology
The codebook is split into 3 main notebooks.

#### 01_Datacleaning

In this section we standardise the format of the datasets and 
1. Cleaning of spray dataset
> * Drop duplicates
> * Standardise columns with other datasets
2. Cleaning weather dataset
> * Datetime relevant columns
> * Drop nulls
> * Impute missing values for tavg, depart etc.
> * Split into weather data from station 1 and 2
> * Standardise columns with other datasets
3. Cleaning train and test datasets
> * Datetime relevant columns
> * Standardise columns with other datasets
> * Standardise columns with other datasets

#### 02_EDA
In this section we explore and draw some initial insights from the data.

1. EDA on Train
> * Find that the rate of WNV peaks in August
> * Find that certain species of mosquitos are more likely to carry the strain of WNV
2. EDA on Weather
> * Find that July-Aug are the hottest and wettest months in Chicago
> * Feature engineer minutes of daylight per day and find that June has the most amount of daylight
> * Visualisations of weather data with average number of mosquitos and find that nubmer of mosquitos is positively correlated with temperature and negatively correlateed with precipitation.
> * Visualisations of weather data with WNV data which suggest that WNV is inverseley proportional to precipitation and dewpoint.
> * OHE of species of mosquitos and merge weather data into train and test datasets
3. EDA on Spray
> * Visualise spray locations and locations where WNV was detected and find that accuracy of spraying locations can be improved especially in 2007.
4. Data Merging
> * Merge the weather data and the trap data on date.

#### 03_Modelling
In this section we try and obtain the best accuracy from Logreg, KNN and Random Forest classification models using Synthetic Minority Ovesrampling Technique (SMOTE) due to significantly imbalanced classes.
1. Gridsearch Logreg
2. Gridsearch KNN
3. Gridsearch RF
4. Model evaluation for Random Forest (best model)
5. Plot AUCROC for RF
6. Feature importances for RF

## Conclusions

Our final model had an AUCROC score of 0.88 on the test set.  Locational features were picked out by the model as the most important, dominating other weather features that might have predicted mosquito propagation such as temperature and precipitation.

However, when predicting WNV in 2008, 2010, 2012 and 2014, there was a significant difference in our accuracy on the test set between our submission score of 0.61 and the leading Kaggle scores of 0.85.  This suggests that our model did not generalise well.  Apart from collating more data, the low score might be due to the following factors:

1. Key features might have been different in the prediction years.
2. Likely that noise was modelled - less relevant features could have been dropped or grouped, then dummified.
3. Use spray data in the model, joined on date of spray, to see if it would have further impact.
4. Explore other types of synthetic sampling such as ADASYN.


## Recommendations

To reduce the spread of WNV, it might be interesting to take a leaf out of other tropical countries such as Singapore, a small city-state with a population of about 5.7 million.  The hot and  humid temperatures means in Singapore mean it is mosquito season all year round.  However, Singapore's National environment Agency  has reduced the number of [dengue cases](https://www.straitstimes.com/singapore/singapores-weekly-dengue-cases-at-lowest-level-this-year-following-historic-outbreak#:~:text=SINGAPORE%20%2D%20Dengue%20cases%20have%20fallen,of%201%2C792%20cases%20in%20July.) through a mixture of education, fogging and bio-engineering.



__1. Spray more frequently.__
As location was the most important factor, it is likely that spray coverage during the mosquito season is one of the key driving factors to curb the spread of WNV.  As could be seen from the EDA, the CDC of Chicago did not place much emphasis on thorough and widespread fogging, which might have caused the spike in cases in 2010.  As spraying is more a preventive measure, this has to be done __constantly and frequently__ during the months of May to October, during mosquito breeding season.

__2. Bioengineering__
We noted previously in the EDA that only 2 breeds of mosquitos, Culex Pipiens and Culex Restauns, carry the WNV strains.  Taking another leaf out of Singapore's mosquito culling guidebook, it may be possible to either introduce natural predators of the mosquitos such as birds or other insects to limit breeding when in season.  Additionally, it may also be possible to introduce specially bred mosquitos which carry bacterias that prevent eggs from hatching, effectively sterilising the population.

__3. Active outreach and monitoring__
Finally, it may be worth the CDC's effort to educate the general public especially in mosquito hotspots (as seen from the spray data).  By involving the community, it is possible to limit the breeding grounds of these mosquitos when in season.  Additionally, Singapore has an active mosquito task force that goes door to door to inspect and educate residents on how to reduce mosquito breeding sites.



