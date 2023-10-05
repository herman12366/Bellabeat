# Bellabeat Case Study 
##### Author: Herman Singh 

### Scenario 
Bellabeat is a high-tech manufacturer of health-focused products for women. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. We must focus on one of Bellabeat's products and analyze smart device data to understand how customers use their smart devices. 

### Ask
Analyze Fitbit data to gain insight and help guide marketing strategy for Bellabeat to grow. Looking at trends in smart device usage, we can determine recommendations for Bellabeat business. 

Primary Stakeholders: Urška Sršen, Bellabeat’s cofounder and Chief Creative Officer; Sando Mur, Bellabeat’s cofounder; Bellabeat marketing analytics team.

Important questions to think about: 
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat's marketing strategy?

### Prepare 
Start by loading up the packages that will be needed: 
```
install.packages(c('tidyverse', 'lubridate', 'skimr', 'ggplot2'))
library(tidyverse)
library(lubridate)
library(skimr)
library(ggplot2)
```
Then load the Fitbit fitness tracker data (available on Kaggle): 18 files in CSV format.
```
dailyActivity_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
dailyCalories_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/dailyCalories_merged.csv")
dailyIntensities_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/dailyIntensities_merged.csv")
dailySteps_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/dailySteps_merged.csv")
heartrate_seconds_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/heartrate_seconds_merged.csv")
hourlyCalories_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
hourlyIntensities_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
hourlySteps_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
minuteCaloriesNarrow_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteCaloriesNarrow_merged.csv")
minuteCaloriesWide_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteCaloriesWide_merged.csv")
minuteIntensitiesNarrow_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteIntensitiesNarrow_merged.csv")
minuteIntensitiesWide_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteIntensitiesWide_merged.csv")
minuteMETsNarrow_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteMETsNarrow_merged.csv")
minuteStepsNarrow_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteStepsNarrow_merged.csv")
minuteStepsWide_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/minuteStepsWide_merged.csv")
sleepDay_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
weightLogInfo_merged <- read_csv(".../Ballebeat/.Rproj.user/Fitabase Data 4.12.16-5.12.16/weightLogInfo_merged.csv")
```
Now we must review and validate the data. We know there are 30 users who have shared data: 
```
sapply(dailyActivity_merged, function(x) length(unique(x)))
sapply(dailyCalories_merged, function(x) length(unique(x)))
sapply(dailyIntensities_merged, function(x) length(unique(x)))
sapply(dailySteps_merged, function(x) length(unique(x)))
sapply(heartrate_seconds_merged, function(x) length(unique(x)))
sapply(hourlyCalories_merged, function(x) length(unique(x)))
sapply(hourlyIntensities_merged, function(x) length(unique(x)))
sapply(hourlySteps_merged, function(x) length(unique(x)))
sapply(minuteCaloriesNarrow_merged, function(x) length(unique(x)))
sapply(minuteCaloriesWide_merged, function(x) length(unique(x)))
sapply(minuteIntensitiesNarrow_merged, function(x) length(unique(x)))
sapply(minuteIntensitiesWide_merged, function(x) length(unique(x)))
sapply(minuteMETsNarrow_merged, function(x) length(unique(x)))
sapply(minuteStepsNarrow_merged, function(x) length(unique(x)))
sapply(minuteStepsWide_merged, function(x) length(unique(x)))
sapply(sleepDay_merged, function(x) length(unique(x)))
sapply(weightLogInfo_merged, function(x) length(unique(x)))
```
Most of the files have 33 unique user IDs. Some files didn't have 33, which might mean we cannot analyze this information. 

### Process 
Now, we will look at data samples to see the number of rows, missing values, and averages, and validate Max and Min values. 
```
skim_without_charts(dailyActivity_merged)
activity_summary <-
  dailyActivity_merged %>% 
  group_by(Id) %>% 
  summarise(n=n())
view(activity_summary)
dailyActivity_merged %>%
  select(TotalDistance, TotalSteps, SedentaryMinutes) %>%
  summary()

skim_without_charts(dailyCalories_merged)
calories_summary <- 
  dailyCalories_merged %>% 
  group_by(Id) %>%
  summarize(n=n())
view(calories_summary)
dailyCalories_merged %>%
  select(Calories) %>%
  summary()

skim_without_charts(dailyIntensities_merged)
intensities_summary <- 
  dailyIntensities_merged %>% 
  group_by(Id) %>%
  summarize(n=n())
view(intensities_summary)
dailyIntensities_merged %>%
  select(SedentaryMinutes, VeryActiveMinutes, SedentaryActiveDistance, VeryActiveDistance) %>%
  summary()

skim_without_charts(dailySteps_merged)
steps_summary <- 
  dailySteps_merged %>% 
  group_by(Id) %>%
  summarize(n=n())
view(steps_summary)
dailySteps_merged %>%
  select(StepTotal) %>%
  summary()

skim_without_charts(minuteMETsNarrow_merged)
minuteMETsNarrow_merged %>%
  select(METs) %>%
  summary()

skim_without_charts(sleepDay_merged)
sleepDay_merged %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()

skim_without_charts(weightLogInfo_merged)
weightLogInfo_merged %>%
  select(WeightKg, BMI, Fat) %>%
  summary()
```
One clear observation is that the median BMI is below 25 meaning that most of the users who logged their weight had a normal weight. 
Some of the variables are not saved in the right format, need to edit the format of the values. Also, we found that the Maximum of Sedentary Minutes is 1440 which is equal to 24 hours. This means that the data includes the amount of time used to sleep. In order to get the real sedentary minutes we will join the data about daily activities with sleep data by inner join and add a column called awakeSedentaryMinutes: 
```
dailyActivity_merged$ActivityDate=as.POSIXct(dailyActivity_merged$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
sleepDay_merged$SleepDay=as.POSIXct(sleepDay_merged$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
weightLogInfo_merged$Date=as.POSIXct(weightLogInfo_merged$Date, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
merged_limited_data <- merge(sleepDay_merged, dailyActivity_merged, by.x=c("Id","SleepDay"), by.y = c("Id", "ActivityDate"))
```
Based on the summaries of the other data frames we see that some ID data exists only for 16-29 days. This should be enough records for most users, however, there is one user that only has 4 days of records. We should remove that user from the data to get a better analysis: 
```
merged_limited_data <- merged_limited_data[-c(4057192912),]
dailyActivity_merged_clean <- dailyActivity_merged[-c(4057192912),]
sleepDay_merged_clean <- sleepDay_merged[-c(4057192912),]
weightLogInfo_merged_clean <-weightLogInfo_merged[-c(4057192912),]
```

### Analyse 
Start creating some visualizations to see trends. Let's try to see if there is a relationship between TotalSteps and SedentaryMinutes. 
```
ggplot(data=merged_limited_data, aes(x=TotalSteps, y=SedentaryMinutes)) + geom_point()
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/37cafb52-84ca-4806-ad26-19f7b6a6be7c)
We clearly so no relationship between sedentary minutes and total step count. There is no visual trend among those two criteria. 
Now let's try to see if there is a relationship between Light Active Minute and Total Steps: 
```
ggplot(data=merged_limited_data, aes(x=TotalSteps, y=LightlyActiveMinutes)) + geom_point()
```
![5c4aae70-da93-442e-8ef9-4c2e0510e55c](https://github.com/herman12366/Bellabeat/assets/20954773/9cccbe33-b40a-4b97-83b9-15b3b28354c2)
We can clearly see a positive trend between these two data values. As Light Active Minute increases so does the total number of steps. 
Now let's make a visualization to see the relationship between calories and TotalSteps and let's also combine all active minutes and see how users spent calories that same day: 
```
ggplot(data=merged_limited_data, aes(x=TotalSteps, y=Calories, color=SedentaryMinutes)) + geom_point() + scale_color_gradient(low="steelblue", high="orange")
dailyActivity_merged_clean <- dailyActivity_merged_clean %>% rowwise() %>%
  mutate(activeMinutesTotal = FairlyActiveMinutes + LightlyActiveMinutes + VeryActiveMinutes)
ggplot(data=dailyActivity_merged_clean, aes(x=activeMinutesTotal, y=Calories)) + geom_point()
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/d09d72c7-753e-4e75-bb41-e4b26994853f)
![image](https://github.com/herman12366/Bellabeat/assets/20954773/6bc8f954-5268-46f3-9cd0-903e54bcb0c1)
One interesting thing to realize when looking at these two graphs is that there isn't any way to know how Calories were being tracked. If this number does indicate the number of calories burnt then there are many records with 0 steps and burning calories in the 1000s. However, if we eliminate the outliers in the data we can visualize a positive trend. As the active minutes increase so do the calories the person is burning. 
```
ggplot(data=sleepDay_merged_clean, aes(x=TotalTimeInBed, y=TotalMinutesAsleep)) + geom_point()
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/7f5ad15d-792c-4804-8bae-e0e63fbbdafc)
When we compare the total minutes spent asleep with the total time in bed we can see a clear positive dependency. However, we can see there are some users who were outliers which means they were not asleep while in bed for a period of time. Let's calculate the time that was spent not asleep but in bed. 
```
merged_limited_data <- merged_limited_data %>% rowwise() %>%
  mutate(awakeInBed = TotalTimeInBed - TotalMinutesAsleep)
ggplot(data=merged_limited_data, aes(x=awakeInBed)) + geom_histogram(color="white", fill="black") + 
  labs(title="Time awake in bed - histogram plot",x="Time awake in bed (min)", y = "Number of records (count)")
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/e8920c97-b0d7-42ef-9a57-87e6ddf6cc92)
We can see that most users spent less than an hour of being awake in bed. Now let's see how active the users were based on active total time. 
```
ggplot(data=dailyActivity_merged_clean, aes(x=activeMinutesTotal)) + geom_histogram(color="white", fill="black") +
  labs(title="Active Minutes histogram plot",x="Active time per day (min)", y = "Number of records (count)")
sum(dailyActivity_merged_clean$activeMinutesTotal < 60)
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/f51908d7-fc56-4771-902c-9f719415f952)
There is a surprising number of users who don't do any physical activity during the day. We see that there are 128 users who don't get at least an hour of physical activity. This might be one of the key focuses for Bellabeat's marketing strategy. Let's dig a little deeper and see the relationship between the amount of physical activity on a given day. 
```
dailyActivity_merged_clean <- dailyActivity_merged_clean %>% rowwise() %>%
  mutate(DayOfWeek = weekdays(ActivityDate))
ggplot(data=dailyActivity_merged_clean, aes(x=activeMinutesTotal)) + geom_histogram() + facet_wrap(~DayOfWeek)

dailyActivity_merged_clean %>% group_by(DayOfWeek) %>%
  summarise(zeroActivityDays=sum(activeMinutesTotal==0), lowActivityDays=sum(activeMinutesTotal < 60), 
            activeDays=sum(activeMinutesTotal>=60), totalDays=(zeroActivityDays+lowActivityDays+activeDays), lowActivityPercent=(lowActivityDays/totalDays))
```
![image](https://github.com/herman12366/Bellabeat/assets/20954773/bb96c6f7-16e3-4778-917b-5b2870603ed3)
We can see that Tuesdays and Thursdays have the most users with low physical activity. Surprisingly Friday had the least amount of users with low physical activity. However, every day there were users with less than an hour of physical activity. Bellabeat can focus on this market and I can recommend setting a reminder to users about the necessary amount of physical activity. We can next analyze the sleep patterns. Prior we already determined there is a difference between the amount of user data provided. This can be one of the recommendations to look into how the app registers the sleep data. 
```
ggplot(data=merged_limited_data, aes(x=TotalMinutesAsleep)) + geom_histogram(color="white", fill="black")
merged_limited_data <- merged_limited_data %>% rowwise() %>%
  mutate(DayOfWeek = weekdays(SleepDay))
merged_limited_data %>% group_by(DayOfWeek) %>%
  summarise(NormalHoursOfSleep=sum(TotalMinutesAsleep>=420), InsufficientSleep=sum(TotalMinutesAsleep<420), 
            InsufficientSleepPercent=(InsufficientSleep/(InsufficientSleep+NormalHoursOfSleep)))
```
| DayOfWeek | NormalHoursOfSleep | InsufficientSleep | InsufficientSleepPercent |
|-----------|--------------------|-------------------|--------------------------|
| Friday    |               26   |            31     |              0.544       |
| Monday    |               27   |            20     |              0.426       |
| Saturday  |               31   |            27     |              0.466       |
| Sunday    |               37   |            18     |              0.327       |
| Thursday  |               35   |            30     |              0.462       |
| Tuesday   |               30   |            35     |              0.538       |
| Wednesday |               45   |            21     |              0.318       |


![image](https://github.com/herman12366/Bellabeat/assets/20954773/c48fa6f1-17e9-479f-ac7c-94aae7f8a72d)

