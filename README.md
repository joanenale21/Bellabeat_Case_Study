# Bellabeat Case Study: How Can a Wellness Technology Company Play It Smart 

# **Introduction**

Bellabeat is a high-growth wellness technology company that manufactures health-focused smart products for women. The marketing team wants to better understand how users engage with fitness trackers to improve marketing strategies for the **Leaf product** - a wearable that tracks activity, sleep, and stress.

This project analyzes the Fitbit smart device usage data to discover trends in user behavior and propose a data-driven marketing action.

In my role as a junior data analyst, I was tasked to clean and preparing real-world Fitbit data using the R language. Analyzing user behavior related to activity, sleep, and inactivity. Creating visualizations to support findings and delivering marketing recommendations to stakeholders. 

**Goals**

The goals of my analysis are:
1. Assess the average level of daily physical activity, including how often users reach the widely recommended 10,000-step goal.
2. Identify patterns in user activity throughout the week by highlighting which days show the highest and lowest engagement in movement.
3. Analyze the amount of sedentary time users accumulate per day and evaluate any connections between prolonged inactivity and overall activity levels.
4. Evaluate users' sleep duration and sleep efficiency to determine how consistently users meet healthy sleep standards.
5. Use these behavioral insights to define opportunities for Bellabeat's marketing strategy. 

# Preparations
First, looking into the dataset shows that it has a small size of 30 Fitbit users. The data is consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. The dataset is generated via Amazon Mechanical Turk for 61 days in 2016. Due to the small size of the data, it may not represent the population of  Bellabeat. 

I downloaded the dataset from this site and extracted it to a new folder on my laptop. There were a total of 18 CSV files, but I only used 2 CSV files since all the data that I need is already here in these files, the files are:
1. dailyActivity_merged.csv
2. sleepDay_merged.csv
#load required libraries
library(tidyverse)
library(lubridate)
library(ggplot2)
library(skimr)
library(corrplot)

#importing datasets
daily_activity <- read_csv("/kaggle/input/fitbit/mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
sleep_day <- read_csv("/kaggle/input/fitbit/mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
#Data inspection
head(daily_activity)
head(sleep_day)
For the data cleaning process, I used R Studio to clean the data to ensure that there were no missing values or duplicated data. Since the SleepDay column has time, I converted it to a proper date format to ensure data consistency.
#check for missing or duplicated data
#check for missing values
sum(is.na(daily_activity))
sum(is.na(sleep_day))

#remove duplicate rows
daily_activity <- distinct(daily_activity)
sleep_day <- distinct(sleep_day)

#format and clean date columns
#Convert ActivityDate and SleepDay to proper date format 
daily_activity <- daily_activity %>% 
  mutate(ActivityDate = mdy(ActivityDate))

sleep_day <- sleep_day %>% 
  mutate(SleepDay = mdy_hms(SleepDay))

#renaming SleepDay column to match the Acitvity date for smooth merging of data
colnames(sleep_day)[colnames(sleep_day) == "SleepDay"] <- "ActivityDate"

#merged the datasets
merged_data <- merge(daily_activity, sleep_day, by = c("Id", "ActivityDate"))
I renamed the SleepDay column to ActivityDate so both datasets have a common date field. Then, I merged the datasets using both ID and ActivityDate, which allows both datasets' data to be combined accurately for each user on each day.
#Quick summary of merged data
summary(merged_data)
Here's a quick inspection of the structure, completeness, and distribution of merged data. This is helpful to validate that the data is clean and ready for analysis. 
#add weekday column for trend analysis
merged_data <- merged_data %>% 
  mutate(Weekday = weekdays(ActivityDate))

#creating new column for analysis which help answer key business questions
merged_data <- merged_data %>% 
  mutate(SleepHours = round(TotalMinutesAsleep / 60, 1),
         MetStepGoal = if_else(TotalSteps >= 10000, "Yes", "No"),
         DayofWeek = wday(ActivityDate, label = TRUE))

#creating metrics relevant to Bellabeat Leaf features
#Calculate sedentary vs. active minutes ratio
merged_data <- merged_data %>% 
  mutate(
    ActiveMinutes = VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes,
    SedentaryRatio = SedentaryMinutes / (ActiveMinutes + 1) #Avoid division by zero
  )

#Categorize sleep efficiency
merged_data <- merged_data %>% 
  mutate(
    SleepEfficiency = TotalMinutesAsleep / TotalTimeInBed,
    SleepCategory = case_when(
      SleepEfficiency >= 0.9 ~ "High",
      SleepEfficiency >= 0.7 ~ "Medium",
      TRUE ~ "Low"
    )
  )

#Activity Trends
merged_data %>% 
  group_by(Weekday) %>% 
  summarise(AvgSteps = mean(TotalSteps), na.rm = TRUE) %>% 
  ggplot(aes(x = factor(Weekday, levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                                            "Friday", "Saturday", "Sunday")), y = AvgSteps)) +
  geom_col(fill = "steelblue") +
  labs(title = "Average Steps by Weekday", x = "Day", y = "Steps") +
  theme_minimal()
  
This chart shows the average number of steps taken per day throughout the week. Based on this, **Saturday** shows the highest average steps, indicating increased activity during weekends, while **Sunday** has a slightly lower average compared to Saturday but still remains above 7,500 steps. **Monday, Tuesday, and Wednesday** demonstrate relatively high and consistent average steps, while **Thursday and Friday** show a slight decline. 
Let's check the correlation between how activity, sleep, and sedentary time are related. This will determine whether increased physical activity is associated with better sleep, and whether inactivity impacts movement or sleep. 
#Does more activity lead to better sleep?

# Calculate correlations
cor_matrix <- cor(merged_data[, c("TotalSteps", "TotalMinutesAsleep", "SedentaryMinutes")],
                  use = "complete.obs")
#visualize
corrplot(cor_matrix, method = "number", type = "upper")

In the correlation analysis, this shows that **TotalSteps, and both TotalMinutesAsleep and SedentaryMinutes** have a weak inverse relationship. While **TotalMinutesAsleep and SedentaryMinutes** have a stronger inverse relationship, indicating that increased sleeping time correlates with reduced sedentary time. 
I utilize Tableau Public for the rest of my analysis. I used the cleaned and merged data file that I created using R and imported into Tableau. Use the link below to view my work on Tableau. 

https://public.tableau.com/views/Bellabeat_Case_Study_Analysis/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

![image.png](attachment:ea1746be-ecbd-4493-a605-83498be1d38a.png)
![image.png](attachment:e70d6a89-d60d-49d3-b6d8-dd132fcc49cf.png)
![image.png](attachment:3b73ba55-45f8-4bda-99e4-1b219b465228.png)
