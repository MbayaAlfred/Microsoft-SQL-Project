![](fire.png) ![](sql.png)

# Fire Department Emergency Call Analysis
## Problem

Use SQL to process and analyze Fire Department Call Data and extract Industry KPI ike the number of incidents, response times, and shift categorization based on the time of day.
This KPI are based on Alarm time, Turnout time, Travel time and Response time

## Solution
- Queried large datasets using complex joins and subqueries.
- Used `CASE` statements to flag suspicious transactions.
- Created summary reports using SQL `GROUP BY` and aggregation functions.

## Key Components of pre-aggregated data
- Month and Day Information using Date Functions
  DATENAME(MONTH, Create_Date) as Month_: This gives you the name of the month.
  DATENAME(WEEKDAY, Create_Date) as DayWeek_: Extracts the name of the weekday.
  MTD_Incidents: Counts the incidents month-to-date using a WINDOW function (PARTITION BY YEAR, MONTH).
  YTD_Incidents: Counts the incidents year-to-date.
  
- CASE statement
  Assign the appropriate shift (7am-3pm, 3pm-11pm, or 11pm-7am) based on the hour of the Create_Date.
  Generate KPI
  AlarmTime_1Min: Whether the unit was dispatched within 1 minute.
  TurnOutTime_1Min: Whether the unit responded within 1 minute.
  TravelTime_8Mins: Whether transport took less than 8 minutes.
  ResponseTime_11Mins: Whether the overall response time was under 11 minutes.
- Grouping:
  The query groups by multiple fields such as Call_Type, Create_Date, EventFirstDispatched, etc., which ensures that you can apply aggregate functions like COUNT.

## SQL Techniques Used
  - CASE statemens
  - Date functions
  - Aggregation
  - Window function (partition)
### [Visit the Fire Department's Complete Power BI Report](https://www.firedept.org)
