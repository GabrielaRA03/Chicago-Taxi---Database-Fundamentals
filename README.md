# Chicago-Taxi-Database-Fundamentals

# Objetive

To analyze taxi trip records, fleet availability, and historical weather conditions using PostgreSQL queries, while conducting server log auditing and directory management using Linux command-line tools.

# Problem Definition

TThe Chicago taxi platform required an investigation into trip data, pricing, and vehicle availability to identify operational bottlenecks and potential fleet shortages. Additionally, evaluating historical weather data (rain/storms) by date and time was necessary to assess external impacts on ride demand. 

On the server side, log auditing was required to inspect requests from specific IP addresses, establish structured event directories, and isolate HTTP client errors (400) and server crashes (500) for engineering root-cause analysis.

# What I Did

- Connected to remote servers via terminal to search raw server logs by IP address, created dedicated directories (~/bug1/events/), and extracted HTTP 400 and 500 error records into separate .txt files
- Queried the chicago_taxi database using SELECT, WHERE, JOIN, GROUP BY, and HAVING to count available vehicles, identify companies with fewer than 100 cabs, and analyze trip volumes and fares across specific date ranges.
- Used conditional CASE logic on weather_records to classify weather conditions by date and hour into "Good" or "Bad" (identifying rain or storm keywords) during targeted periods.

# Technologies Used

SQL · PostgreSQL · psql · Linux CLI (File & Directory Management) · grep · SELECT · JOIN · GROUP BY · HAVING · CASE · Server Log Analysis

# Results

- Created file directories and successfully isolated client errors (400) and server crashes (500) into 400.txt and 500.txt from main.txt, streamlining error tracking for developers.
- Identified taxi companies operating with fewer than 100 vehicles (e.g., Nova Taxi Affiliation LLC with 97 cabs, Patriot Taxi with 89), highlighting potential supply shortages in specific areas.
- Classified weather conditions across specific date ranges and timeframes, providing structured datasets to correlate bad weather (rain/storms) with spikes in trip demand.

# Learnings

- Developed practical command-line skills in Linux for text filtering (grep), directory creation, and file manipulation.
- GAin hands-on experience SQL techniques in PostgreSQL for data aggregation (GROUP BY, HAVING), conditional classification (CASE), and relational joins across trip and weather tables.

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

