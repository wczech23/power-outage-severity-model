# Power Outage Severity Model

## Created By William Czech

### Introduction

The dataset I chose contains information about power outages that occurred in the U.S from 2000-2016, and has 1534 rows which each represent a unique power outage. Each outage contains data on the duration of the outage, the cause of the outage, its location in the U.S, the date of the outage, and economic and population data about the location where the outage occurred. The question I am seeking to answer with this dataset is what features of a power outage contribute to its severity? 

To measure the severity of an outage, I will use the duration of the outage using the OUTAGE.DURATION column, and will be only looking at cases involving severe weather and not targeted attacks or random equipment failures, as these cases are more difficult to predict. The features I will use to predict the duration of an outage are the region where the outage occurred (CLIMATE.REGION), details on what severe weather cause the outage (CAUSE.CATEGORY.DETAIL), the total number of customers in the state (TOTAL.CUSTOMERS) and the percentage of land in the state where the outage occured that is classified as an urban area (AREAPCT_URBAN).

The model I will build from this analysis will allow for better predictions on where more severe power outages in the U.S may occur, and could potentially help urban planners better protect areas of the U.S at greater risk for these outages.

### Data Cleaning and Exploration

To clean my dataset, I merged two columns containing the start date and start time into a single timestamp, and completed the same process for the ending time of the outage. To make the duration of the outage a more simple unit, I converted the time of each outage from minutes to hours. Since my analysis is only focused on severe weather cases, I dropped all rows in my dataset that did not have severe weather as the cause of the outage. Additionally, I dropped rows that contained missing values with columns I was to perform analysis on with my model, and also dropped all values in the CAUSE.CATEGORY.DETAIL column that had less than 4 occurences to speed up the training of my model.

After this cleaning steps, this is the first few rows of my dataframe for the columns that I will analyze.

| cause_detail   | climate_region     |   area_urban |   total_customers |   outage_duration |
|:---------------|:-------------------|-------------:|------------------:|------------------:|
| heavy wind     | East North Central |         2.14 |       2.5869e+06  |              50   |
| thunderstorm   | East North Central |         2.14 |       2.60681e+06 |              42.5 |
| winter storm   | East North Central |         2.14 |       2.5869e+06  |              31   |
| tornadoes      | East North Central |         2.14 |       2.5869e+06  |              49.5 |
| thunderstorm   | East North Central |         2.14 |       2.47491e+06 |              66   |


#### Plots and Analysis

After cleaning my data, I developed some plots to get a better understanding of the relationships in the dataset.

<iframe
  src="plots/outage_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The plot above shows that most outages caused by severe weather lasted between 12 and 86 hours, with the longest outage lasting over 800 hours. We are determining the severity of an outage based on the outage duration, so this plot depicts the distribution of power outage severity.

<iframe
  src="plots/outage_cause_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram above shows that outages caused by mainly by thunderstorms, hurricanes, and winter storms. If we can estimate the severity of outages from these weather occurences, we can build an effective model to predict the severity of outages.

<iframe
  src="plots/outage_duration_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This histogram is interesting because it depicts that specific outage causes may lead to longer (more severe) outages. Flooding, hurricanes, and wildfires had the longest outages on average, likely due to the severity of these natural disasters.

<iframe
  src="plots/outage_duration_region.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This histograms shows that the Southwest region has a much longer average power outage duration compared to other U.S regions with 288 hours. For other regions, the distribution is more uniform with a range of 35 - 100 hours on average, with an outlier in the West North Central region. This plot illustrates which regions have the most severe outages on average, and indicates that the Southwest, South, and Northwest have the highest severity power outages.


#### Data Aggregations

|   flooding |   heatwave |   heavy wind |   hurricanes |   snow/ice  |   storm |   thunderstorm |   tornadoes |   wildfire |   wind storm |   wind/rain |   winter |   winter storm |
|-----------:|-----------:|-------------:|-------------:|------------:|--------:|---------------:|------------:|-----------:|-------------:|------------:|---------:|---------------:|
|          0 |          0 |            6 |            0 |           0 |       1 |              9 |           1 |          0 |            0 |           1 |        0 |              5 |
|          0 |          2 |            0 |           13 |           0 |       4 |             17 |           0 |          0 |            0 |           1 |        0 |              0 |
|          0 |          0 |           12 |            0 |           6 |       2 |              1 |           1 |          3 |            0 |           1 |        0 |             16 |
|          0 |          0 |            9 |            0 |           7 |       2 |              2 |           0 |          0 |            0 |           0 |        4 |             30 |
|          0 |          0 |            4 |            0 |           0 |       7 |              0 |           0 |          0 |            2 |           0 |       18 |             34 |
|          0 |          5 |            0 |            3 |           0 |       7 |             42 |           1 |          8 |            1 |           0 |        0 |              0 |
|          2 |          0 |            2 |            0 |           0 |       8 |             63 |           1 |          2 |            0 |           2 |        0 |              0 |
|          1 |          0 |            3 |            0 |           0 |       3 |              2 |           0 |          0 |            2 |           5 |        1 |              4 |
|          1 |          1 |            1 |            0 |           0 |       4 |             19 |           2 |          2 |            0 |           0 |        0 |              0 |
|          0 |          0 |           10 |            0 |           0 |       0 |              9 |           1 |          2 |            1 |           0 |        0 |              7 |
|          0 |          0 |            9 |           20 |           0 |       2 |              3 |           0 |          6 |            0 |           2 |        0 |              4 |
|          0 |          2 |            4 |           37 |           0 |       1 |              8 |           1 |          0 |            0 |           0 |        0 |              0 |

This pivot table is significant because it depicts what storms are the most common during specific months of the year. If we know what storms cause the most severe outages, knowing what months they are most likely to occur gives us an idea of when the most severe outages occur.
