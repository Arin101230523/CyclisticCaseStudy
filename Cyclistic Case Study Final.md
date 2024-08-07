---
author: Arin Sood
date: 2024-05-31
editor_options:
  markdown:
    wrap: 72
output:
  html_document:
    theme: cerulean
    toc: true
    toc_depth: 2
title: Cyclistic Case Study
---

## **INTRODUCTION**

Hey!

This is my take on the capstone project for Google's [Data Analytics
Professional
Certificate](https://www.coursera.org/google-certificates/data-analytics-certificate).
As part of the final course, I will be showcasing all the skills I
learned throughout the program into a practice case study. Using a
real-world business problem, I will be tackling it with the **Ask,
Prepare, Process, Share, and Act** methodology. Any feedback would be
greatly appreciated!

`<br>`{=html}

## **SCENARIO**

You are a junior data analyst working on the marketing analyst team at
Cyclistic, a bike-share company in Chicago. The director of marketing
believes the company's future success depends on maximizing the number
of annual memberships. Therefore, your team wants to understand how
casual riders and annual members use Cyclistic bikes differently. From
these insights, your team will design a new marketing strategy to
convert casual riders into annual members. But first, Cyclistic
executives must approve your recommendations, so they must be backed up
with compelling data insights and professional data visualizations.

### **Characters and teams**

● **Cyclistic**: A bike-share program that features more than 5,800
bicycles and 600 docking stations. Cyclistic sets itself apart by also
offering reclining bikes, hand tricycles, and cargo bikes, making
bike-share more inclusive to people with disabilities and riders who
can't use a standard two-wheeled bike. The majority of riders opt for
traditional bikes; about 8% of riders use the assistive options.
Cyclistic users are more likely to ride for leisure, but about 30% use
the bikes to commute to work each day.

● **Lily Moreno**: The director of marketing and your manager. Moreno is
responsible for the development of campaigns and initiatives to promote
the bike-share program. These may include email, social media, and other
channels.

● **Cyclistic marketing analytics team**: A team of data analysts who
are responsible for collecting, analyzing, and reporting data that helps
guide Cyclistic marketing strategy. You joined this team six months ago
and have been busy learning about Cyclistic's mission and business
goals---as well as how you, as a junior data analyst, can help Cyclistic
achieve them.

● **Cyclistic executive team**: The notoriously detail-oriented
executive team will decide whether to approve the recommended marketing
program.

### **About the company**

In 2016, Cyclistic launched a successful bike-share offering. Since
then, the program has grown to a fleet of 5,824 bicycles that are
geotracked and locked into a network of 692 stations across Chicago. The
bikes can be unlocked from one station and returned to any other station
in the system anytime.

Until now, Cyclistic's marketing strategy relied on building general
awareness and appealing to broad consumer segments. One approach that
helped make these things possible was the flexibility of its pricing
plans: single-ride passes, full-day passes, and annual memberships.
Customers who purchase single-ride or full-day passes are referred to as
casual riders. Customers who purchase annual memberships are Cyclistic
members.

Cyclistic's finance analysts have concluded that annual members are much
more profitable than casual riders. Although the pricing flexibility
helps Cyclistic attract more customers, Moreno believes that maximizing
the number of annual members will be key to future growth. Rather than
creating a marketing campaign that targets all-new customers, Moreno
believes there is a solid opportunity to convert casual riders into
members. She notes that casual riders are already aware of the Cyclistic
program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at
converting casual riders into annual members. In order to do that,
however, the team needs to better understand how annual members and
casual riders differ, why casual riders would buy a membership, and how
digital media could affect their marketing tactics. Moreno and her team
are interested in analyzing the Cyclistic historical bike trip data to
identify trends.

`<br>`{=html}

## **ASK**

#### **Three questions will guide the future marketing program:**

1.  How do annual members and casual riders use Cyclistic bikes
    differently?

2.  Why would casual riders buy Cyclistic annual memberships?

3.  How can Cyclistic use digital media to influence casual riders to
    become members?

Moreno has assigned you the first question to answer: **How do annual
members and casual riders use Cyclistic bikes differently?**

`<br>`{=html}

## **DELIVERABLES**

1.  A clear statement of the business task

2.  A description of all data sources used

3.  Documentation of any cleaning or manipulation of data

4.  A summary of your analysis

5.  Supporting visualizations and key findings

6.  Your top three recommendations based on your analysis

`<br>`{=html}

#### Let's get started!

`<br>`{=html}

## **PREPARATION**

The [data](https://divvy-tripdata.s3.amazonaws.com/index.html) that will
be used in this analysis is provided by Divvy, a real bike-share
company, from which Cyclistic is based on and is organized into csv
files by month. Details regarding its public use can be found on
[Divvy's License
Agreement](https://divvybikes.com/data-license-agreement).

To keep this analysis as real as possible, I will be using the last 12
months of data available to me (at this time is April 2023 - March
2024).

`<br>`{=html}

## **PROCESSING**

We will first begin by loading in the libraries needed for analyzing
Cyclistic's historical data.

``` {r}
library(tidyverse) 
library(geosphere) 
library(lubridate)
library(leaflet)
```

`<br>`{=html}

Next we will import the 12 most recent datasets (April 2023 - April
2024).

``` {r}
CyclisticData202304 <- read.csv("~/Data/202304-divvy-tripdata.csv")
CyclisticData202305 <- read.csv("~/Data/202305-divvy-tripdata.csv")
CyclisticData202306 <- read.csv("~/Data/202306-divvy-tripdata.csv")
CyclisticData202307 <- read.csv("~/Data/202307-divvy-tripdata.csv")
CyclisticData202308 <- read.csv("~/Data/202308-divvy-tripdata.csv")
CyclisticData202309 <- read.csv("~/Data/202309-divvy-tripdata.csv")
CyclisticData202310 <- read.csv("~/Data/202310-divvy-tripdata.csv")
CyclisticData202311 <- read.csv("~/Data/202311-divvy-tripdata.csv")
CyclisticData202312 <- read.csv("~/Data/202312-divvy-tripdata.csv")
CyclisticData202401 <- read.csv("~/Data/202401-divvy-tripdata.csv")
CyclisticData202402 <- read.csv("~/Data/202402-divvy-tripdata.csv")
CyclisticData202403 <- read.csv("~/Data/202403-divvy-tripdata.csv")
CyclisticData202404 <- read.csv("~/Data/202404-divvy-tripdata.csv")
```

After examining the data, we see that the relative structure of columns
and data types between each report remains consistent. Therefore, we can
combine these datasets into a new dataframe.

``` {r}
CyclisticMerged <- rbind(CyclisticData202304, CyclisticData202305, CyclisticData202306, CyclisticData202307, CyclisticData202308, CyclisticData202309, CyclisticData202310, CyclisticData202311, CyclisticData202312, CyclisticData202401, CyclisticData202402, CyclisticData202403, CyclisticData202404)

head(CyclisticMerged)
```

As we can see our merged dataset was a success. Before we begin cleaning
the data, let's get some important metrics about our dataset.

``` {r}
glimpse(CyclisticMerged)
summary(CyclisticMerged)
```

Key takeaways:

-   6,165,202 rides recorded
-   13 different metrics for each ride
-   Ride length can be measured by started_at and ended_at d/tt/m
    variables
-   Start-lat, start-lng, end_lat, end_lng are ways to visualize the
    geospatial landscape of rides
-   ride_id is used to identify unique riders
-   Rides can be grouped by their type, member status, and the starting
    and ending station ids

However, there is one big concern we need to address. Namely, rides are
not all grouped by the rider_id created, so we cannot measure how
frequently the person behind the ID(s) uses it.

With this in mind, let's move on to the cleaning step.

`<br>`{=html}

## **CLEANING**

Let's start by using the distinct function to remove duplicate rider_ids
while preserving the columns.

``` {r}
CyclisticMergedDupRemoved <- CyclisticMerged %>%
  distinct(ride_id, .keep_all = TRUE)

print(paste("Removed", nrow(CyclisticMerged)-nrow(CyclisticMergedDupRemoved), "duplicate rows"))
```

Now let's create a new column to measure the ride time by finding the
difference between the end and start time columns.

``` {r}
CyclisticMergedDupRemoved$ride_time_calc <- (as.double(difftime(CyclisticMerged$ended_at, CyclisticMerged$started_at))) / 60
```

Now let's see a summary of the data:

``` {r}
summary(CyclisticMergedDupRemoved$ride_time_calc)
```

Now from our newly created ride_time_calc column, we should remove any
outliers such as [rides under 60
seconds](https://divvybikes.com/system-data) and any especially lengthy
rides so that we do not severely skew our data. Let's first see how our
data is skewed and move on from there.

``` {r}
quantile_ride_time_calc = quantile(CyclisticMergedDupRemoved$ride_time_calc, seq(0, 1, by=0.01))
quantile_ride_time_calc
```

Seeing this, we can see that we should remove any values less than 1
minute, and any values greater than 120 minutes to avoid skew

``` {r}
CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  filter(ride_time_calc >=1) %>%
  filter(ride_time_calc <=120)

summary(CyclisticMergedDupRemoved$ride_time_calc)
```

Now we can create a two new columns to show what day of the week each
ride was and what month it took place.

``` {r}
CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  mutate(weekday = strftime(CyclisticMergedDupRemoved$started_at, "%A"))

CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  mutate(month = strftime(CyclisticMergedDupRemoved$started_at, "%B"))

head(CyclisticMergedDupRemoved)
```

We can then change this new column to a factor for faster analysis.

``` {r}
CyclisticMergedDupRemoved$weekday <- as.factor(CyclisticMergedDupRemoved$weekday)

CyclisticMergedDupRemoved$month <-
  as.factor(CyclisticMergedDupRemoved$month)

class(CyclisticMergedDupRemoved$weekday)
nlevels(CyclisticMergedDupRemoved$weekday)
summary(CyclisticMergedDupRemoved$weekday)

class(CyclisticMergedDupRemoved$month)
nlevels(CyclisticMergedDupRemoved$month)
summary(CyclisticMergedDupRemoved$month)
```

Let's also add a column that stores the start time of these rides.

``` {r}
CyclisticMergedDupRemoved$started_at <- ymd_hms(CyclisticMergedDupRemoved$started_at)

CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  mutate(hour_started = floor_date(started_at, unit = "hour"))
CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  mutate(hour_started = strftime(CyclisticMergedDupRemoved$hour_started, "%R"))

head(CyclisticMergedDupRemoved)
```

Likewise, let's make this into a factor for faster analysis later on.
**Note,** NA's occur for starting stations due to electric bikes as they
do not need to be docked.

``` {r}
CyclisticMergedDupRemoved$hour_started <- as.factor(CyclisticMergedDupRemoved$hour_started)

class(CyclisticMergedDupRemoved$hour_started)
nlevels(CyclisticMergedDupRemoved$hour_started)
summary(CyclisticMergedDupRemoved$hour_started)
```

Lastly, let's create a new column to measure the distance between start
and ending coordinates. **Note,** some rows contain NA for ending
station and starting due to electric bikes not being required to be
docked.

``` {r}
CyclisticMergedDupRemoved$distance_km <-
  distGeo(matrix(c(CyclisticMergedDupRemoved$start_lng, CyclisticMergedDupRemoved$start_lat), ncol=2), matrix(c(CyclisticMergedDupRemoved$end_lng, CyclisticMergedDupRemoved$end_lat), ncol=2)) / 1000

summary(CyclisticMergedDupRemoved$distance_km)
```

We see that the max is particularly high, let's see what the head of a
descended sort of this data looks like so we can remove high outliers.

``` {r}
CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  arrange(desc(distance_km))
head(CyclisticMergedDupRemoved$distance_km, n = 20)
```

We see that there is only one extremely high distance value so we can
remove it to avoid skew.

``` {r}
CyclisticMergedDupRemoved <- CyclisticMergedDupRemoved %>%
  slice(-(1))

head(CyclisticMergedDupRemoved$distance_km, n = 10)
summary(CyclisticMergedDupRemoved$distance_km)
```

Let's also change **member_casual** to a factor for better analysis
later on.

``` {r}
CyclisticMergedDupRemoved$member_casual <- as.factor(CyclisticMergedDupRemoved$member_casual)
 
 class(CyclisticMergedDupRemoved$member_casual)
 nlevels(CyclisticMergedDupRemoved$member_casual)
 summary(CyclisticMergedDupRemoved$member_casual)
```

For the final step, we will remove irrelevant columns such as the
started_at and ended_at due to us already finding the time differences.

``` {r}
CyclisticCleaned <- CyclisticMergedDupRemoved %>%
  select(-c(started_at, ended_at))

head(CyclisticCleaned)
summary(CyclisticCleaned)
```

With all that done, we will now move on to the analysis step!

`<br>`{=html}

## **ANALYSIS**

\*\*

Now it is time for my favorite step, analysis. Using the packages and
libraries we installed above, we will be grouping the data to create
visualizations for potential insights.

First, let's begin by looking at the distribution of member types.

``` {r}
member_casual_view <- CyclisticCleaned %>%
  group_by(member_casual) %>%
  summarize(
    count = n(),
    percentage = round(count / nrow(CyclisticCleaned) * 100, 2),
    .groups = "drop"
  )

options(scipen = 999, repr.plot.width = 12, repr.plot.height = 8)

ggplot(member_casual_view, aes(x = "", y = count, fill = member_casual)) +
  geom_col(width = 0.6, color = "white") +
  geom_text(
    aes(label = paste0(count, " (", percentage, "%)")),
    position = position_stack(vjust = 0.5),
    size = 5,
    fontface = "bold"
  ) +
  labs(
    title = "Ride Nums. by User Class",
    subtitle = "April 2023 - March 2024",
  ) +
  theme_void(base_size = 17) +
  coord_polar(theta = "y") +
  scale_fill_manual(values = c("#FF6F61", "#6B5B95"), name = NULL) +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold"),
    plot.subtitle = element_text(size = 12, hjust = 0.5),
    legend.position = "right",
    legend.spacing.x = unit(0.5, "cm")
  )
```

There is a large difference in the number of casual vs. member users,
approximately 1.75 million. Let's delve deeper and separate each month
by its rides and separate those rides by the membership status of users.

``` {r}
CyclisticCleaned$month <- ordered(CyclisticCleaned$month, levels = 
                                    c("April", "May", "June", "July", "August", "September", "October", "November", "December", "January", "February", "March"))

member_casual_monthly <- CyclisticCleaned %>%
  group_by(month, member_casual) %>%
  summarize(count = n(), .groups = "drop") %>%
  arrange(month, member_casual)

options(repr.plot.width = 12, repr.plot.height = 6)

custom_colors <- c("#FF6F61", "#6B5B95")

ggplot(data = member_casual_monthly, aes(x = month, y = count, fill = member_casual)) +
  geom_col(position = "dodge", width = 0.7) +
  labs(title = "Monthly Ride Counts by Membership Class", subtitle = "(April 2023 - March 2024)", x = "Month", y = "Num. of Rides") +
  scale_fill_manual(values = custom_colors, name = "User Type:") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5, size = 16, face = "bold"), plot.subtitle = element_text(hjust = .5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

There are a few key takeaways from this data:

-   The total number of rides peaks in the summer months (April -
    August) with members being the highest and casual riders not being
    far behind
-   In the less popular winter months, a significant portion of all
    rides are through members

Let's now look deeper through the popularity of rides through each
weekday.

``` {r}
CyclisticCleaned$weekday <- ordered(CyclisticCleaned$weekday, levels = c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))

weekday_member_casual <- CyclisticCleaned %>%
  group_by(weekday, member_casual) %>%
  summarize(count = n(), .groups = "drop") %>%
  arrange(weekday, member_casual)

custom_colors <- c("#FF6F61", "#6B5B95")

ggplot(data = weekday_member_casual, aes(x = weekday, y = count, fill = member_casual), show.legend = FALSE) +
  geom_col(position = "dodge", width = .8) +
  labs(title = "Ride Counts Each Day of the Week", x = "Day", y = "Num. of Rides", subtitle = "(April 2023 - March 2024)") +
  scale_fill_manual(values = custom_colors, name = "User Class") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5, size = 16, face = "bold"), plot.subtitle = element_text(hjust = .5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

As we can see through the data, throughout the work week the number of
members dominates the number of rides. During the weekends, the number
of casuals increases and almost reaches the number of members.

This makes sense as those with set schedules would likely plan to use
these bikes and thus secure a membership while weekend plans are more
spontaneous and encourages tourism, leading to a shift in the numbers.

Let's further examine the times that these bikes are used during each
weekday.

``` {r}
hourly_member_casual_weekday <- CyclisticCleaned%>%
  filter(weekday %in% c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")) %>%
  group_by(hour_started, member_casual)%>%
  summarize(count = n(), .groups = "drop")%>%
  arrange(hour_started, member_casual)

custom_colors <- c("#FF6F61", "#6B5B95")

ggplot(data = hourly_member_casual_weekday, aes(x = hour_started, y = count, group = member_casual, color = member_casual)) + 
  geom_point() + 
  geom_line() + 
  scale_color_manual(values = custom_colors) +
  labs(title = "Ride Count by Time of Day Weekdays", subtitle = "(April 2023 - March 2024)", x = "Hour", y = "Num. of Rides") + 
  guides(color = guide_legend(title = "User Class")) + 
  theme(text = element_text(size = 12), axis.text.x = element_text(angle = 90), plot.title = element_text(hjust = 0.5, size = 16, face = "bold"), plot.subtitle = element_text(hjust = .5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

``` {r}
hourly_member_casual_weekend <- CyclisticCleaned %>%
  filter(weekday %in% c("Saturday", "Sunday")) %>%
  group_by(hour_started, member_casual) %>%
  summarize(count = n(), .groups = "drop") %>%
  arrange(hour_started, member_casual)

custom_colors <- c("#FF6F61", "#6B5B95")

ggplot(data = hourly_member_casual_weekend, aes(x = hour_started, y = count, group = member_casual, color = member_casual)) + 
  geom_point() + 
  geom_line() +
  scale_color_manual(values = custom_colors) +
  labs(title = "Ride Count by Time of Day Weekends", subtitle = "(April 2023 - March 2024)", x = "Hour", y = "Num. of Rides") + 
  guides(color = guide_legend(title = "User Class")) + 
  theme(text = element_text(size = 12), axis.text.x = element_text(angle = 90), plot.title = element_text(hjust = 0.5, size = 16, face = "bold"), plot.subtitle = element_text(hjust = .5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

As we can see, the weekends tend to be more rounded, indicating a loose
structure in when bikes are used the most, while the weekdays are more
rigid and sharp, indicating scheduled times. We can also see that during
the weekdays, 12:00 seems to be a peak.

Now let's see which bike type is more favored by member types. **Note,**
docked bikes were later changed to be included into classic bikes.

``` {r}
casual_classic <- CyclisticCleaned %>%
  filter(member_casual == "casual" & (rideable_type == "classic_bike" | rideable_type == "docked_bike"))

casual_electric <- CyclisticCleaned %>%
  filter(member_casual == "casual" & rideable_type == "electric_bike")

count_casual_classic <- nrow(casual_classic)
count_casual_electric <- nrow(casual_electric)

total_casual_riders <- count_casual_classic + count_casual_electric

pie_data <- data.frame(
  category = c("Classic", "Electric Bike"),
  count = c(count_casual_classic, count_casual_electric),
  percentage = c(count_casual_classic/total_casual_riders, count_casual_electric/total_casual_riders)
)

ggplot(pie_data, aes(x = "", y = count, fill = category)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar("y", start = 0) +
  geom_text(aes(label = paste0(count, " (", round(percentage * 100), "%)")), position = position_stack(vjust = 0.5), fontface = "bold") +
  labs(title = "Casual Riders by Bike Type") +
  scale_fill_manual(values = c("#0077FF", "#FF7700"), name = "Bike Type") +
  theme_void() +
  theme(plot.title = element_text(hjust = 0.5, size = 16, face = "bold"))
```

``` {r}
member_classic <- CyclisticCleaned %>%
  filter(member_casual == "member" & (rideable_type == "classic_bike" | rideable_type == "docked_bike"))

member_electric <- CyclisticCleaned %>%
  filter(member_casual == "member" & rideable_type == "electric_bike")

count_member_classic <- nrow(member_classic)
count_member_electric <- nrow(member_electric)

total_member_riders <- count_member_classic + count_member_electric

pie_data <- data.frame(
  category = c("Classic", "Electric Bike"),
  count = c(count_member_classic, count_member_electric),
  percentage = c(count_member_classic/total_member_riders, count_member_electric/total_member_riders)
)

ggplot(pie_data, aes(x = "", y = count, fill = category)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar("y", start = 0) +
  geom_text(aes(label = paste0(count, " (", round(percentage * 100), "%)")), position = position_stack(vjust = 0.5), fontface = "bold") +
  labs(title = "Member Riders by Bike Type") +
  scale_fill_manual(values = c("#00CC66", "#9966FF"), name = "Bike Type") +
  theme_void() +
  theme(plot.title = element_text(hjust = 0.5, size = 16, face = "bold"))
```

They seem to be relatively similar across the board. Let's see if
comparing ride lengths for both member types on each bike type can offer
better insights.

``` {r}
casual_bikes <- CyclisticCleaned %>%
  filter(member_casual == "casual") %>%
  mutate(bike_type = ifelse(rideable_type %in% c("classic_bike", "docked_bike"), "classic/docked", "electric_bike")) %>%
  group_by(hour_started, bike_type) %>%
  summarize(average_ride_time = mean(ride_time_calc), .groups = "drop") %>%
  arrange(hour_started, bike_type)

custom_colors <- c("classic/docked" = "#0077FF", "electric_bike" = "#FF7700")

ggplot(data = casual_bikes, aes(x = hour_started, y = average_ride_time, group = bike_type, color = bike_type)) + 
  geom_point() + 
  geom_line() + 
  scale_color_manual(values = custom_colors) +
  labs(title = "Average Ride Time by Hour Started for Casual Riders",
       subtitle = "(April 2023 - March 2024)",
       x = "Hour",
       y = "Average Ride Time (minutes)",
       color = "Bike Type") + 
  theme_minimal() +
  theme(text = element_text(size = 12),
        axis.text.x = element_text(angle = 90),
        plot.title = element_text(hjust = 0.5, size = 14, face = "bold"),
        plot.subtitle = element_text(hjust = 0.5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

``` {r}
member_bikes <- CyclisticCleaned %>%
  filter(member_casual == "member") %>%
  mutate(bike_type = ifelse(rideable_type %in% c("classic_bike", "docked_bike"), "classic/docked", "electric_bike")) %>%
  group_by(hour_started, bike_type) %>%
  summarize(average_ride_time = mean(ride_time_calc), .groups = "drop") %>%
  arrange(hour_started, bike_type)

custom_colors <- c("classic/docked" = "#00CC66", "electric_bike" = "#9966FF")

ggplot(data = member_bikes, aes(x = hour_started, y = average_ride_time, group = bike_type, color = bike_type)) + 
  geom_point() + 
  geom_line() + 
  scale_color_manual(values = custom_colors) +
  labs(title = "Average Ride Time by Hour Started for Members",
       subtitle = "(April 2023 - March 2024)",
       x = "Hour",
       y = "Average Ride Time (minutes)",
       color = "Bike Type") + 
  theme_minimal() +
  theme(text = element_text(size = 12),
        axis.text.x = element_text(angle = 90),
        plot.title = element_text(hjust = 0.5, size = 14, face = "bold"),
        plot.subtitle = element_text(hjust = 0.5, size = 12, face = "bold"),
        axis.title = element_text(size = 12),
        legend.position = "top",
        legend.title = element_text(size = 12),
        legend.text = element_text(size = 10))
```

Interestingly enough, it seems casual riders that chose to use classic
bikes would spend much more time to electric bike casual riders than
member riders. This can be especially seen between 5-10 am.

One possible cause is that members are more experienced with riding
bikes and thus can maintain a higher speed. Both membership types were
consistent in showing classic bikes being slower than electric, which
makes sense due to their differences in speed.

Lastly, let's take a look at the geospatial differences between starting
points of casual riders and member riders for round trips. We will be
using the leaflet library and we will first separate and summarize
distances where starting and ending were the same and different
respectively before combining them to create a display.

Since we made the coordinate columns factors, we can group them by these
coordinates and remove any that are less than specified values to make
visualization easier and more efficient.

``` {r}
distance_summarized <- CyclisticCleaned %>%
  drop_na(distance_km)

distance_0 <- distance_summarized %>%
  filter(distance_km==0)

route_summary_0 <- distance_0 %>%
  group_by(start_lng, start_lat, end_lng, end_lat, member_casual) %>%
  summarize(total = n(), .groups="drop") %>%
  filter(total>75) %>%
  arrange(desc(total))

route_summarized <- distance_summarized %>%
  filter(!distance_km==0) %>%
  group_by(start_lng, start_lat, end_lng, end_lat, member_casual) %>%
  summarize(total = n(), .groups = "drop") %>%
  filter(total>100) %>%
  arrange(desc(total))

data_for_clustering <- bind_rows(route_summary_0, route_summarized)
```

``` {r}

normalize_radius <- function(total, min_radius=2, max_radius=20) {
  min_total <- min(total)
  max_total <- max(total)
  normalized <- ((total - min_total) / (max_total - min_total)) * (max_radius - min_radius) + min_radius
  return(normalized)
}

casual_data <- data_for_clustering %>%
  filter(member_casual=="casual") %>%
  mutate(radius = normalize_radius(total))

leaflet(data = casual_data) %>%
  addTiles() %>%
  addCircleMarkers(~start_lng, ~start_lat, 
             radius = ~radius,
             fillOpacity = .3,
             stroke = TRUE,
             color = "#FF6F61")
```

``` {r}
member_data <- data_for_clustering %>%
  filter(member_casual == "member") %>%
  mutate(radius = normalize_radius(total))

leaflet(data = member_data) %>%
  addTiles() %>%
  addCircleMarkers(
    ~start_lng, ~start_lat,
    radius = ~radius,
    fillOpacity = 0.3,
    stroke = TRUE,
    color = "#6B5B95"
  )
```

It seems that for both user types, the area near Lake Michigan seems to
be the most used starting area with casual members having large amounts
of users starting in areas with high tourism such as ports.

Through these visualizations, we have determined several key insights
into the differences of behaviors between casual and member users. Now
it is time to summarize these findings and present our data-driven
recommendations for a targeted marketing campaign into a digestable
manner.

`<br>`{=html}

## **SHARE & ACT**

Our goal was to find insights and report findings on how casual and
member riders use Cyclistic bicycles differently. From our analysis of
rides from April 2023 to March 2024, we found several similarities and
differences between these two groups.

**Similarities:**

-   Ride numbers between both user groups share a similar positive
    correlation in spring & summer months (April-August) in comparison
    to winter months (December-March) and is likely due to weather
    conditions in Chicago
-   Ride times during weekdays for both user types were more rigid,
    peaking primarily during early morning and especially lunch hours
    (12:00 peak) while ride times during weekends were less structured
    and peaking from early morning to roughly 2 pm
-   Both groups do not show a strong preference for either electric or
    classic bikes though casual riders have a slightly greater
    preference for electric bikes (Casual: C - 47%, E - 53%, Members:
    C - 51%, E - 49%)
-   Ride lengths for both user types for both bicycle types indicated
    users spending more time on classic bikes than electric bikes
    (likely due to speed differences)
-   Round trip rides starting locations for both casual and member users
    were primarily around the Lake Michigan area with casual riders
    having a stronger preference for tourist heavy areas such as ports

**Differences:**

-   A significant portion of all rides are done via users who are
    members (64.66%) rather than casual riders (35.34%) across the
    5,967,282 rides recorded
-   In all months, members conducted more rides on Cyclistic bicycles
    with a significant peak occurring in April, perhaps due to the
    beginning of spring weather. In summer months, casual rider counts
    were closer to their member counterparts in comparison to winter
    months.
-   Casual ride counts were much higher and closer to their member
    counterparts during the weekends, likely due to tourists spending
    time in Chicago on their days off, in comparison to being roughly
    half their member counterparts during weekdays. Member rides peaked
    during the work week (Tuesday-Thursday) and were at their lowest
    during the weekend
-   Likewise, ride counts during weekends by time showed members and
    casual being extremely similar. On weekdays, they shared similar
    patterns with members making up a significant portion of those rides
-   Average ride times between classic and electric bikes between both
    member classes showed classic bikes having longer lengths. However,
    members were shown to have a closer gap to their electric
    counterparts
-   Casual riders were more likely to return to their starting location
    in comparison to members
-   Popular casual rider locations were slightly more skewed to more
    areas of Chicago such as parks while popular member locations were
    more strictly oriented toward the Lake Michigan area

With these similarities and differences, we can create character
profiles for both casual and member riders.

**Casual Riders:**

-   Seem to often be tourists or locals looking for recreational rides
    through Cyclistic with a preference for scenic and tourist-heavy
    areas, especially around Lake Michigan, parks, and downtown areas
-   Tend to love the warmer spring and summer months and are much more
    active on weekends, likely using their days off to explore more of
    the city
-   Have a slight preference for electric bikes (53%)
-   Have more round trips and longer ride times, indicating their use of
    Cyclistic bicycles as a means for exercising
-   Have a small portion that behave similar to member riders on
    weekdays and may be convinced to switch to a membership

**Member Riders:**

-   Seem to be regular users who reside in the Chicago area primarily
    using Cyclistic bicycles for daily commuting
-   Make a significant portion of the Cyclistic userbase and have a very
    noticeable peak in April months, perhaps due to spring weather
    beginning
-   Are very active during the weekdays, especially Tuesday-Thursday,
    but drop down during the weekends
-   Have large amounts of riders during early morning and especially
    lunch hours (12:00)
-   Do not seem to have a strong preference for electric or classic
    bikes
-   Less likely to return to their starting location
-   Have a smaller time difference in ride lengths between different
    bike types
-   Are more strictly oriented toward the scenic Lake Michigan area

`<br>`{=html}

With this in mind, let's move on to the recommendations

`<br>`{=html}

## **RECOMMENDATIONS**

1.  Create a marketing campaign and fitness reward program targeted at
    casual users. A significant portion of these riders see Cyclistic as
    a fun and cost-effective way to explore the city and get exercise
    done during the weekends and other holidays. By marketing Cyclistic
    as **THE** platform for introducing people to the world of biking,
    we can push more users to try biking through our platform and
    convert to a membership to hit goals. We can also add a rewards plan
    and refer-a-friend bonuses so people feel more inclined to continue
    to use Cyclistic. Furthermore, as these users tend to explore and
    start from tourist heavy areas in downtown and the Lake Michigan
    area, advertisements positioned in this locations would be most
    impactful. To amplify these efforts, we can explore collaborations
    with popular tourist influencers to better promote our platform and
    appeal for this target audience.

2.  Highlight the cost effectiveness of member plans vs. casual riders
    who frequent Cyclistic as their main mode of commuting
    transportation through weekly metrics. Currently, there exists a
    small subset of casual riders whose behavior mimics members very
    closely during weekdays in terms of ride times. Despite this, they
    seem to continue to rely on casual membership plans. By underscoring
    the potential savings and additional benefits offered by member
    plans, such as lower rates per ride and access to exclusive
    features, this initiative aims to encourage these riders to consider
    upgrading to membership plans, enhancing user satisfaction and
    increasing the membership base.

3.  Introduce seasonal offerings, marketing campaigns, and membership
    types for both casual and existing members. Our data shows that
    summer months, particularly April and May-August, have the highest
    number of riders from both user types. We can leverage this into
    creating a seasonal membership option with additional benefits that
    is only active during these months so casual riders not wanting to
    commit to an annual membership will be more likely to switch to a
    seasonal membership. These benefits could include discounted rates,
    bonus ride credits, or exclusive access to summer events or
    promotions. Additionally, we can use the knowledge that the influx
    of casual riders in summer months likely arises from tourists and
    offer them a tourist membership bundled with benefits with other
    attractions. To effectively promote these offerings, customized
    marketing campaigns will be implemented, utilizing targeted
    advertising on social media platforms, email newsletters, and
    partnerships with local tourist attractions. By implementing these
    strategies, Cyclistic can effectively cater to the preferences and
    behaviors of its diverse user base, drive membership conversion, and
    enhance overall user satisfaction.

`<br>`{=html}

### **CLOSING NOTES**

That about wraps up this case study. I had a lot of fun making this and
would love to hear any feedback from anyone who happened to make it to
the end. Thanks!
