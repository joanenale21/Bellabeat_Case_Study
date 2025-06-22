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


For the data cleaning process, I used R Studio to clean the data to ensure that there were no missing values or duplicated data. Since the SleepDay column has time, I 
converted it to a proper date format to ensure data consistency. I renamed the SleepDay column to ActivityDate so both datasets have a common date field. Then, I merged the datasets using both ID and ActivityDate, which allows both datasets' data to be combined accurately for each user on each day.


## 📈 Key Insights

- **Weekend Activity**: Saturday shows the highest average steps, indicating more physical activity during weekends.
- **Weekday Trends**: Monday through Wednesday have high and consistent step counts.
- **TotalMinutesAsleep and SedentaryMinutes** have a stronger inverse relationship, indicating that increased sleeping time correlates with reduced sedentary time.
- Around 75-80% of users have high sleep efficiency, while 15% are medium, and 5-10% fall into low efficiency. 


## 💡 Recommendations

- Launch motivational challenges or promotions focused on increasing activity during low-engagement days (Thursday–Friday).
- Encourage weekend wellness programs or campaigns aligned with users’ natural activity peaks on Saturdays.
- Use Target campaigns around users with medium/low sleep efficiency by promoting Leaf’s sleep tracking and mindfulness features. 


I utilize Tableau Public for the rest of my analysis. I used the cleaned and merged data file that I created using R and imported into Tableau. Use the link below to view my work on Tableau. 

https://public.tableau.com/views/Bellabeat_Case_Study_Analysis/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

