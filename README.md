# Chicago Taxi - Database Fundamentals

# Objetive

To analyze taxi trip records, vehicle availability, weather data using PostgreSQL queries, and to check server logs and organize error files using Linux command-line tools.

# Defining the problem & Business Impact

The Chicago taxi platform needed a clear view of trip volume, vehicle availability, and weather patterns to spot service shortages and understand why ride demand changes during bad weather. Without this visibility, users can face long wait times or fewer available rides during peak hours or storms.

At the same time, the engineering team needed to review raw server logs to find requests from specific IP addresses and separate HTTP 400 and 500 errors. Isolating these errors makes it faster to investigate incidents and fix backend issues that affect users.

# What I Did

- Connected to a remote server via Linux terminal, filtered raw server logs by IP address using grep, created structured folders (~/bug1/events/), copied date-specific server logs into main.txt and extract 400 and 500 error records into separate .txt files.

- Queried the chicago_taxi database in PostgreSQL using SELECT, WHERE, JOIN, GROUP BY, and HAVING to count available cabs, identify companies with fewer than 100 cabs and check trip counts across specific dates.
  
- Used conditional CASE logic on weather_records to classify weather conditions by date and hour into "Good" or "Bad" (identifying rain or storm keywords) during targeted periods.

# Technologies Used

SQL · PostgreSQL · psql · Linux CLI (File & Directory Management) · grep · SELECT · JOIN · GROUP BY · HAVING · CASE · Server Log Analysis

# Results & Business Implications

- **Server error isolation** (400.txt and 500.txt): Created organized folders and separated HTTP 400 client errors and 500 server errors from main.txt.
    -  Proven result: error logs extracted into separate files
    -  Possible implication: helps developers find recurring errors faster and investigate incidents before they affect more users

- **Small-fleet companies identified**: Found taxi companies operating with fewer than 100 cabs, such as Nova Taxi Affiliation LLC (97 cabs) and Patriot Taxi (89 cabs).
    - Proven result: fleet counts below 100 vehicles
    - Possible implication: smaller fleets may have limited service availability during busy periods or in high-demand areas.

- **Weather classification** : Classified weather conditions across specific date ranges and timeframes into structured categories.
  - Proven result: weather records grouped by rain/storm conditions as Good or Bad
  - Possible implication: allows the team to compare bad weather hours with spikes in ride demand or service delays.
    

# Learnings

- Practiced using Linux command-line tools (grep, directories, and file management) to filter and organize server logs.
  
- Strengthened SQL skills in PostgreSQL for data aggregation (GROUP BY, HAVING), conditional classification (CASE), and relational joins across trip and weather tables.


# Strategic Recommendations & Product Decision Support

- **Review error logs to prioritize bug fixes**: Use the separated 400 and 500 error files to check which endpoints fail most often and from which IPs.
    - Decision support: helps product and engineering leads decide which backend issues to fix first during incident reviews.

- **Monitor fleet supply in high-demand windows**: Track trip demand alongside smaller-fleet operators (under 100 cabs) to spot times when available vehicles may run low.
    - Decision support: helps operations teams evaluate service availability and plan partner coverage.

- **Use weather labels for demand planning**: Combine "Bad" weather flags (rain/storms) with historical trip data to anticipate busy hours.
    - Decision support: helps the team evaluate whether extra driver incentives or user wait-time alerts are needed during bad weather.


# Evidence
**IP Search & Server Log Filtering (Linux CLI)**
- grep -R "^233.201" ~/logs/2019/12

<img width="367" height="106" alt="imagen (6)" src="https://github.com/user-attachments/assets/e9bbb8dd-1be2-46fa-b85e-73a5273241cf" />

This evidence validates: Demonstrates log auditing using Linux regex search tools to locate all request logs associated with target IP addresses (233.201.*)

<br>

**Directory Creation & HTTP Error File Redirection**

- grep "400" ~/bug1/main.txt> ~/bug1/events/400.txt
- grep "500" ~/bug1/main.txt> ~/bug1/events/500.txt

This evidence Confirms the creation of dedicated event directories and the extraction of HTTP 400 and 500 status code log entries into separate files for backend error analysis

<br>

**Taxi Companies with Small Fleets (< 100 Vehicles)**

```sql
chicago_taxi=# SELECT company_name, COUNT(*) AS cnt
FROM cabs GROUP BY company_name
HAVING COUNT (*) < 100 ORDER BY cnt DESC;
```

<img width="225" height="255" alt="imagen (7)" src="https://github.com/user-attachments/assets/acc0c6ee-68d9-475b-ae3c-11e6e7ad6d6a" />

This validates aggregate SQL queries using GROUP BY and HAVING filters to identify companies with fleet sizes below 100 vehicles.

<br>

**Weather Condition Classification by Timestamp** 

- (May 11 - Jun 11, 2017), selection from weather_records categorizing conditions into "Bad" (rain/storm) or "Good" using a CASE statement

<img width="166" height="252" alt="imagen (8)" src="https://github.com/user-attachments/assets/6848e28c-b3fa-4d84-9184-8bfe17c7e067" />

```sql
SELECT ts,
CASE
WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
ELSE 'Good'
END AS weather_conditions
FROM weather_records
WHERE ts BETWEEN '2017-11-05 00:00:00' AND '2017-11-06 00:00:00';
```

This evidence demonstrates conditional evaluation (CASE) in PostgreSQL to classify weather conditions by timestamp (date and hour) into "Good" or "Bad


<br>

[Documentation of the exercises performed](https://docs.google.com/document/d/1q3A2y9f2qTaJNBTPAB-P6-c4FtFxh2D1/edit?pli=1)

