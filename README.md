# Power Outage Severity Model

# Created By William Czech

## Introduction

The dataset I chose contains information about power outages that occurred in the U.S from 2000-2016, and has 1534 rows which each represent a unique power outage. Each outage contains data on the duration of the outage, the cause of the outage, its location in the U.S, the date of the outage, and economic and population data about the location where the outage occurred. The question I am seeking to answer with this dataset is what features of a power outage contribute to its severity? 

To measure the severity of an outage, I will use the duration of the outage using the OUTAGE.DURATION column, and will be only looking at cases involving severe weather and not targeted attacks or random equipment failures, as these cases are more difficult to predict. The features I will use to predict the duration of an outage are the region where the outage occurred (CLIMATE.REGION), details on what severe weather cause the outage (CAUSE.CATEGORY.DETAIL), the month of the outage (OUTAGE.MONTH) and the percentage of land in the state where the outage occured that is classified as an urban area (AREAPCT_URBAN).

The model I will build from this analysis will allow for better predictions on where more severe power outages in the U.S may occur, and could potentially help urban planners better protect areas of the U.S at greater risk for these outages.

