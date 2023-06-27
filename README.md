BellaBeatCaseStudy
# How Can a Wellness Technology Play it Smart
## Scenario
You are a junior data analyst working on the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused
products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the
global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart
device fitness data could help unlock new growth opportunities for the company. You have been asked to focus on one of
Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The
insights you discover will then help guide marketing strategy for the company. You will present your analysis to the Bellabeat
executive team along with your high-level recommendations for Bellabeat’s marketing strategy.

## Products
○ Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress,
menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and
make healthy decisions. The Bellabeat app connects to their line of smart wellness products.

○ Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects
to the Bellabeat app to track activity, sleep, and stress.

○ Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user
activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your
daily wellness.

○ Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are
appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your
hydration levels.

○ Bellabeat membership: Bellabeat also offers a subscription-based membership program for users.
Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and
beauty, and mindfulness based on their lifestyle and goals.

## About the company
Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that manufactures health-focused smart products.
Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around
the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with
knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly
positioned itself as a tech-driven wellness company for women.

By 2016, Bellabeat had opened offices around the world and launched multiple products. Bellabeat products became available
through a growing number of online retailers in addition to their own e-commerce channel on their website. The company
has invested in traditional advertising media, such as radio, out-of-home billboards, print, and television, but focuses on digital
marketing extensively. Bellabeat invests year-round in Google Search, maintaining active Facebook and Instagram pages, and
consistently engages consumers on Twitter. Additionally, Bellabeat runs video ads on Youtube and display ads on the Google
Display Network to support campaigns around key marketing dates.

Sršen knows that an analysis of Bellabeat’s available consumer data would reveal more opportunities for growth. She has
asked the marketing analytics team to focus on a Bellabeat product and analyze smart device usage data in order to gain
insight into how people are already using their smart devices. Then, using this information, she would like high-level
recommendations for how these trends can inform Bellabeat marketing strategy.  

[Follow Along Presentation](Bellabeatppt.pdf)

I. Executive Summary

Bellabeat, a high-tech manufacturer specializing in health-focused products for women, aims to better understand its customers' behavior through the analysis of smart device usage data. Through this understanding, Bellabeat seeks to enhance its marketing strategies and product development. This case study leverages the data from fitness trackers like Fitbit and Bellabeat's Leaf wellness tracker, focusing on metrics such as activity, sleep, and stress.

II. Ask

The key business question we aim to answer is: How are consumers using Bellabeat's smart devices, and what insights can we gather about their health patterns?

III. Prepare

We used the Fitbit Fitness Tracker dataset and data from Bellabeat's devices. The dataset includes measures of physical activity, sleep quality, and heart rate. These data are considered reliable, as they come directly from users' trackers, making it a firsthand source. For the particular data we were interested in we did not have to deal with missing variables or duplicates. We did however, have to create an index for ease of plotting.
[Download archive.zip](https://github.com/quirozfm/Bellabeat-Case-Study/blob/main/archive.zip)

R.

    install.packages("here")
    library(here)
    install.packages("skimr")
    library(skimr)
    install.packages(tidyverse)
    library(tidyverse)
    install.packages("dplyer")
    library(dplyer)
    install.packages("janitor")
    library(janitor)
    library(readr)
    
    GreatValueFitbit <- read_csv("F:/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
    
    
    
  
IV. Process

We performed thorough data cleaning, ensuring the data was suitable for analysis. This involved removing any incomplete or incorrect entries, handling missing values, and ensuring consistency in the data.
~~~
colnames(GreatValueFitbit)

skim_without_charts(GreatValueFitbit)
    
Activity_Distribution <- subset(GreatValueFitbit, select = c("VeryActiveMinutes", "FairlyActiveMinutes", "LightlyActiveMinutes", "SedentaryMinutes"))
summary_Activity_Distribution <- summary(Activity_Distribution)
print(summary_Activity_Distribution)
# Melt the dataframe to long format for plotting
Activity_Distribution_long <- Activity_Distribution %>%
  pivot_longer(cols = everything(), names_to = "ActivityType", values_to = "Minutes")

ggplot(Activity_Distribution_long, aes(x = ActivityType, y = Minutes)) +
  geom_boxplot() +
  theme_minimal() +
  labs(title = "Activity Distribution", x = "Activity Type", y = "Minutes")

GreatValueFitbit$TotalActiveMinutes <- GreatValueFitbit$VeryActiveMinutes + GreatValueFitbit$FairlyActiveMinutes + GreatValueFitbit$LightlyActiveMinutes

ggplot(GreatValueFitbit, aes(x = TotalActiveMinutes, y = Calories)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE, color = "red") +
  labs(title = "Relationship between Activity and Calorie Burn", x = "Total Active Minutes", y = "Calories Burned")

TotalActiveMinutes <- subset (GreatValueFitbit, select = c(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes,Calories))
str(TotalActiveMinutes)

sleep_data <- read.csv("F:/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
sleep <- subset(sleep_data, select =c(TotalTimeInBed, TotalTimeAsleep)
summary(sleep)

sleep_data$Index <- seq.int(nrow(sleep_data))

sleep_data <- tail(sleep_data, 30) 
          
avg_sleep_data <- sleep_data %>%
group_by(Index) %>%
summarise(AverageTimeInBed = mean(TotalTimeInBed),
AverageMinutesAsleep = mean(TotalMinutesAsleep))               

ggplot(avg_sleep_data, aes(x = factor(Index))) +
  geom_bar(aes(y = AverageTimeInBed), stat = "identity", fill = "darkblue", alpha = 0.7, width = 0.4, position = 'dodge') +
  geom_bar(aes(y = AverageMinutesAsleep), stat = "identity", fill = "darkgreen", alpha = 0.7, width = 0.4, position = 'dodge') +
  labs(x = "Index", y = "Average Time (minutes)",
title = "Comparison of Average Time in Bed and Time Asleep") +
theme_minimal()               

~~~
    
V. Analyze

Activity Levels:

We found a strong correlation between physical activity and the calories burned, indicating that as physical activity increased, so did the calories burned. Notably, the majority of users were more lightly active or sedentary. We also found that 'Very Active Minutes' tend to be less common, suggesting that most users are not involved in intense workouts regularly.

![Alt Text](https://github.com/quirozfm/Bellabeat-Case-Study/blob/main/Activity%20Distribution%20Box.jpeg) ![Alt Text](https://github.com/quirozfm/Bellabeat-Case-Study/blob/main/Activity%20Index%20%20scaled.jpeg)
![Alt Text](https://github.com/quirozfm/Bellabeat-Case-Study/blob/main/Activity%20vs%20Calorie%20Burn%20Scatter.jpeg)

Sleep Patterns:

Our analysis showed that users often get less than the recommended 7-9 hours of sleep per night. This lack of sleep can influence overall fitness, energy levels, and other aspects of daily life.

![Alt Text](https://github.com/quirozfm/Bellabeat-Case-Study/blob/main/Sleep%20over%20time.jpeg)

VI. Share

Based on our findings, we would recommend the following strategies:

Promote Active Lifestyle: Our analysis indicates that many users fall into the lightly active or sedentary category. We suggest promoting an active lifestyle with features encouraging movement throughout the day. For example, we could add reminders to stand up or walk around every hour.

Sleep Education: We suggest launching an educational campaign about the importance of sleep for overall health. This campaign can include sleep hygiene tips, benefits of good sleep, and a comparison of users' current sleep patterns with recommended standards.

Enhanced Features: Developing features that allow users to set sleep and activity goals could foster engagement. A reward system for achieving these goals can serve as motivation for users to lead a healthier lifestyle.

VII. Act

Our next steps involve implementing the proposed product features and marketing strategies. We should continue monitoring user data to assess the impact of these changes and adapt our strategies accordingly. Additionally, further analysis could explore correlations between sleep data and other data from the tracker, such as physical activity or heart rate.

VIII. Further Data Exploration

Further, it could be beneficial to collect additional user information like dietary habits, stress levels, and health history. This data could help us understand our users better and guide more personalized product features and marketing strategies. 
