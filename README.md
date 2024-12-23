# Zuber

Task 1:

Find the number of taxi rides for each taxi company for November 15-16, 2017, name the resulting field trips_amount and print it, too. Sort the results by the trips_amount field in descending order.

SQL:

SELECT
    cabs.company_name as company_name,
    COUNT(trips.trip_id) AS trips_amount
FROM
    trips INNER JOIN cabs on cabs.cab_id = trips.cab_id
WHERE
CAST(start_ts as date) BETWEEN '2017-11-15' AND '2017-11-16'
GROUP BY
    cabs.company_name
ORDER BY
    trips_amount DESC

Results:


Task 2:

Find the number of rides for every taxi company whose name contains the words "Yellow" or "Blue" for November 1-7, 2017. Group the results by the company name.

SQL:
SELECT
    cabs.company_name as company_name,
    count(trip_id) as trips_amount
FROM
    trips INNER JOIN cabs ON trips.cab_id = cabs.cab_id
WHERE
    CAST(start_ts as DATE) BETWEEN '2017-11-01' AND '2017-11-07'
    AND cabs.company_name LIKE '%Blue%' 
GROUP BY
    cabs.company_name
UNION
SELECT
    cabs.company_name as company_name,
    count(trip_id) as trips_amount
FROM
    trips
INNER JOIN cabs ON trips.cab_id = cabs.cab_id
WHERE
    CAST(start_ts as DATE) BETWEEN '2017-11-01' AND '2017-11-07'
    and cabs.company_name LIKE '%Yellow%'
GROUP BY
    cabs.company_name

Results:




Task 3:
For November 1-7, 2017, the most popular taxi companies were Flash Cab and Taxi Affiliation Services. Find the number of rides for these two companies and name the resulting variable trips_amount. Join the rides for all other companies in the group "Other." Group the data by taxi company names. Name the field with taxi company names company. Sort the result in descending order by trips_amount.

SQL:
SELECT
    CASE 
    	WHEN cabs.company_name = 'Flash Cab' THEN 'Flash Cab'
WHEN cabs.company_name = 'Taxi Affiliation Services' THEN 'Taxi Affiliation Services'
  	  ELSE 'Other'
    END AS company,
    COUNT (trips.trip_id) AS trips_amount
FROM
    trips
INNER JOIN cabs ON trips.cab_id = cabs.cab_id
where 
    start_ts::date BETWEEN '2017-11-01' AND '2017-11-07'
group by
    company
ORDER by
  trips_amount DESC

Task 4:
Retrieve the identifiers of the O'Hare and Loop neighborhoods  from the neighborhoods table.

SQL:
SELECT
    neighborhood_id,
    name
FROM
    neighborhoods
WHERE name LIKE '%O''Hare%' OR name = 'Loop'

Task 5:
For each hour, retrieve the weather condition records from the weather_records table. Break all hours into two groups: Bad if the description field contains the words rain or storm, and Good for others. The final table must include two fields: date and hour (ts) and weather_conditions.

SQL:
SELECT
CASE
    WHEN weather_records.description LIKE '%rain%' OR weather_records.description LIKE '%storm%' THEN 'Bad'
    ELSE 'Good'
END as weather_conditions,
    ts
FROM
    weather_records

Task 6:
Retrieve from the trips table all the rides that started in the Loop (pickup_location_id: 50) on a Saturday and ended at O'Hare (dropoff_location_id: 63). Get the weather conditions for each ride. Also, retrieve the duration of each ride. Ignore rides for which data on weather conditions is not available.

The table columns should be in the following order:

start_ts
weather_conditions
duration_seconds
Sort by trip_id.

SQL:
SELECT
    trips.start_ts,
    CASE
    WHEN weather_records.description LIKE '%rain%' OR weather_records.description LIKE '%storm%' THEN 'Bad'
    ELSE 'Good'
END as weather_conditions,
    trips.duration_seconds
FROM trips
    INNER JOIN weather_records ON trips.start_ts = weather_records.ts
WHERE 
    pickup_location_id = 50
    and dropoff_location_id = 63
    and EXTRACT(DOW FROM trips.start_ts) = 6
ORDER by
    trips.trip_id;

  

