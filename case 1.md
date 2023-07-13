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

```
-- Data Exploration

-- checking the data types of all columns

SELECT column_name, data_type
FROM `2022_tripdata`.INFORMATION_SCHEMA.COLUMNS
WHERE table_name = 'combined_data';

-- checking for number of null values in all columns

SELECT COUNT(*) - COUNT(ride_id) ride_id,
 COUNT(*) - COUNT(rideable_type) rideable_type,
 COUNT(*) - COUNT(started_at) started_at,
 COUNT(*) - COUNT(ended_at) ended_at,
 COUNT(*) - COUNT(start_station_name) start_station_name,
 COUNT(*) - COUNT(start_station_id) start_station_id,
 COUNT(*) - COUNT(end_station_name) end_station_name,
 COUNT(*) - COUNT(end_station_id) end_station_id,
 COUNT(*) - COUNT(start_lat) start_lat,
 COUNT(*) - COUNT(start_lng) start_lng,
 COUNT(*) - COUNT(end_lat) end_lat,
 COUNT(*) - COUNT(end_lng) end_lng,
 COUNT(*) - COUNT(member_casual) member_casual
FROM `2022_tripdata.combined_data`;

-- checking for duplicate rows

SELECT COUNT(ride_id) - COUNT(DISTINCT ride_id) AS duplicate_rows
FROM `2022_tripdata.combined_data`;

-- ride_id - all have length of 16

SELECT LENGTH(ride_id) AS length_ride_id, COUNT(ride_id) AS no_of_rows
FROM `2022_tripdata.combined_data`
GROUP BY length_ride_id;

-- rideable_type - 3 unique types of bikes

SELECT DISTINCT rideable_type, COUNT(rideable_type) AS no_of_trips
FROM `2022_tripdata.combined_data`
GROUP BY rideable_type;

-- started_at, ended_at - TIMESTAMP - YYYY-MM-DD hh:mm:ss UTC

SELECT started_at, ended_at
FROM `2022_tripdata.combined_data`
LIMIT 10;

SELECT COUNT(*) AS longer_than_a_day
FROM `2022_tripdata.combined_data`
WHERE (
  EXTRACT(HOUR FROM (ended_at - started_at)) * 60 +
  EXTRACT(MINUTE FROM (ended_at - started_at)) +
  EXTRACT(SECOND FROM (ended_at - started_at)) / 60) >= 1440;   -- longer than a day - total rows = 5360

SELECT COUNT(*) AS less_than_a_minute
FROM `2022_tripdata.combined_data`
WHERE (
  EXTRACT(HOUR FROM (ended_at - started_at)) * 60 +
  EXTRACT(MINUTE FROM (ended_at - started_at)) +
  EXTRACT(SECOND FROM (ended_at - started_at)) / 60) <= 1;      -- less than a minute - total rows = 122283

-- start_station_name, start_station_id - total 833064 rows with both start station name and id missing

SELECT DISTINCT start_station_name
FROM `2022_tripdata.combined_data`
ORDER BY start_station_name;

SELECT COUNT(ride_id) AS rows_with_start_station_null          -- return 833064 rows
FROM `2022_tripdata.combined_data`
WHERE start_station_name IS NULL OR start_station_id IS NULL;

-- end_station_name, end_station_id - total 892742 rows with both end station name and id missing

SELECT DISTINCT end_station_name
FROM `2022_tripdata.combined_data`
ORDER BY end_station_name;

SELECT COUNT(ride_id) AS rows_with_null_end_station          -- return 892742 rows
FROM `2022_tripdata.combined_data`
WHERE end_station_name IS NULL OR end_station_id IS NULL;

-- end_lat, end_lng - total 5858 rows with both missing

SELECT COUNT(ride_id) AS rows_with_null_end_loc
FROM `2022_tripdata.combined_data`
WHERE end_lat IS NULL OR end_lng IS NULL;

-- member_casual - 2 unique values - member and casual riders

SELECT DISTINCT member_casual, COUNT(member_casual) AS no_of_trips
FROM `2022_tripdata.combined_data`
GROUP BY member_casual;
```































