# Week 1: SQL and Docker Homework
In this homework we'll prepare the environment and practice with Docker and SQL

### Question 1. Knowing docker tags
After running the following command:
```
docker build --help
```

Answer:
We find that the tag **iidfile string** has the text *Write the image ID to the file*

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

Answer: 
We find that there are only **3 modules** installed

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

