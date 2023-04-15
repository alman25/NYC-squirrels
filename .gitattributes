Create Table sq (
	longitude DECIMAL(10,8),
	lat DECIMAL(10,8),
	unique_squirrel_id VARCHAR(14),
	hectare VARCHAR(3),
	shift VARCHAR(2),
	dates VARCHAR(8),
	hectare_squirrel_number NUMERIC,
	age VARCHAR(8),
	primary_fur_color VARCHAR(8),
	locations VARCHAR(12),
	running NUMERIC,
	chasing NUMERIC,
	climbing NUMERIC,
	eating NUMERIC,
	foraging NUMERIC,
	kuks NUMERIC,
	quaas NUMERIC,
	moans NUMERIC,
	tail_flags NUMERIC,
	tail_twitches NUMERIC,
	approaches NUMERIC,
	indifferent NUMERIC,
	runs_from NUMERIC
);

SELECT *
FROM sq;

# Identify duplicates in the table
SELECT unique_squirrel_id, COUNT(*)
FROM sq
GROUP BY 1
HAVING COUNT(*) > 1;

-- Drop Duplicate Rows
SET SQL_SAFE_UPDATES = 0;

DELETE FROM sq
WHERE unique_squirrel_id IN(
		SELECT unique_squirrel_id
        FROM(
			SELECT unique_squirrel_id,
            ROW_NUMBER() OVER (PARTITION BY hectare, shift, dates, hectare_squirrel_number, age, primary_fur_color, locations, running,
            chasing, climbing, eating, foraging, kuks, quaas, moans, tail_flags, tail_twitches, approaches, indifferent, runs_from)
            as rownum
            FROM sq) as sub
		WHERE rownum > 1);

# Information on Squirrel appearance and behavior. 
SELECT DISTINCT(primary_fur_color) as 'Fur Color'
FROM sq
WHERE 'Fur Color' IS NOT NULL and primary_fur_color <> '';

SELECT primary_fur_color as 'Fur Color', 
	   COUNT(primary_fur_color) as Count
FROM sq
WHERE primary_fur_color IS NOT NULL and primary_fur_color <> ''
GROUP BY primary_fur_color
ORDER by 2 DESC;

# Cinnamon Sq Approachability 
Select age as Age, 
	   primary_fur_color AS Color,
	   SUM(approaches) AS Approaches, 
	   SUM(indifferent) AS Indifferent,
	   SUM(runs_from) AS "Runs From"
From sq
WHERE AGE IS NOT NULL 
AND AGE <> ''
AND primary_fur_color='Cinnamon'
GROUP BY age, primary_fur_color
ORDER BY 1;

# Black Sq Approachability 
Select age as Age, 
	   primary_fur_color AS Color,
	   SUM(approaches) AS Approaches, 
	   SUM(indifferent) AS Indifferent,
	   SUM(runs_from) AS "Runs From"
From sq
WHERE AGE IS NOT NULL 
AND AGE <> ''
AND primary_fur_color='Black'
GROUP BY age, primary_fur_color
ORDER BY 1;

#Gray Sq Approachability 
Select age as Age, 
	primary_fur_color AS Color,
	count(approaches) AS Approaches, 
	count(indifferent) AS Indifferent,
	count(runs_from) AS "Runs From"
From sq
WHERE AGE IS NOT NULL 
AND AGE <> ''
AND AGE <> '?'
AND primary_fur_color='Gray'
GROUP BY age, Color
ORDER BY 1;

#Identify location by fur color
SELECT primary_fur_color as "Fur Color", 
	   locations as Locations, 
       Count(locations) as Totals
FROM sq
WHERE primary_fur_color IS NOT null AND primary_fur_color <> ''
AND locations IS NOT null and locations <> ''
GROUP BY 2, 1
ORDER BY primary_fur_color, locations;

# Count total by age and color
SELECT age as Age, 
	   primary_fur_color as "Fur Color", 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Juvenile' AND primary_fur_color = 'Black'
GROUP BY 1, 2
ORDER BY 1;

SELECT age as Age, 
	   primary_fur_color as 'Fur Color', 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Juvenile' AND primary_fur_color = 'Cinnamon'
GROUP BY 1, 2
ORDER BY 1;

SELECT age as Age, 
	   primary_fur_color as 'Fur Color', 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Juvenile' AND primary_fur_color = 'Gray'
GROUP BY 1, 2
ORDER BY 1;

#Above and Ground Level -- Ground Plane Above Ground
SELECT age as Age, 
	   primary_fur_color as 'Fur Color', 
       locations as Locations, 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Adult' AND primary_fur_color = 'Black'
GROUP BY 1, 2, 3
ORDER BY 1;

SELECT age as Age, 
	   primary_fur_color as 'Fur Color', 
       locations as Locations, 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Juvenile' AND primary_fur_color = 'Cinnamon'
GROUP BY 1, 2, 3
ORDER BY 1;

SELECT age as Age, 
	   primary_fur_color as 'Fur Color', 
       locations as Locations, 
       COUNT(unique_squirrel_id) As Totals
FROM sq
WHERE age = 'Adult' AND primary_fur_color = 'Gray' and locations <> ''
GROUP BY 1, 2, 3
ORDER BY 1;

# What was the date for the highest number of sightings?
SELECT dates as Dates,
	   sum(unique_squirrel_id) as "Total Sightings"
FROM sq
GROUP BY 1
ORDER BY 2 DESC;
#10/13/18

# Who runs more, adults or juveniles?
SELECT age as Age,
	   count(running) as Total
FROM sq
WHERE running = 'True'
AND AGE <> ''
AND AGE <> '?'
GROUP BY 1;
#adults

# Which squirrels are more willing to approach humans, and in which areas?
SELECT primary_fur_color as "Fur Color",
	   count(approaches) as 'Approach Humans'
FROM sq
WHERE approaches = 'True'
AND primary_fur_color <> ''
GROUP BY 1
ORDER BY 2 DESC;
#Gray

# Do squirrels who approach humans eat more frequently?
SELECT approaches,
	   count(eating) as Eating
FROM sq
GROUP by 1;
#No

# features:

#   # fur color vs behavior >>> heatmap
#   # age vs behavior >>> heatmap

# time:
#   # at what time where the squirrels more frequently sighted?
SELECT DISTINCT shift,
	   count(*) as Count
FROM sq
GROUP by 1
Order by 2;

# location:
#   # where were the squirrels most frequently sighted?:
SELECT DISTINCT locations as Locations,
	   count(*) as Count
FROM sq
WHERE locations <> ''
GROUP by 1;

#     # park/city - am most frequent location/pm most frequent location
SELECT DISTINCT locations as Locations,
	   count(*) as Count,
       shift as Shift
FROM sq
WHERE locations <> ''
GROUP by 1, 3;

#     # what behaviors are associated with the different locations?:
#       # kuks + etc vs location
SELECT Distinct kuks as Kuks,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3;

SELECT Distinct chasing as Chasing,
	   count(*),
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct climbing as Climbing,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct eating as Eating,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct foraging as Foraging,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct quaas as Quaas,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct moans,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct tail_flags as TailFlags,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct tail_twitches as TailTwitches,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct approaches as Approaches,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct indifferent as Indifferent,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT Distinct runs_from as RunsFrom,
	   count(*) as Count,
       locations as Locations
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

#       # approach/avoid humans vs location
SELECT approaches as Approaches,
	   count(*) as Count,
       locations as Location
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;

SELECT runs_from as RunsFrom,
	   count(*) as Count,
       locations as Location
FROM sq
WHERE locations <> ''
GROUP by 1, 3
ORDER by 3;


# behavior:
#   which behaviors were recorded? which ones were more frequent? when? (am/pm - month)
SELECT 
	   count(*) as Running,
       shift as Shift
FROM sq
WHERE running  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Chasing,
       shift as Shift
FROM sq
WHERE chasing  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Climbing,
       shift as Shift
FROM sq
WHERE climbing  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Eating,
       shift as Shift
FROM sq
WHERE eating  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Foraging,
       shift as Shift
FROM sq
WHERE foraging  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Kuks,
       shift as Shift
FROM sq
WHERE kuks  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Quaas,
       shift as Shift
FROM sq
WHERE quaas  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Moans,
       shift as Shift
FROM sq
WHERE moans = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as TailFlags,
       shift as Shift
FROM sq
WHERE tail_flags = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as TailTwitches,
       shift as Shift
FROM sq
WHERE tail_twitches  = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Approaches,
       shift as Shift
FROM sq
WHERE approaches = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as Indifferent,
       shift as Shift
FROM sq
WHERE indifferent = 1
GROUP by 2
ORDER by 2;

SELECT 
	   count(*) as RunsFrom,
       shift as Shift
FROM sq
WHERE runs_from = 1
GROUP by 2
ORDER by 2;

#   where do the squirrels eat? can I find that in this dataset?
SELECT locations as Locations,
	   count(eating) as Eat
FROM sq
WHERE eating = 1
AND locations <> ''
GROUP by 1;
