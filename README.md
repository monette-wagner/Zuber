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

