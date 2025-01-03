# Introduction

In this project I am going to go through the queries I have run in this analysis about a bicycle rental company in Chicago. This study is part of the Google Data Analytics Certificate programme. To view the SQL queries I used for this case study, please navigate to the "SQL code" folders. To view the full analysis, go to: -website-

# Business task

As a member of the Cyclistic marketing analytics team, our ultimate goal is to develop a marketing strategy that maximises the number of annual memberships. In order to achieve that, we need to find out in which ways causal riders and annual members use Cyclistic similarly or differently. By understanding our customers’ needs, we can tailor our marketing strategy to convert casual riders into annual membership holders, and thus ensuring the company’s future success. Our key stakeholders in this study will be Lilly Moreno, director marketing and the Cyclistic executive board.

# Preparation of data

The data I am using in this analysis is open data, readily available for the public (see licence agreement in Appendix), however to grant the reliability and consistency of the data, the analysis will be completed based on a copy of the data which is stored on a hard drive, protected by a password. This ensures no one is able to modify the data and therefore skew the results. I will be working with the most recent available data (January 2023 to September 2024) to ensure the data is current. This primary data is provided by the company itself, granting reliability, originality and comprehensibility. 

Initially I imported a smaller segment of the data into Google Sheets, and upon inspection I realised this is a very large dataset, so I decided to use Big Query (SQL) instead. During the preparation of the data, I uploaded each csv file into buckets in BigQuery, as the files were too big to upload directly from the computer.

# Process

1. First, for consistency, I checked how many characters the ride_id contain, then I counted the number of ride_id’s. The length is 16 characters and the number of ride_id’s are 10450717 (ride_id is primary key).
2. Then I counted all the DISTINCT ride_id’s and compared it to the previous number. The result came up as 10450506, meaning there will be 211 ride_id’s that are duplicates. I’ll delete these later.
3. I checked the allowed rideable_types, which is 4: electric_bike, docked_bike, electric_scooter and classic_bike.
4. Then I selected all the trips that were less than a minute, or more than a day long, as these could be outliers and errors. A less than a minute ride won’t give us much insight.
5. Next, I checked the start station and end station names, looking for nulls and inconsistencies. The query revealed that there are 1810 start stations, and 1757126 rows had NULL as their start station. There are 1821 end stations, and 1835918 rows had NULL as their end station. Running a query for DISTINCT values confirmed these numbers and revealed that there are 1752 distinct start_ids and 1764 distinct end_ids.
6. This time I checked for NULL values in start station names,  end station names, and latitudes/longitudes. The query revealed that there are 2736400 trips with NULL values in the station names, these will have to be removed. Another query revealed there are 13250 rows with missing coordinates, these will have to be removed too.
7. Finally I confirmed there are 2 types of members: member or casual.

Now that we understand the data better, we can start the cleaning process by removing rows that have missing values or outliers.

# Cleaning

1. First I removed all “classic bike” trips with their start station name or start station id values as NULL. This removed 1,757,224 rows. Then I repeated this step using end station names and end station ids. This removed 979,142 rows. 
2. I repeated the first step, but this time for “docked bikes”. The reason why I’m not deleting “electric scooter” records is because they don’t require a docking station to operate. This query didn’t delete any rows. I double checked with another query if this is corrected, and it seemed “docked bike” trips all had start/end station and start/end id values.
3. Then, I removed all the rows with either missing start latitude/longitude or end latitude/longitude values. This removed 116 rows.
4. Next, I replaced the docked_bike values with classic_bike as all classic bikes have to be returned to a docking station. This meant creating a new table with an extra column for the updated rideable types, then transferring all the columns except the old rideable_type column to another, updated new table.
5. Then, I made sure maintenance rides and testing data was removed from the table, as they could skew the analysis on customers’ trips.
6. In this step, I deleted all rows where the length of the trip was less than a minute or longer than a day. These outliers could skew the analysis as they are not relevant to how most customers use Cyclistic. This query removed 255,820 rows.
7. Then, to help with the analysis I created extra columns for trip duration (in minutes), day of the week, and month, and saved the results as a new table from which I’ll carry out my analysis. The cleaned, updated table now has 7458381 rows compared to the 8693459 rows the old table had. This means 1235078 rows were cleaned.

# Analysis

1. First, I created a new table to aggregate the number of trips taken by each membership type and each bike type. I found that both casuals and members most preferred classic bikes, and least preferred electric scooters. The number of trips taken by members on a classic bike is almost twice the amount of trips taken by casual riders. 
2. Next, I created a table of the number of trips broken down by months and membership types. Among casual riders, July was the most popular month, whereas for members, August saw the most trips.
3. Then I repeated the process, this time broken down to days of week. I found that among casual riders, Saturday was the most popular day, whereas for members, weekdays were the busiest.
4. In a new table, I repeated the process to find out the busiest times of day for each user type. I found that the 16:00 to 18:00 time period was most popular for both casual riders and members, however the number of trips by members were almost double that of casual riders. 
5. Then, I calculated the average trip length per day per rider type. I found that the average trip length for members was very similar throughout the weekdays, and slightly longer on weekends. On average, casual riders take longer trips than members, with weekends being the most popular for the longest trips.
6. Next, I created a table for the most popular starting docking stations among casual riders. Turns out, the 2 most popular stations are Bissell St & Armitage Ave and Racine Ave & Belmont Ave.
7. I repeated the same process for members too. The most popular stations were Paulina Ave & North Ave and Broadway & Waveland Ave.
8. Then, I searched for the most popular ending stations amongst casual riders by creating another table. Wells St & Elm St and Sedgwick St & North Ave were the most favoured stations. 
9. I repeated the same process for members. The top 2 ending stations were Wells St & Huron St and St. Clair St & Erie St.
10. This time I was looking at bikes that don’t need a docking station to operate. Therefore the most popular spots can only be described by coordinates. Amongst casual riders, these were the top 2 locations: 41.884621072579357 / -87.627834230661392 and 41.918084 / -87.643749.
11. Amongst members, all locations only occurred once, so our query was inconclusive. 

The "Share" and "Act" phase of the project with visualisations can be viewed on my website at: -website-


