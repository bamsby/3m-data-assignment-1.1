# Video Game Sales Analysis

## Dataset

You will be working with the following dataset: [Video Game Sales](https://www.kaggle.com/datasets/gregorut/videogamesales?resource=download)

📦 **Dataset Download Instructions**
1. Download the dataset ZIP file from the above link.
2. After downloading: Unzip the file to access vgsales.csv. Note the full file path to vgsales.csv — you'll need it in the next step.

🔍 **Challenge: Load the Data into DuckDB**
Using DBeaver and your DuckDB connection, how would you load the vgsales.csv file into a table so you can begin querying it?

## Business Question
How can game developers and publishers optimize their strategy to maximize global sales by understanding the performance of different game genres, platforms, and publishers?

*To answer the above question, use the following SQL queries to explore the dataset and address the following questions:*

Which genres contribute the most to global sales?

SQL:
```sql
SELECT Genre, SUM(Global_Sales)
FROM Game
GROUP BY Genre
ORDER BY SUM(Global_Sales) DESC;
```
Findings:
```findings
Action	1751.1799999999691
Sports	1330.929999999988
Shooter	1037.3699999999901
Role-Playing	927.3699999999941
Platform	831.3699999999974
```
Which platforms generate the highest global sales?

SQL:
```sql
SELECT
  Platform,
  SUM(Global_Sales) AS total_global_sales
FROM main.Game
GROUP BY Platform
ORDER BY total_global_sales DESC;
```
Findings:
```findings
PS2	    1255.6399999999871
X360	979.9599999999996
PS3	    957.8399999999987
Wii	    926.7099999999971
DS	    822.4899999999874
PS	    730.659999999997
```
Which publishers are the most successful in terms of global sales?

SQL:
```sql
select sum(global_sales), publisher
from main.Game
group by publisher  
order by 1 desc;
```
Findings:
```findings
sum(global_sales)	Publisher
1786.5599999999981	Nintendo
1110.3199999999915	Electronic Arts
727.4599999999983	Activision
607.4999999999989	Sony Computer Entertainment
474.71999999999935	Ubisoft
```
How does success vary across regions (North America, Europe, Japan, Others)?

SQL:
```sql
SELECT
  Platform,
  SUM(NA_Sales) AS total_NA_sales,
  SUM(EU_Sales) AS total_EU_sales,
  SUM(JP_Sales) AS total_JP_sales,
  SUM(Other_Sales) AS total_Other_sales,
  SUM(Global_Sales) AS total_global_sales
FROM main.Game
GROUP BY Platform
ORDER BY total_global_sales DESC;
```
Findings:
```findings
PS2	583.8399999999925	339.2899999999957	139.20000000000064	193.44000000000062	1255.6399999999871
X360	601.0499999999992	280.5799999999964	12.429999999999923	85.54000000000137	979.9599999999996
PS3	392.2599999999998	343.70999999999805	79.99000000000005	141.92999999999992	957.8399999999987
Wii	507.7099999999991	268.3799999999979	69.35000000000001	80.6100000000016	926.7099999999971
DS	390.7099999999977	194.64999999999938	175.57000000000048	60.52999999999964	822.4899999999874
```
What are the trends over time in game sales by genre and platform?

SQL:
```sql
SELECT
  Year,
  Genre,
  SUM(Global_Sales) AS total_global_sales
FROM main.Game
WHERE Year IS NOT NULL
GROUP BY Year, Genre
ORDER BY Year, total_global_sales DESC;
```
Findings:
```findings
1980	Shooter	7.069999999999999
1980	Misc	2.71
1980	Fighting	0.77
1980	Sports	0.49
1980	Action	0.34
```
Which platforms are most successful for specific genres?

SQL:
```sql
WITH genre_platform AS (
  SELECT
    Genre,
    Platform,
    SUM(Global_Sales) AS total_global_sales,
    DENSE_RANK() OVER (
      PARTITION BY Genre
      ORDER BY SUM(Global_Sales) DESC
    ) AS rnk
  FROM main.Game
  GROUP BY Genre, Platform
)
SELECT Genre, Platform, total_global_sales, rnk
FROM genre_platform
WHERE rnk <= 3
ORDER BY Genre, rnk, total_global_sales DESC;
```
Findings:
```findings
Action	PS3	307.8799999999995	1
Action	PS2	272.7599999999997	2
Action	X360	242.67000000000024	3
Adventure	DS	47.290000000000035	1
Adventure	PS3	22.900000000000006	2
```
## Deliverables:
- SQL Queries: Provide all the SQL queries you used to answer the business questions.
- Summary of Findings: For each question, summarise your key findings and recommendations based on your analysis.

## Submission

- Submit the GitHub URL of your assignment to NTU black board.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.
