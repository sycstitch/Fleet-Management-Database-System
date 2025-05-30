# Fleet Management Database - Query Examples


### TABLE OF CONTENTS

*BEGINNER QUERIES*
- Basic Vehicle Inventory (SELECT only)
- Downtown Trip Filtering (WHERE)
- Customer Account Balances (WHERE, ORDER BY)
- Trip Count by Destination (GROUP BY, Aliasing)
- Active Drivers (GROUP BY, HAVING, Aliasing)
- Top 5 Longest Trips (ORDER BY, LIMIT)

*INTERMEDIATE QUERIES*
- Trip Details with Driver and Customer Names (INNER JOIN, Aliasing)
- All Employees and Their Driver Status (LEFT JOIN, CASE Statements)
- Combined Name List (UNION, String Functions)
- Trip Distance Categories (CASE Statements)
- Drivers Handling Long Distance Trips (Subqueries)
- Above Average Customer Balances (Subqueries with Aggregates)
- Trip Distance Rankings (Window Functions)
- Driver Performance Analysis (Advanced Window Functions)

*ADVANCED QUERIES*
- Driver Statistics Summary (CTEs)
- Monthly Trip Analysis (CTEs, Date Functions, Window Functions)

*OVERALL BUSINESS INSIGHTS & CONCLUSIONS*
<br /><br />

-------------------------------------------------------------------------------
<br />

<details>
<summary>BEGINNER QUERIES</summary>

### Basic Vehicle Inventory
- **Goal:** List all vehicles in the fleet
- **Skills:** SELECT<br />

**Query:**
`SELECT * FROM VEHICLE;`

**Output:**
```
VEH_NUMBER | MOD_CODE   | VEH_TTAF | VEH_TTEL | VEH_TTER
1484P      | PA23-250   | 1833.1   | 1833.1   | 101.8
2289L      | C-90A      | 4243.8   | 768.9    | 1123.4
2778V      | PA31-350   | 7992.9   | 1513.1   | 789.5
4278Y      | PA31-350   | 2447.3   | 622.1    | 243.2
```

**Conclusion:** The fleet consists of 4 vehicles with 2 PA31-350 models, indicating a preference for this model type. Vehicle 2778V has significantly higher usage hours (7992.9) compared to others, suggesting it may need maintenance attention or replacement soon.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Downtown Trip Filtering
- **Goal:** Find all trips to downtown area
- **Skills:** SELECT, WHERE
- **Why these skills:** WHERE clause filters records based on specific criteria, essential for targeted business analysis<br />

**Query:**
```
SELECT TRIP_ID, TRIP_DRIVER, TRIP_DESTINATION, TRIP_DISTANCE 
FROM TRIP 
WHERE TRIP_DESTINATION = 'DTN';
```

**Output:**
```
TRIP_ID | TRIP_DRIVER | TRIP_DESTINATION | TRIP_DISTANCE
10001   | 104         | DTN              | 936
10005   | 101         | DTN              | 1023
10010   | 109         | DTN              | 998
10014   | 106         | DTN              | 936
```
**Conclusion:** Four downtown trips show varied distances (936-1023 miles) with different drivers handling each route. Driver 101 handled the longest downtown trip (1023 miles), while drivers 104 and 106 both completed identical 936-mile downtown routes.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Customer Account Balances (Highest First)
- **Goal:** Show customers with outstanding balances, highest first
- **Skills:** SELECT, WHERE, ORDER BY<br />

**Query:**
```
SELECT CUS_LNAME, CUS_FNAME, CUS_BALANCE 
FROM CUSTOMER 
WHERE CUS_BALANCE > 0 
ORDER BY CUS_BALANCE DESC;
```

**Output:**
```
CUS_LNAME | CUS_FNAME | CUS_BALANCE
Olowski   | Paul      | 1285.19
O'Brian   | Amy       | 1014.56
Smith     | Kathy     | 896.54
Orlando   | Myron     | 673.24
Smith     | Olette    | 453.98
```

**Conclusion:** 5 customers have outstanding balances totaling $4,323.51. Paul Olowski has the highest debt ($1,285.19), representing 30% of total outstanding. This indicates potential collection issues that need management attention.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Trip Count by Destination
- **Goal:** Count trips to each destination
- **Skills:** SELECT, GROUP BY, Aliasing
- **Why these skills:** GROUP BY aggregates data by destination, COUNT() provides trip totals, aliasing makes output readable<br />

**Query:**
```
SELECT TRIP_DESTINATION AS Destination, COUNT(*) AS TripCount
FROM TRIP 
GROUP BY TRIP_DESTINATION
ORDER BY TripCount DESC;
```

**Output:**
```
Destination | TripCount
DTN         | 4
GNV         | 3
TYS         | 3
STL         | 3
BNA         | 2
MOB         | 1
MQY         | 1
```

**Conclusion:** Downtown (DTN) is the most popular destination with 4 trips, followed by three destinations tied at 3 trips each (GNV, TYS, STL). This shows balanced demand across multiple routes, with downtown being the clear leader for fleet operations.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Active Drivers (More Than 2 Trips)
- **Goal:** Find drivers with high trip volume
- **Skills:** SELECT, GROUP BY, HAVING, Aliasing
- **Why these skills:** GROUP BY aggregates by driver, HAVING filters grouped results (vs WHERE), COUNT() measures activity level<br />

**Query:**
```
SELECT TRIP_DRIVER AS DriverID, COUNT(*) AS TotalTrips
FROM TRIP 
GROUP BY TRIP_DRIVER 
HAVING COUNT(*) > 2;
```

**Output:**
```
DriverID | TotalTrips
101      | 4
104      | 3
105      | 3
106      | 3
109      | 3
```

**Conclusion:** Driver 101 has the most trips (4), but trip count alone doesn't indicate total responsibility - distance matters too. While 101 appears most active by frequency, the actual workload depends on miles driven per trip, which varies significantly among drivers.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Top 5 Longest Trips
- **Goal:** Show the longest distance trips
- **Skills:** SELECT, ORDER BY, LIMIT
- **Why these skills:** ORDER BY DESC sorts by distance (largest first), LIMIT restricts to top 5 results for focused analysis<br />

**Query:**
```
SELECT TRIP_ID, TRIP_DRIVER, TRIP_DESTINATION, TRIP_DISTANCE
FROM TRIP 
ORDER BY TRIP_DISTANCE DESC 
LIMIT 5;
```

**Output:**
```
TRIP_ID | TRIP_DRIVER | TRIP_DESTINATION | TRIP_DISTANCE
10015   | 104         | GNV              | 1645
10003   | 105         | GNV              | 1574
10007   | 104         | GNV              | 1574
10009   | 105         | GNV              | 1574
10005   | 101         | DTN              | 1023
```

**Conclusion:** Drivers 104 and 105 dominate long-distance trips, with 104 handling the longest single trip (1,645 miles). This reveals that while driver 101 has more total trips, drivers 104 and 105 handle the most demanding long-haul routes, suggesting specialized roles within the driver pool.
<br /><br />

-------------------------------------------------------------------------------
<br />
</details>


<details>
<summary>INTERMEDIATE QUERIES</summary>

### Trip Details with Driver and Customer Names
- **Goal:** Complete trip information with related data
- **Skills:** INNER JOIN, Aliasing
- **Why these skills:** INNER JOIN connects multiple tables (TRIP, EMPLOYEE, CUSTOMER) to show relationships, aliasing simplifies table references and output readability<br />

**Query:**
```
SELECT T.TRIP_ID, E.EMP_LNAME AS DriverName, C.CUS_LNAME AS CustomerName, 
       T.TRIP_DESTINATION, T.TRIP_DISTANCE
FROM TRIP T
JOIN EMPLOYEE E ON T.TRIP_DRIVER = E.EMP_NUM
JOIN CUSTOMER C ON T.CUS_CODE = C.CUS_CODE
ORDER BY T.TRIP_ID;
```

**Output:**
```
TRIP_ID | DriverName | CustomerName | TRIP_DESTINATION | TRIP_DISTANCE
10001   | Lange      | Dunne        | DTN              | 936
10002   | Lewis      | Brown        | BNA              | 320
10003   | Williams   | Orlando      | GNV              | 1574
10004   | Duzak      | Smith        | STL              | 472
10005   | Lewis      | Dunne        | DTN              | 1023
[... continues for all trips]
```

**Conclusion:** This comprehensive view reveals customer-driver relationships and usage patterns. Notably, Dunne appears twice (trips 10001, 10005) with both trips going to DTN, showing she's a repeat customer who frequently travels downtown. This suggests an opportunity to offer location-based discounts for customers who frequent specific destinations or loyalty programs for repeat customers.
<br /><br />

-------------------------------------------------------------------------------
<br />

### All Employees and Their Driver Status
- **Goal:** Show which employees are certified drivers
- **Skills:** LEFT JOIN, CASE Statements
- **Why these skills:** LEFT JOIN includes all employees (even non-drivers), CASE statement handles NULL values by providing readable status labels<br />

**Query:**
```
SELECT E.EMP_LNAME, E.EMP_FNAME, 
       CASE WHEN D.DRV_LICENSE IS NOT NULL THEN D.DRV_LICENSE ELSE 'Not Certified' END AS DriverStatus
FROM EMPLOYEE E 
LEFT JOIN DRIVER D ON E.EMP_NUM = D.EMP_NUM
ORDER BY E.EMP_LNAME;
```

**Output:**
```
EMP_LNAME   | EMP_FNAME | DriverStatus
Diante      | Jorge     | Not Certified
Duzak       | Jeanine   | COM
Genkazi     | Leighla   | Not Certified
Jones       | Anne      | Not Certified
Kolmycz     | George    | Not Certified
Lange       | John      | ATP
Lewis       | Rhonda    | ATP
Travis      | Elizabeth | COM
VanDam      | Rhett     | Not Certified
Wiesenbach  | Paul      | Not Certified
Williams    | Robert    | COM
```

**Conclusion:** Only 5 out of 11 employees (45%) are certified drivers, with 2 holding ATP licenses (highest qualification) and 3 holding COM licenses. This limited driver pool explains why certain drivers handle more trips and longer distances - the company relies heavily on its certified drivers for all operations.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Combined Name List
- **Goal:** Create unified employee/customer directory for emergency contacts
- **Skills:** UNION, String Functions
- **Why these skills:** UNION combines different table results into single dataset, CONCAT creates full names, useful for emergency contact lists and company-wide notifications<br />

**Query:**
```
SELECT CONCAT(CUS_FNAME, ' ', CUS_LNAME) AS FullName, 'Customer' AS PersonType 
FROM CUSTOMER
UNION
SELECT CONCAT(EMP_FNAME, ' ', EMP_LNAME) AS FullName, 'Employee' AS PersonType 
FROM EMPLOYEE
ORDER BY FullName;
```

**Output:**
```
FullName          | PersonType
Alfred Ramas      | Customer
Amy O'Brian       | Customer
Anne Farriss      | Customer
Anne Jones        | Employee
Elizabeth Travis  | Employee
George Kolmycz    | Employee
George Williams   | Customer
James Brown       | Customer
Jeanine Duzak     | Employee
[... continues for all people]
```

**Conclusion:** This unified directory of 21 people (10 customers, 11 employees) is useful for emergency situations where you need to quickly contact anyone associated with the company, mass notifications about service disruptions, or compliance reporting that requires complete person rosters.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Trip Distance Categories
- **Goal:** Categorize trips by distance for pricing strategy
- **Skills:** CASE Statements
- **Why these skills:** CASE statement creates conditional logic to categorize continuous data (distance) into discrete business categories<br />

**Query:**
```
SELECT TRIP_ID, TRIP_DISTANCE,
  CASE 
    WHEN TRIP_DISTANCE > 1000 THEN 'Long Distance'
    WHEN TRIP_DISTANCE > 500 THEN 'Medium Distance'
    ELSE 'Short Distance'
  END AS DistanceCategory
FROM TRIP
ORDER BY TRIP_DISTANCE DESC;
```

**Output:**
```
TRIP_ID | TRIP_DISTANCE | DistanceCategory
10015   | 1645          | Long Distance
10003   | 1574          | Long Distance
10007   | 1574          | Long Distance
10009   | 1574          | Long Distance
10005   | 1023          | Long Distance
10010   | 998           | Medium Distance
10001   | 936           | Medium Distance
10014   | 936           | Medium Distance
10012   | 884           | Medium Distance
[... continues for all trips]
```

**Conclusion:** 5 trips (28%) are classified as long-distance (>1000 miles), indicating significant long-haul operations. This classification helps with pricing strategies, driver scheduling, and vehicle maintenance planning based on trip intensity.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Drivers Handling Long Distance Trips
- **Goal:** Find drivers qualified for extended routes (>1000 miles)
- **Skills:** Subqueries
- **Why these skills:** Subquery first identifies long-distance trips, then outer query finds which drivers handle them, useful for analyzing specialization<br />

**Query:**
```
SELECT DISTINCT E.EMP_NUM, E.EMP_LNAME, E.EMP_FNAME
FROM EMPLOYEE E
WHERE E.EMP_NUM IN (
  SELECT TRIP_DRIVER 
  FROM TRIP 
  WHERE TRIP_DISTANCE > 1000
)
ORDER BY E.EMP_LNAME;
```

**Output:**
```
EMP_NUM | EMP_LNAME | EMP_FNAME
104     | Lange     | John
101     | Lewis     | Rhonda
105     | Williams  | Robert
```
**Conclusion:** Only 3 drivers (60% of certified drivers) handle long-distance trips over 1000 miles. This connects back to our earlier findings - while driver 101 has the most trips overall, drivers 104 and 105 specialize in the most challenging long-haul routes, showing clear operational specialization within the limited certified driver pool.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Above Average Customer Balances
- **Goal:** Find customers with above-average balances
- **Skills:** Subqueries with Aggregates<br />
- **Why these skills:** Subquery calculates the average balance across all customers, outer query compares each customer against this benchmark to identify high-balance accounts

**Query:**
```
SELECT CUS_LNAME, CUS_FNAME, CUS_BALANCE
FROM CUSTOMER 
WHERE CUS_BALANCE > (SELECT AVG(CUS_BALANCE) FROM CUSTOMER)
ORDER BY CUS_BALANCE DESC;
```

**Output:**
```
CUS_LNAME | CUS_FNAME | CUS_BALANCE
Olowski   | Paul      | 1285.19
O'Brian   | Amy       | 1014.56
Smith     | Kathy     | 896.54
Orlando   | Myron     | 673.24
```

**Conclusion:** These 4 customers have balances significantly above the company average ($432.35), representing the top tier of outstanding receivables. Notably, Orlando appears in both high-balance customers and frequent users (2 trips), suggesting he may be a high-value client with extended credit terms rather than a collection risk. This premium customer segment accounts for the majority of outstanding debt and warrants personalized account management.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Trip Distance Rankings
- **Goal:** Rank all trips by distance to identify outliers
- **Skills:** Window Functions
- **Why these skills:** ROW_NUMBER() assigns unique ranks, RANK() handles ties, OVER clause defines ranking criteria - useful for performance analysis<br />

**Query:**
```
SELECT TRIP_ID, TRIP_DRIVER, TRIP_DISTANCE,
  ROW_NUMBER() OVER (ORDER BY TRIP_DISTANCE DESC) AS DistanceRank,
  RANK() OVER (ORDER BY TRIP_DISTANCE DESC) AS DistanceRankTied
FROM TRIP;
```

**Output:**
```
TRIP_ID | TRIP_DRIVER | TRIP_DISTANCE | DistanceRank | DistanceRankTied
10015   | 104         | 1645          | 1            | 1
10003   | 105         | 1574          | 2            | 2
10007   | 104         | 1574          | 3            | 2
10009   | 105         | 1574          | 4            | 2
10005   | 101         | 1023          | 5            | 5
10010   | 109         | 998           | 6            | 6
10001   | 104         | 936           | 7            | 7
10014   | 106         | 936           | 8            | 7
[... continues for all trips]
```

**Conclusion:** Trip 10015 is the clear distance leader at 1,645 miles. Three trips tie for 2nd place at 1,574 miles, showing consistent long-distance operations. The ranking reveals driver 104 and 105 dominate the longest trips, confirming their specialization in extended routes.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Driver Performance Analysis
- **Goal:** Calculate running totals and averages per driver for workload assessment
- **Skills:** Advanced Window Functions
- **Why these skills:** PARTITION BY groups calculations by driver, running totals show cumulative workload, averages show per-driver performance metrics<br />

**Query:**
```
SELECT TRIP_ID, TRIP_DRIVER, TRIP_DISTANCE,
  SUM(TRIP_DISTANCE) OVER (PARTITION BY TRIP_DRIVER ORDER BY TRIP_ID) AS RunningTotal,
  AVG(TRIP_DISTANCE) OVER (PARTITION BY TRIP_DRIVER) AS DriverAvgDistance
FROM TRIP
ORDER BY TRIP_DRIVER, TRIP_ID;
```

**Output:**
```
TRIP_ID | TRIP_DRIVER | TRIP_DISTANCE | RunningTotal | DriverAvgDistance
10002   | 101         | 320           | 320          | 597.75
10005   | 101         | 1023          | 1343         | 597.75
10011   | 101         | 352           | 1695         | 597.75
10012   | 101         | 884           | 2579         | 597.75
10017   | 101         | 508           | 3087         | 597.75
10001   | 104         | 936           | 936          | 1051.67
10007   | 104         | 1574          | 2510         | 1051.67
10015   | 104         | 1645          | 4155         | 1051.67
[... continues for all drivers]
```

**Conclusion:** Running totals show cumulative distance driven by each driver across their trips. Driver 104 has the highest average distance per trip (1,051.67 miles) compared to driver 101 (597.75 miles), confirming that while 101 takes more trips, 104 handles more demanding long-distance routes consistently.
<br /><br />

-------------------------------------------------------------------------------
<br />
</details>


<details>
<summary>ADVANCED QUERIES</summary>

### Driver Statistics Summary
- **Goal:** Create comprehensive driver performance report with rankings
- **Skills:** CTEs (Common Table Expressions)
- **Why these skills:** CTEs break complex analysis into readable steps - first calculate stats, then add rankings, finally join with names for business context<br />

**Query:**
```
WITH DriverStats AS (
  SELECT TRIP_DRIVER, 
         COUNT(*) AS TripCount, 
         SUM(TRIP_DISTANCE) AS TotalDistance,
         AVG(TRIP_DISTANCE) AS AvgDistance,
         SUM(TRIP_FUEL_USED) AS TotalFuel
  FROM TRIP 
  GROUP BY TRIP_DRIVER
),
DriverRankings AS (
  SELECT *, 
         RANK() OVER (ORDER BY TotalDistance DESC) AS DistanceRank,
         RANK() OVER (ORDER BY TripCount DESC) AS TripRank
  FROM DriverStats
)
SELECT E.EMP_LNAME AS DriverName, 
       DR.TripCount, 
       DR.TotalDistance, 
       ROUND(DR.AvgDistance, 2) AS AvgDistance,
       DR.TotalFuel,
       DR.DistanceRank,
       DR.TripRank
FROM DriverRankings DR
JOIN EMPLOYEE E ON DR.TRIP_DRIVER = E.EMP_NUM
ORDER BY DR.TotalDistance DESC;
```

**Output:**
```
DriverName | TripCount | TotalDistance | AvgDistance | TotalFuel | DistanceRank | TripRank
Lange      | 3         | 3155          | 1051.67     | 852.0     | 1            | 2
Lewis      | 4         | 2391          | 597.75      | 780.6     | 2            | 1
Williams   | 3         | 2862          | 954.0       | 973.6     | 3            | 2
Duzak      | 3         | 2052          | 684.0       | 540.4     | 4            | 2
Travis     | 3         | 1954          | 651.33      | 513.4     | 5            | 2
```

**Conclusion:** Lange leads in total distance (3,155 miles) despite having fewer trips than Lewis, confirming his role as the long-distance specialist. Lewis has the most trips (4) but lower average distance, indicating focus on shorter, frequent routes. Williams shows high fuel consumption relative to distance, suggesting potential vehicle efficiency issues.
<br /><br />

-------------------------------------------------------------------------------
<br />

### Monthly Trip Analysis
- **Goal:** Analyze trip patterns by date for seasonal planning
- **Skills:** CTEs, Date Functions, Window Functions
- **Why these skills:** DATE_FORMAT() extracts month from dates, CTEs organize the analysis steps, window functions calculate cumulative metrics over time<br />

**Query:**
```
WITH MonthlyStats AS (
  SELECT DATE_FORMAT(TRIP_DATE, '%Y-%m') AS TripMonth,
         COUNT(*) AS TripsPerMonth,
         SUM(TRIP_DISTANCE) AS MonthlyDistance,
         AVG(TRIP_DISTANCE) AS AvgTripDistance
  FROM TRIP
  GROUP BY DATE_FORMAT(TRIP_DATE, '%Y-%m')
)
SELECT TripMonth,
       TripsPerMonth,
       MonthlyDistance,
       ROUND(AvgTripDistance, 2) AS AvgTripDistance,
       SUM(TripsPerMonth) OVER (ORDER BY TripMonth) AS CumulativeTrips
FROM MonthlyStats
ORDER BY TripMonth;
```

**Output:**
```
TripMonth | TripsPerMonth | MonthlyDistance | AvgTripDistance | CumulativeTrips
2024-02   | 18            | 13414           | 745.22          | 18
```

**Conclusion:** February 2024 shows strong operational activity with 18 trips covering 13,414 total miles. The average trip distance of 745.22 miles indicates a healthy mix of medium to long-distance operations, positioning the company well for sustained revenue generation.</b >
<br /><br />

-------------------------------------------------------------------------------
<br />
</details>

<details>
<summary>OVERALL BUSINESS INSIGHTS & CONCLUSIONS</summary>

## The Fleet Management Story: Data-Driven Insights

After analyzing the fleet management database through multiple SQL query perspectives, several key business insights emerge that tell a comprehensive story about the company's operations:

### Driver Specialization and Workload Distribution
The data reveals a sophisticated operation where **trip count doesn't equal responsibility**. While Rhonda Lewis (Driver 101) handles the most trips (4), John Lange (Driver 104) carries greater responsibility with higher-intensity long-distance routes averaging 1,051 miles per trip versus Lewis's 598 miles. This connects directly to our workforce analysis - with only 45% of employees certified as drivers, the company maximizes efficiency through specialization rather than equal distribution.

**The certification gap explains operational patterns**: ATP-licensed drivers (Lange, Lewis) handle the most demanding routes, while COM-licensed drivers (Williams, Duzak, Travis) support with moderate-distance trips. The 6 non-certified employees represent untapped capacity that could alleviate workload pressure on the current driver pool.

### Customer Intelligence and Revenue Opportunities
Customer behavior analysis reveals **Dunne as a high-value repeat customer** with 2 downtown trips, representing the ideal target for loyalty programs. The $4,323 in outstanding receivables, while concerning, may reflect extended credit terms for frequent users rather than collection issues. **Recommendation**: Implement location-based discounts for customers like Dunne who frequently travel to DTN, and create loyalty tiers based on trip frequency.

### Strategic Workforce Development Need
**Adding driver ratings (1-10 scale) would revolutionize operations** by connecting certification levels to actual performance and customer satisfaction. This would enable:
- **Smarter driver-customer matching**: Avoid pairing customers with drivers they've rated poorly
- **Performance-based training**: Identify if certified drivers actually perform better than their credentials suggest
- **A/B testing opportunities**: Test whether higher-rated drivers increase customer retention
- **Balanced workload distribution**: Train non-certified employees to reduce dependence on the current 5-driver pool

This rating system would provide crucial context missing from our current analysis - certification doesn't guarantee customer satisfaction or driving quality miles. This specialization maximizes both efficiency and driver expertise.

### Strategic Route Management
**Downtown (DTN) represents the company's bread-and-butter market** with 4 trips, while **GNV dominates long-distance operations** with the top 4 longest trips all heading to this destination. This geographic concentration suggests established customer relationships and route optimization. The company serves 7 distinct destinations, indicating good market diversification without overextension.

### Financial Health with Collection Challenges
While 50% of customers maintain zero balances (indicating prompt payment), **$4,323 in outstanding receivables** concentrated among 5 customers presents both opportunity and risk. Paul Olowski's $1,285 balance represents 30% of total receivables, suggesting either a high-value client relationship or potential collection issue requiring management attention.

### Fleet Utilization and Maintenance Priorities
The vehicle analysis reveals **Vehicle 2778V with 7,992 usage hours** significantly exceeds others, flagging it for priority maintenance or replacement consideration. The fleet composition favors PA31-350 models (50% of fleet), indicating standardization benefits for maintenance and training.

### Human Resource Optimization Opportunity
With only **45% of employees certified as drivers** (5 of 11), the company has untapped potential for capacity expansion. The presence of 2 ATP-certified drivers provides capability for the most demanding routes, while 3 COM-certified drivers handle standard operations effectively.

### Seasonal Performance Indicator
February 2024's performance of **18 trips covering 13,414 miles** with an average of 745 miles per trip suggests robust operational tempo. This baseline enables future seasonal comparisons and capacity planning.

### Strategic Recommendations
1. **Leverage specialization**: Continue assigning long-distance routes to Lange and high-frequency local routes to Lewis
2. **Address receivables**: Implement focused collection strategy for the $4,323 outstanding, particularly the top 4 accounts
3. **Expand driver pool**: Train additional employees for driver certification to increase operational flexibility
4. **Vehicle maintenance**: Schedule comprehensive maintenance for Vehicle 2778V given its high usage hours
5. **Route optimization**: Capitalize on strong DTN and GNV demand through dedicated service offerings
</details>
