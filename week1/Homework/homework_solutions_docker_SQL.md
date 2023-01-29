# Week 1: SQL and Docker Homework
In this homework we'll prepare the environment and practice with Docker and SQL

### Question 1. Knowing docker tags
After running the following command:
```
docker build --help
```

Answer: **-iidfile string** has the text *Write the image ID to the file*

## Question 2. Understanding docker first run
Run docker with the python:3.9 image in an interactive mode and the entrypoint of bash. Now check the python modules that are installed ( use pip list). How many python packages/modules are installed?

After running the following commands:
```
docker run -it --entrypoint=bash python:3.9
root@18e1901ae6fd:/# pip list
Package    Version
---------- -------
pip        22.0.4
setuptools 58.1.0
wheel      0.38.4
```

Answer: **3** modules installed

## Prepare Postgres
To starting docker compose with postgres and pgadmin 
```
docker-compose up 
```

Then ingesting data on postgres. I modified the ingestion script inside the docker image(taxi_ingest:v001) to accept ".csv.gz" files.
```
docker run -it --network=1st_week_default taxi_ingest:v001 --user=root --password=root --host=pgdatabase --port=5432 --db_name=ny_taxi --table_name=yellow_taxi_trips --url=https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-01.csv.gz
docker run -it --network=1st_week_default taxi_ingest:v001 --user=root --password=root --host=pgdatabase --port=5432 --db_name=ny_taxi --table_name=zones  --url=https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv
```

## Question 3. Count records
How many taxi trips were totally made on January 15?
To find out this run:
```
SELECT 
	COUNT(*)
FROM 
	yellow_taxi_trips t
WHERE
	t.lpep_pickup_datetime::date = '2019-01-15' AND
	t.lpep_dropoff_datetime::date = '2019-01-15';
```

Answer: **20530**

## Question 4
Question 4. Largest trip for each day
Which was the day with the largest trip distance Use the pick up time for your calculations.
To find this run:
```
SELECT 
	lpep_pickup_datetime::date, 
	MAX(trip_distance) AS Larges_trip
FROM 
	yellow_taxi_trips t
GROUP BY
	lpep_pickup_datetime::date
ORDER BY
	Larges_trip DESC;
```
Answer: **2019-01-15**

## Question 5. The number of passengers
In 2019-01-01 how many trips had 2 and 3 passengers?
To find out thi run:
```
SELECT 
	t.passenger_count,
	COUNT(*) AS Number_of_trips
FROM 
	yellow_taxi_trips t
WHERE
	t.lpep_pickup_datetime::date = '2019-01-01' AND
	(
		t.passenger_count = 2 OR 
	 	t.passenger_count = 3
	)
GROUP BY
	t.passenger_count;
```
Answer: **2: 1282 ; 3: 254**

## Question 6. Largest tip
For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip? We want the name of the zone, not the id.
To find out this run:
```
SELECT
	MAX(tip_amount),
	zpu."Zone" AS "pickup_loc",
    	zdo."Zone" AS "dropoff_loc"
FROM
	yellow_taxi_trips t
	JOIN zones zpu ON t."PULocationID" = zpu."LocationID"
	JOIN zones zdo ON t."DOLocationID" = zdo."LocationID"
WHERE
    zpu."Zone" = 'Astoria'
GROUP BY 2, 3
ORDER BY 1 DESC LIMIT 10;
```
Answer: **Long Island City/Queens Plaza**
