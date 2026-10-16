# Chicago-Taxi---Database-Fundamentals
# Objetive

To analyze taxi service data and server logs using SQL and Linux command-line tools to investigate potential issues related to vehicle availability, trip records, and HTTP errors.

# Problem Definition

The taxi service team needed to investigate reports of insufficient vehicle availability and inconsistencies in trip data. Server logs also needed to be analyzed to identify requests from specific IP addresses and separate HTTP 400 and 500 errors for further investigation.

# What I Did

- Connected to a remote server and used Linux command-line tools to search server logs by IP address and filter requests.
- Extracted logs from a specified time period and separated HTTP 400 and 500 errors into individual files.
- Queried the taxi database to count available vehicles and identify companies with fewer than 100 vehicles.
- Used SQL queries to analyze trip records, compare trip volumes across taxi companies, and investigate fare-related data.
- Applied SQL filtering, grouping, conditional logic, and table relationships to retrieve relevant data for investigation.

# Technologies Used

SQL · PostgreSQL · psql · Linux · SELECT · JOIN · GROUP BY · HAVING · CASE · Server Log Analysis

# Results

Retrieved and organized taxi service data to investigate vehicle availability, trip volumes, and fare-related information. Filtered server logs by IP address and HTTP status codes, separating error records into files for further analysis.

# Learnings

- Practiced using Linux command-line tools to search, filter, and organize server logs.
- Learned to query relational databases using SQL.
- Strengthened my understanding of JOIN, GROUP BY, HAVING, and CASE.
- Applied data analysis techniques to investigate potential software and service issues.

# Evidence

**Command used to retrieve the required logs and the results obtained**.
grep -R "^233.201" ~/logs/2019/12

imagen

Commands used to extract the logs from `main.txt` into `400.txt` and `500.txt`.

- grep "400" ~/bug1/main.txt> ~/bug1/events/400.txt
- grep "500" ~/bug1/main.txt> ~/bug1/events/500.txt

**Excerpt of the list of companies with fewer than 100 vehicles and the SQL query used.**

chicago_taxi=# SELECT company_name, COUNT(*) AS cnt
FROM cabs GROUP BY company_name
HAVING COUNT (*) < 100 ORDER BY cnt DESC;

imagen

**Result table for Weather Conditions Classification** 

(May 11 - Jun 11, 2017), selection from weather_records categorizing conditions into "Bad" (rain/storm) or "Good" using a CASE statement

imagen

SQL Query

SELECT ts,
CASE
WHEN description LIKE '%rain%' OR description LIKE '%storm%' THEN 'Bad'
ELSE 'Good'
END AS weather_conditions
FROM weather_records
WHERE ts BETWEEN '2017-11-05 00:00:00' AND '2017-11-06 00:00:00';

Documentation of the exercises performed
https://docs.google.com/document/d/1q3A2y9f2qTaJNBTPAB-P6-c4FtFxh2D1/edit?pli=1

