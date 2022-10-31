Bellabeat Data Analysis Case Study
================
Carmen
2022-10-18

## Introduction

Hi, welcome to my Bellabeat data analysis case study. I perform numerous
real-world tasks of a junior data analyst by following the steps of the
data analysis process: Ask, Prepare, Process, Analyze, Share, and Act -
to analyze smart device data and gain an insight into how customers use
their smart devices.

## Who is Bellabeat?

Bellabeat is a high-tech company that focuses on providing women with
health-focused products. Although it is still small, it has the
potential to become a leader in the smart device market. Urska Srsen,
the company’s Chief Creative Officer, believes that analyzing the data
collected by smart devices could help improve the company’s sales and
expand its operations.

## Business Task

Through this case study, i will analyze the data collected by
Bellabeat’s products to gain a deeper understanding of its customers’
behaviors. The following questions will guide my analysis: 1) what are
some trends in smart device usgae?, 2) how could these trends apply to
Bellabeat customers?, 3) How could these trends help influence Bellabeat
marketing strategy? In addition to presenting my findings, i will also
recommend high-level recommendations for Bellabeats marketing strtategy.

## Description of all data sources used

## Preparing the data

I used the Fit Bit Fitness Tracker Data (CC0: Public Domain, data set
made available through Mobius), a public data that explores smart device
users’ daily habits. This Kaggle data set contains personal fitness
tracker from 30 eligible fit bit users, who consented to the submission
of minute-level output for daily activity, steps, heart rate, and sleep
monitoring. This data set has possible limitations, which means i can
consider adding another data to help address such limitations. Because i
am tasked to only focus on one of Bellabeat’s products to analyze smart
device data, i chose the wellness tracker (Leaf/Watch) both of which
focus on tracking activity, sleep, and stress. I decided that data
on:daily activity, daily calories, daily intensities, daily steps, sleep
per day, weight, and heart rate per seconds would best capture insights
into daily wellness.

I installed some R packages for the manipulation, exploration and
visualization of the data:

``` r
install.packages("tidyverse")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("ggplot2")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("dplyr")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("tidyr")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("here")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("lubridate")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("skimr")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

``` r
install.packages("janitor")
```

    ## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
    ## (as 'lib' is unspecified)

After installing packages, i loaded the packages above to make my data
analysis smooth and efficient:

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.5 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggplot2)
library(dplyr)
library(tidyr)
library(here)
```

    ## here() starts at /cloud/project

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(skimr)
library(janitor)
```

    ## 
    ## Attaching package: 'janitor'
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     chisq.test, fisher.test

## Loading CSV files

I then downloaded data set “FitBit Fitness Tracker Data” from Kaggle.
Files downloaded saved as csv files. Before loading the data, need to
compress zip all csv files within downloaded folder to be able to upload
multiple files as one file to the RStudio server, file will
automatically expand after upload.

Then i imported the dataset into RStudio but needes to make functions of
“readr” available for R with command:

``` r
library(readr)
```

### Importing datasets into RStudio and creating dataframes:

``` r
daily_activity <- read_csv("dailyActivity_merged.csv")
```

    ## Rows: 940 Columns: 15
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): ActivityDate
    ## dbl (14): Id, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDi...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_calories <- read_csv("dailyCalories_merged.csv")
```

    ## Rows: 940 Columns: 3
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (2): Id, Calories
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
daily_intensities <- read_csv("dailyIntensities_merged.csv")
```

    ## Rows: 940 Columns: 10
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): ActivityDay
    ## dbl (9): Id, SedentaryMinutes, LightlyActiveMinutes, FairlyActiveMinutes, Ve...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
sleep_per_day <- read_csv("sleepDay_merged.csv")
```

    ## Rows: 413 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): SleepDay
    ## dbl (4): Id, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
weight_log_info <- read_csv("weightLogInfo_merged.csv")
```

    ## Rows: 67 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Date
    ## dbl (6): Id, WeightKg, WeightPounds, Fat, BMI, LogId
    ## lgl (1): IsManualReport
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Once this was done, i then processed the data for analysis by cleaning,
organizing, and formatting it before i could process my data for
analysis:

## Documentation of any cleaning or manipulation of data

To explore the data sets, i first summarized the data to better
understand the distribution of the data and visualize any outliers
within the data set.

### Summary of data attributes

I used the summary function of head() and colnames() to preview the
dataframes. By using these functions, i can get a quick preview of the
data, the structure of the dataframe, and column names for an overview
of the data attributes.

#### Exploring key tables

``` r
head(daily_activity)
```

    ## # A tibble: 6 × 15
    ##       Id Activ…¹ Total…² Total…³ Track…⁴ Logge…⁵ VeryA…⁶ Moder…⁷ Light…⁸ Seden…⁹
    ##    <dbl> <chr>     <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 1.50e9 4/12/2…   13162    8.5     8.5        0    1.88   0.550    6.06       0
    ## 2 1.50e9 4/13/2…   10735    6.97    6.97       0    1.57   0.690    4.71       0
    ## 3 1.50e9 4/14/2…   10460    6.74    6.74       0    2.44   0.400    3.91       0
    ## 4 1.50e9 4/15/2…    9762    6.28    6.28       0    2.14   1.26     2.83       0
    ## 5 1.50e9 4/16/2…   12669    8.16    8.16       0    2.71   0.410    5.04       0
    ## 6 1.50e9 4/17/2…    9705    6.48    6.48       0    3.19   0.780    2.51       0
    ## # … with 5 more variables: VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
    ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>, and
    ## #   abbreviated variable names ¹​ActivityDate, ²​TotalSteps, ³​TotalDistance,
    ## #   ⁴​TrackerDistance, ⁵​LoggedActivitiesDistance, ⁶​VeryActiveDistance,
    ## #   ⁷​ModeratelyActiveDistance, ⁸​LightActiveDistance, ⁹​SedentaryActiveDistance

``` r
head(daily_calories)
```

    ## # A tibble: 6 × 3
    ##           Id ActivityDay Calories
    ##        <dbl> <chr>          <dbl>
    ## 1 1503960366 4/12/2016       1985
    ## 2 1503960366 4/13/2016       1797
    ## 3 1503960366 4/14/2016       1776
    ## 4 1503960366 4/15/2016       1745
    ## 5 1503960366 4/16/2016       1863
    ## 6 1503960366 4/17/2016       1728

``` r
head(daily_intensities)
```

    ## # A tibble: 6 × 10
    ##       Id Activ…¹ Seden…² Light…³ Fairl…⁴ VeryA…⁵ Seden…⁶ Light…⁷ Moder…⁸ VeryA…⁹
    ##    <dbl> <chr>     <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
    ## 1 1.50e9 4/12/2…     728     328      13      25       0    6.06   0.550    1.88
    ## 2 1.50e9 4/13/2…     776     217      19      21       0    4.71   0.690    1.57
    ## 3 1.50e9 4/14/2…    1218     181      11      30       0    3.91   0.400    2.44
    ## 4 1.50e9 4/15/2…     726     209      34      29       0    2.83   1.26     2.14
    ## 5 1.50e9 4/16/2…     773     221      10      36       0    5.04   0.410    2.71
    ## 6 1.50e9 4/17/2…     539     164      20      38       0    2.51   0.780    3.19
    ## # … with abbreviated variable names ¹​ActivityDay, ²​SedentaryMinutes,
    ## #   ³​LightlyActiveMinutes, ⁴​FairlyActiveMinutes, ⁵​VeryActiveMinutes,
    ## #   ⁶​SedentaryActiveDistance, ⁷​LightActiveDistance, ⁸​ModeratelyActiveDistance,
    ## #   ⁹​VeryActiveDistance

``` r
head(sleep_per_day)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay              TotalSleepRecords TotalMinutesAsleep TotalT…¹
    ##        <dbl> <chr>                             <dbl>              <dbl>    <dbl>
    ## 1 1503960366 4/12/2016 12:00:00 AM                 1                327      346
    ## 2 1503960366 4/13/2016 12:00:00 AM                 2                384      407
    ## 3 1503960366 4/15/2016 12:00:00 AM                 1                412      442
    ## 4 1503960366 4/16/2016 12:00:00 AM                 2                340      367
    ## 5 1503960366 4/17/2016 12:00:00 AM                 1                700      712
    ## 6 1503960366 4/19/2016 12:00:00 AM                 1                304      320
    ## # … with abbreviated variable name ¹​TotalTimeInBed

``` r
head(weight_log_info)
```

    ## # A tibble: 6 × 8
    ##           Id Date                  WeightKg Weight…¹   Fat   BMI IsMan…²   LogId
    ##        <dbl> <chr>                    <dbl>    <dbl> <dbl> <dbl> <lgl>     <dbl>
    ## 1 1503960366 5/2/2016 11:59:59 PM      52.6     116.    22  22.6 TRUE    1.46e12
    ## 2 1503960366 5/3/2016 11:59:59 PM      52.6     116.    NA  22.6 TRUE    1.46e12
    ## 3 1927972279 4/13/2016 1:08:52 AM     134.      294.    NA  47.5 FALSE   1.46e12
    ## 4 2873212765 4/21/2016 11:59:59 PM     56.7     125.    NA  21.5 TRUE    1.46e12
    ## 5 2873212765 5/12/2016 11:59:59 PM     57.3     126.    NA  21.7 TRUE    1.46e12
    ## 6 4319703577 4/17/2016 11:59:59 PM     72.4     160.    25  27.5 TRUE    1.46e12
    ## # … with abbreviated variable names ¹​WeightPounds, ²​IsManualReport

#### Identifying all columns in data

``` r
colnames(daily_activity)
```

    ##  [1] "Id"                       "ActivityDate"            
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"

``` r
colnames(daily_calories)
```

    ## [1] "Id"          "ActivityDay" "Calories"

``` r
colnames(daily_intensities)
```

    ##  [1] "Id"                       "ActivityDay"             
    ##  [3] "SedentaryMinutes"         "LightlyActiveMinutes"    
    ##  [5] "FairlyActiveMinutes"      "VeryActiveMinutes"       
    ##  [7] "SedentaryActiveDistance"  "LightActiveDistance"     
    ##  [9] "ModeratelyActiveDistance" "VeryActiveDistance"

``` r
colnames(sleep_per_day)
```

    ## [1] "Id"                 "SleepDay"           "TotalSleepRecords" 
    ## [4] "TotalMinutesAsleep" "TotalTimeInBed"

``` r
colnames(weight_log_info)
```

    ## [1] "Id"             "Date"           "WeightKg"       "WeightPounds"  
    ## [5] "Fat"            "BMI"            "IsManualReport" "LogId"

#### Unique participants in data

``` r
n_distinct(daily_activity$Id)
```

    ## [1] 33

``` r
n_distinct(daily_calories$Id)
```

    ## [1] 33

``` r
n_distinct(daily_intensities$Id)
```

    ## [1] 33

``` r
n_distinct(sleep_per_day$Id)
```

    ## [1] 24

``` r
n_distinct(weight_log_info)
```

    ## [1] 67

``` r
nrow(daily_activity)
```

    ## [1] 940

``` r
nrow(daily_calories)
```

    ## [1] 940

``` r
nrow(daily_intensities)
```

    ## [1] 940

``` r
nrow(sleep_per_day)
```

    ## [1] 413

``` r
nrow(weight_log_info)
```

    ## [1] 67

``` r
daily_activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes,
         LightActiveDistance) %>%
  summary()
```

    ##    TotalSteps    TotalDistance    SedentaryMinutes LightActiveDistance
    ##  Min.   :    0   Min.   : 0.000   Min.   :   0.0   Min.   : 0.000     
    ##  1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 729.8   1st Qu.: 1.945     
    ##  Median : 7406   Median : 5.245   Median :1057.5   Median : 3.365     
    ##  Mean   : 7638   Mean   : 5.490   Mean   : 991.2   Mean   : 3.341     
    ##  3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.:1229.5   3rd Qu.: 4.782     
    ##  Max.   :36019   Max.   :28.030   Max.   :1440.0   Max.   :10.710

``` r
daily_calories %>%  
  select(Calories) %>%
  summary()
```

    ##     Calories   
    ##  Min.   :   0  
    ##  1st Qu.:1828  
    ##  Median :2134  
    ##  Mean   :2304  
    ##  3rd Qu.:2793  
    ##  Max.   :4900

``` r
daily_intensities %>%  
  select(VeryActiveDistance,
         ModeratelyActiveDistance,
         LightActiveDistance,
         SedentaryActiveDistance) %>%
  summary()
```

    ##  VeryActiveDistance ModeratelyActiveDistance LightActiveDistance
    ##  Min.   : 0.000     Min.   :0.0000           Min.   : 0.000     
    ##  1st Qu.: 0.000     1st Qu.:0.0000           1st Qu.: 1.945     
    ##  Median : 0.210     Median :0.2400           Median : 3.365     
    ##  Mean   : 1.503     Mean   :0.5675           Mean   : 3.341     
    ##  3rd Qu.: 2.053     3rd Qu.:0.8000           3rd Qu.: 4.782     
    ##  Max.   :21.920     Max.   :6.4800           Max.   :10.710     
    ##  SedentaryActiveDistance
    ##  Min.   :0.000000       
    ##  1st Qu.:0.000000       
    ##  Median :0.000000       
    ##  Mean   :0.001606       
    ##  3rd Qu.:0.000000       
    ##  Max.   :0.110000

``` r
sleep_per_day %>%  
  select(TotalSleepRecords,
         TotalMinutesAsleep,
         TotalTimeInBed) %>%
  summary()
```

    ##  TotalSleepRecords TotalMinutesAsleep TotalTimeInBed 
    ##  Min.   :1.000     Min.   : 58.0      Min.   : 61.0  
    ##  1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0  
    ##  Median :1.000     Median :433.0      Median :463.0  
    ##  Mean   :1.119     Mean   :419.5      Mean   :458.6  
    ##  3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0  
    ##  Max.   :3.000     Max.   :796.0      Max.   :961.0

``` r
weight_log_info %>%  
  select(WeightPounds) %>%
  summary()
```

    ##   WeightPounds  
    ##  Min.   :116.0  
    ##  1st Qu.:135.4  
    ##  Median :137.8  
    ##  Mean   :158.8  
    ##  3rd Qu.:187.5  
    ##  Max.   :294.3

After transforming the data to work with, i explored and identify trends
and relationships

## Summmary of analysis

Because i was interested in the relationship between daily activity,
modalities of daily intensities, and sleep, i plotted the following:

``` r
ggplot(data=daily_activity, aes(x=TotalSteps, y=SedentaryMinutes)) + geom_point()
```

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

``` r
ggplot(data=daily_intensities, aes(x=SedentaryActiveDistance, y=VeryActiveDistance)) + geom_point()
```

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

``` r
ggplot(data=sleep_per_day, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + geom_point()
```

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
\## Merging datasets together Note that datasets have the ‘Id’ field -
this can be used to merge the datasets.

``` r
combined_data <- full_join(sleep_per_day, daily_activity, daily_intensities, by ="Id")
```

``` r
n_distinct(combined_data$Id)
```

    ## [1] 33

## Supporting visualization and key findins

``` r
ggplot()+
  geom_point(data=combined_data, mapping = aes(x=TotalMinutesAsleep, y=VeryActiveDistance), color="blue")+
  geom_point(data=combined_data, mapping = aes(x=TotalMinutesAsleep, y=LightActiveDistance), color="orange")+
  scale_y_continuous(limits=c(0, 15),
                     breaks=c(0, 5, 10, 15))+
  scale_x_continuous(limits=c(0, 800),
                     breaks=c(0, 100, 200, 300, 400, 500, 600, 700, 800))+
  stat_smooth(method=loess)
```

    ## Warning: Removed 227 rows containing missing values (geom_point).
    ## Removed 227 rows containing missing values (geom_point).

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

``` r
ggplot(data=combined_data, aes(x=TotalMinutesAsleep, y=VeryActiveDistance)) + geom_point()+
    scale_y_continuous(limits=c(0, 15),
                     breaks=c(0, 4, 8, 12, 15))+
  scale_x_continuous(limits=c(0, 800),
                     breaks=c(0, 100, 200, 300, 400, 500, 600, 700, 800))+
  geom_smooth(method = loess)
```

    ## `geom_smooth()` using formula 'y ~ x'

    ## Warning: Removed 227 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 227 rows containing missing values (geom_point).

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

``` r
ggplot(data=combined_data, aes(x=TotalMinutesAsleep, y=LightActiveDistance)) + geom_point()+
    scale_y_continuous(limits=c(0, 15),
                     breaks=c(0, 4, 8, 12, 15))+
  scale_x_continuous(limits=c(0, 800),
                     breaks=c(0, 100, 200, 300, 400, 500, 600, 700, 800))+
  geom_smooth(method = loess)
```

    ## `geom_smooth()` using formula 'y ~ x'

    ## Warning: Removed 227 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 227 rows containing missing values (geom_point).

![](Bellabeat-Data-Analysis-Case-Study_files/figure-gfm/unnamed-chunk-31-1.png)<!-- -->
\## Key findings from analysis After plotting my explorations of
interest - the bidirectional relationship between sleep and exercise. I
was interested in whether the modality of daily activity improved sleep
quality or duration, or whether it had a negative impact on sleep. I
found that those who sleep more, are more likely to be active, however,
at a lower modality than those who sleep less and have a very active
day. According to the sample, those who slept about 7.5 hours a day,
managed a 4 mile light active distance. Those who slept aabout 5.8
hours, managed a 2 mile very active distance. Additionally, there was a
decrease in amount in active distance. For example, at 6.7 hours, there
is a sudden drop in active distance, but then slowly starts to increase
with more hours slept. On the other hand, light active distance plateus
until about 10 hours of sleep. After that, light activity slowly
decreases.

## Top high-level content recommensations based on analysis

i focused this analysis on data related to the products: Leaf and Time,
both of which tracked data on actvity, sleep, and stress. Both connect
to the Bellabeat app to provide users with insights into their daily
wellness. However, these products are not included (or at least did not
get the impression) to be a part of the membership that gives users 24/7
access to personalized guidance on their activity, nutrition, sleep,
health, beauty, and mindfulness on their goals or lifestyle preference.
Because most people would most likely choose one or a few of the
products to purchase, if they do not choose the membership, they do not
have other forms of personalized guidance. Because of so, i recommend
adding a functionality that allows the user to personalize and take
charge of their physical and mental well-being without needing to pay a
membership fee. The functionality needs to encourage active lifestyles,
offer personalized guidance, and set reminders (e.g., to sleep, be
active, etc.), allow tracking of many other things other than just
activity, stress, and sleep. Because sleep is very dependent on ones
age, health status, and even mode of active intensity, it is important
to be able to keep track of things like calories, weight, intensity, and
duration of someones active day to help us better understand our
behaviors and approaches to working on them (if we so choose to).
