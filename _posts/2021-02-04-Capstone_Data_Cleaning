---
layout: post
title: 'How Many Fire Engines Tomorrow?' Data Gathering And Cleaning Overview.
---
1) Data Sources:

London Datastore - https://data.london.gov.uk/dataset/london-fire-brigade-incident-records
2 files listing all LFB callouts for 2017-2020 (inclusive) and 2016.

Met Office Archives (CEDA Midas Open) - https://catalogue.ceda.ac.uk/uuid/dbd451271eb04662beade68da43546e1
1 file per year for hourly weather observations, with additional files per year for rain details and wind details for 2016-2019 (inclusive) - 12 files in total.

Meteostat - https://github.com/meteostat Great resource (with python library and easy api) for historical weather data worldwide (initially was going to webscrape from the 
website - https://meteostat.net/en/station/03772) 1 File for 2020 - one observation (sun) missing from the dataset compared with met office records (and other observations to be 
feature engineered).

Earth System Research Laboratories - https://www.esrl.noaa.gov/gmd/grad/solcalc/calcdetails.html - excel spreadsheet to download to calculate sunrise/sunset times in London 
(needed for an 'is light' marker).

2) Creating the database:

Overview of final database:

class 'pandas.core.frame.DataFrame'>
Int64Index: 518240 entries, 0 to 518239
Data columns (total 26 columns):
     Column                 Non-Null Count   Dtype         
---  ------                 --------------   -----         
 0   DateOfCall             518240 non-null  object        
 1   CalYear                518240 non-null  int64         
 2   HourOfCall             518240 non-null  int64         
 3   IncidentGroup          518240 non-null  object        
 4   PropertyType           518240 non-null  object        
 5   SpecialServiceType     518240 non-null  object        
 6   IncidentStationGround  518240 non-null  object        
 7   DateTimestamp          518240 non-null  object        
 8   Month                  518240 non-null  int64         
 9   Day                    518240 non-null  int64         
 10  dayofweek              518240 non-null  int64         
 11  holiday                518240 non-null  int64         
 12  weekend                518240 non-null  int64         
 13  Month_Day              518240 non-null  object        
 14  ob_time                518240 non-null  datetime64[ns]
 15  air_temperature        518240 non-null  float64       
 16  cld_ttl_amt_id         518240 non-null  float64       
 17  islight                518240 non-null  int64         
 18  max_gust_speed         518240 non-null  float64       
 19  prcp_amt               518240 non-null  float64       
 20  rltv_hum               518240 non-null  float64       
 21  stn_pres               518240 non-null  float64       
 22  visibility             518240 non-null  float64       
 23  wind_direction         518240 non-null  float64       
 24  wind_speed             518240 non-null  float64       
 25  wmo_hr_sun_dur         518240 non-null  float64   
 
 The first 14 columns are from the LFB files, the last 11 from the weather files and sunrise/sunset calculations with 'ob_time' as the glue between them.
 
 The LFB files list each callout (with a precise time) during the period. The ob_time strips out the time element more precise than hours to be able to match 
 the callout environmental conditions with the actual callout time. 'float64' variables are continuous, 'int64' variables are either date/time related (kept in as a
 back-up if necessary for modelling) or categorical. 'object' variables are mainly text (and will be used as categorical variables).
 
 3) Clarifying the Project goals

Having taken an initial look at the data (and quality - more below), the project goals are:

A) Predict numbers of callouts per day for London (based on aggregating the predicted callouts per hour). This will be based on a webscraped weekly forecast from the
met office website (giving the same variables as we have in the history, including the 'islight' marker).
B) Classifying the predicted callouts into False Alarm/Fire/Special Services (the object variables are likely to be more useful for this task).
C) Strech goal is to predict by individual fire station.

I'll be using time series (and potentially other linear regression) models for predicting the number of callouts and will try logistic regression and random forests for
the classification (with the option to try additional models for both goals if needed).

Current Assumptions: The main assumption is that the weather will have an effect on callouts (fingers crossed) and that the effect will be seperable from any
time series predictive effects. There are other (smaller) assumptions noted below as part of the data cleaning.

4) Assembling and Cleaning the Data (A Long Process!)

The first dataset imported was the 2017-2020 LFB dataset:

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 413008 entries, 0 to 413007
Data columns (total 38 columns):
     Column                                  Non-Null Count   Dtype  
---  ------                                  --------------   -----  
 0   IncidentNumber                          413008 non-null  object 
 1   DateOfCall                              413008 non-null  object 
 2   CalYear                                 413008 non-null  int64  
 3   TimeOfCall                              413008 non-null  object 
 4   HourOfCall                              413008 non-null  int64  
 5   IncidentGroup                           413008 non-null  object 
 6   StopCodeDescription                     413008 non-null  object 
 7   SpecialServiceType                      129372 non-null  object 
 8   PropertyCategory                        413008 non-null  object 
 9   PropertyType                            413008 non-null  object 
 10  AddressQualifier                        413008 non-null  object 
 11  Postcode_full                           203965 non-null  object 
 12  Postcode_district                       413008 non-null  object 
 13  UPRN                                    413008 non-null  int64  
 14  USRN                                    413008 non-null  int64  
 15  IncGeo_BoroughCode                      413008 non-null  object 
 16  IncGeo_BoroughName                      413008 non-null  object 
 17  ProperCase                              413008 non-null  object 
 18  IncGeo_WardCode                         413008 non-null  object 
 19  IncGeo_WardName                         413008 non-null  object 
 20  IncGeo_WardNameNew                      413008 non-null  object 
 21  Easting_m                               203965 non-null  float64
 22  Northing_m                              203965 non-null  float64
 23  Easting_rounded                         413008 non-null  int64  
 24  Northing_rounded                        413008 non-null  int64  
 25  Latitude                                203965 non-null  float64
 26  Longitude                               203965 non-null  float64
 27  FRS                                     413008 non-null  object 
 28  IncidentStationGround                   413008 non-null  object 
 29  FirstPumpArriving_AttendanceTime        389188 non-null  float64
 30  FirstPumpArriving_DeployedFromStation   389181 non-null  object 
 31  SecondPumpArriving_AttendanceTime       157869 non-null  float64
 32  SecondPumpArriving_DeployedFromStation  157865 non-null  object 
 33  NumStationsWithPumpsAttending           410795 non-null  float64
 34  NumPumpsAttending                       410795 non-null  float64
 35  PumpCount                               411209 non-null  float64
 36  PumpHoursRoundUp                        411085 non-null  float64
 37  Notional Cost (£)                       411085 non-null  float64

A number of the columns were not necessary for my task and were dropped. The 2016 dataset was imported next - with fewer columns (and as I found out 
a little later, some different formats for the same columns).

At this point the database looked like this:
class 'pandas.core.frame.DataFrame'>
RangeIndex: 518444 entries, 0 to 518443
Data columns (total 7 columns):
     Column                 Non-Null Count   Dtype 
---  ------                 --------------   ----- 
 0   DateOfCall             518444 non-null  object
 1   CalYear                518444 non-null  int64 
 2   HourOfCall             518444 non-null  int64 
 3   IncidentGroup          518443 non-null  object
 4   PropertyType           518443 non-null  object
 5   SpecialServiceType     162658 non-null  object
 6   IncidentStationGround  518444 non-null  object
 
 At this point, columns were added for (bank) holiday and weekend markers (as well as some additional time/date related columns - these may be redundant)
 
 At this point, it was time to tackle the weather.
 
 The numerous files were imported from the met office archive. 
 
 Firstly, the hourly weather:
 
 <class 'pandas.core.frame.DataFrame'>
RangeIndex: 8785 entries, 0 to 8784
Columns: 104 entries, ob_time to drv_hr_sun_dur_q
dtypes: float64(98), object(6)
memory usage: 7.0+ MB

After dropping the unnecessary columns, we were left with:

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 8785 entries, 0 to 8784
Data columns (total 9 columns):
     Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   ob_time          8785 non-null   object 
 1   wind_direction   8775 non-null   float64
 2   wind_speed       8775 non-null   float64
 3   cld_ttl_amt_id   8781 non-null   float64
 4   visibility       8781 non-null   float64
 5   air_temperature  8782 non-null   float64
 6   rltv_hum         8782 non-null   float64
 7   stn_pres         8782 non-null   float64
 8   wmo_hr_sun_dur   8414 non-null   float64
dtypes: float64(8), object(1)

The rain and wind files gave an additional column each (and one set of files per year). Finally the sunrise/set info was added.

One year's result (2016):

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 8784 entries, 0 to 8783
Data columns (total 12 columns):
     Column           Non-Null Count  Dtype         
---  ------           --------------  -----         
 0   air_temperature  8784 non-null   float64       
 1   rltv_hum         8784 non-null   float64       
 2   prcp_amt         8784 non-null   float64       
 3   wind_direction   8784 non-null   float64       
 4   max_gust_speed   8784 non-null   float64       
 5   stn_pres         8784 non-null   float64       
 6   wmo_hr_sun_dur   8784 non-null   float64       
 7   visibility       8784 non-null   float64       
 8   cld_ttl_amt_id   8784 non-null   float64       
 9   wind_speed       8784 non-null   float64       
 10  ob_time          8784 non-null   datetime64[ns]
 11  islight          8784 non-null   int64         
dtypes: datetime64[ns](1), float64(10), int64(1)
memory usage: 823.6 KB

The same process was used for 2017, 2018 and 2019.

There were also some gaps in the data, which were backfilled by a combination of downloading additional data from meteostat and backfilling from existing data.

The 2020 data was different. Some of the data needed to be feature engineered from the availble data to achieve the same columns (with some assumptions about 
visibility and cloud cover equivalents to text descriptions).

At this point, it was necessary to adjust the weather data (exclusively in GMT) to match with the LFB data (in local time - ie GMT and BST). 
This required inserting a row into a dataframe, shifting the datetime index and then removing the extra row.

The final steps were merging the weather dataframes and the merging the weather datframe with the LFB database to give the final result.

A typical entry:

DateOfCall                                      22/10/2020
CalYear                                               2020
HourOfCall                                              13
IncidentGroup                                  False Alarm
PropertyType             Self contained Sheltered Housing 
SpecialServiceType                                       0
IncidentStationGround                           Wandsworth
DateTimestamp                                   2020-10-22
Month                                                   10
Day                                                     22
dayofweek                                                3
holiday                                                  0
weekend                                                  0
Month_Day                                            10_22
ob_time                                2020-10-22 13:00:00
air_temperature                                       15.5
cld_ttl_amt_id                                           8
islight                                                  1
max_gust_speed                                        31.5
prcp_amt                                                 0
rltv_hum                                                67
stn_pres                                            1008.7
visibility                                            2000
wind_direction                                         270
wind_speed                                         9.71923
wmo_hr_sun_dur                                           0

Thanks for reading.

Andrew
