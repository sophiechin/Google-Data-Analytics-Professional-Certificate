# Google Data Analytics Professional Capstone
### Case Study 1: How Does A Bike-Share Navigate Speedy Success?


## 1. Introduction

Cyclistic is a bike-share program that offers inclusive options such as reclining bikes, hand tricycles, and cargo bikes, making it accessible to people with disabilities. While most riders choose traditional bikes, about 8% use the assistive options. The program is popular for leisure rides, but around 30% of users also rely on it for daily commuting. Cyclistic aims to shift its marketing focus towards converting casual riders into annual members as they have proven to be more profitable. By analyzing historical bike trip data, the marketing team aims to understand the differences between annual members and casual riders, determine why casual riders would consider a membership, and explore the impact of digital media on marketing strategies.

## 2. Scenario

As a junior data analyst on the marketing team at Cyclistic, a bike-share company in Chicago, the director of marketing believes that the company's success hinges on maximizing annual memberships. Your team's objective is to gain an understanding of how casual riders and annual members utilize Cyclistic bikes differently. With these insights, you will develop a new marketing strategy to convert casual riders into annual members. However, your recommendations must be supported by compelling data insights and professional data visualizations in order to obtain approval from Cyclistic executives.

## 3. Data Analysis Process

In this case study, the six steps of the data analysis process will be used in order to solve this problem. Those 6 steps include:
1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

### Step 1: Ask
**Key Task**: Create marketing tactics aimed at converting casual riders into Cyclistic members.

So here are the analysis questions that will help in the analysis:

1. How do annual members and casual riders use Cyclistic bikes differently?
2. What are the causes that drive casual riders to buy Cyclistic annual memberships?
3. How can Cyclistic use digital media as a tool to convert casual riders to members?

For this case, Lily Moreno who is the director of marketing has assigned me as a junior data analyst to the first question: *How do annual members and casual riders use Cyclistic bikes differently?*

### Step 2: Prepare

This step will address the data source that will be used for the analysis and the organization of the data structure.

**Data Source**:  Cyclistic’s historical trip data from Jan 2022 to Dec 2022 which is a public dataset published by Motivate International Inc. will be used to analyze and identify trends. [Click Here](https://divvy-tripdata.s3.amazonaws.com/index.html) for the dataset.

**Data Information**: In the data source, there are 12 files in total following the naming convention of "YYYYMM-divvy-tripdata". Each file contains data for a specific month, including other details such as ride ID, bike type, start time, end time, start station, end station, start location, end location, and member status. The corresponding column names are:
- ride_id
- rideable_type
- started_at
- ended_at
- start_station_name
- start_station_id
- end_station_name
- end_station_id
- start_lat
- start_lng
- end_lat
- end_lng
- member_casual

### Step 3: Process

**Tool**: Google BigQuery is used to combine the total 12 files into one dataset.

#### Data Combination

To help with data combination, the following SQL query is implemented in order to combine all 12 files into a single dataset:

```
DROP TABLE IF EXISTS `2022_tripdata_combined`;

CREATE TABLE IF NOT EXISTS `2022_tripdata_combined` AS 
(
  SELECT * FROM `2022_tripdata_202201_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202202_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202203_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202204_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202205_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202206_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202207_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202208_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202209_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202210_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202211_tripdata`
  UNION ALL
  SELECT * FROM `2022_tripdata_202212_tripdata`
);

```

Then, check the row numbers:

```
SELECT COUNT(*)
FROM `2022_tripdata_combined`;

```

### Data Exploration

In order to do data exploration, the first thing to do is to check the data type to observe the inconsistencies:

```
SELECT column_name, data_type
FROM `2022_tripdata`.INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'combined_data';
```



































