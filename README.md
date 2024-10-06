![](fire.png) ![](sql.png)
# Fire Department Response Analysis
## Problem
Use SQL to process and analyze Fire Department Call Data and extract KPI (Alarm time, Turnout time, Travel time and Response time) by Shift times.

## Solution
- Queried large datasets using complex joins and subqueries.
- Used `CASE` statements to generate KPI's, and Work shifts.
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
  - Sample SQL code:
CREATE VIEW vwFD_Data

		AS

		SELECT *,
	
		DATENAME(MONTH,Create_date) as Month_,
		count(Call_Type) OVER (PARTITION BY YEAR  (Create_Date),MONTH (Create_Date) ORDER BY Create_date) AS  MTD_Incidents,
		count(Call_Type) OVER (PARTITION BY YEAR  (Create_Date) ORDER BY Create_date)  AS YTD_Incidents,
	
		DATENAME(WEEKDAY,Create_date) as DayWeek_,
		DATEPART(WEEKDAY,Create_date) as WeekDay_,
		RIGHT(CONVERT(VARCHAR(19),EventFirstDispatched,0), 7) as Time_,
		/* Code for adding Shift to the table */

		 CASE
			WHEN DATEPART(HOUR,Create_Date) >= 7  AND DATEPART(HOUR,Create_Date) <  15 THEN    '7am-3pm'
			WHEN DATEPART(HOUR,Create_Date) >= 15 AND DATEPART(HOUR,Create_Date) <  23	THEN '3pm-11pm' 
			ELSE '11pm-7am'END ShiftName,
		Count(Call_Type) as CallTypeCount,
		/* code for creating time differences, setting up the KPI */	
		CASE WHEN DATEDIFF(MINUTE,EventFirstDispatched,EventFirstEnroute) <= 1 THEN 1 ELSE 0 END AlarmTime_1Min,
		CASE WHEN DATEDIFF(MINUTE,Create_Date,EventFirstDispatched) <=1 THEN 1 ELSE 0 END TurnOutTime_1Min,
		CASE WHEN DATEDIFF(MINUTE,StartTransport,TransportArrive) <= 8 THEN 1 ELSE 0 END TravelTime_8Mins,
		CASE WHEN DATEDIFF(MINUTE,EventFirstDispatched,EventFirstArrive) <= 11 THEN 1 ELSE 0 END ResponseTime_11Mins

		FROM		dbo.FD_Table
		GROUP BY	 Call_Source,Call_Type,Create_Date,Create_Year,Create_Month,EventFirstDispatched,
					EventFirstArrive,EventFirstEnroute,LastKnownLatitude,LastKnownLongitude,FirstArriveEngine,
					StartTransport,TransportArrive,Close_Date,Municipality,Beat

### _<a href="https://app.powerbi.com/links/QY38agf8U1?ctid=78d1fb89-a6cc-4862-a67c-a7287504e26f&pbi_source=linkShare_blank">Interact with the Fire Department Response Analysis Report here </a>_
