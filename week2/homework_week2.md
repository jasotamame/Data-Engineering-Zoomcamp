# Week 2 Homework
The goal of this homework is to familiarise users with workflow orchestration and observation.

### Question 1. Load January 2020 data
Using the etl_web_to_gcs.py flow that loads taxi data into GCS as a guide, create a flow that loads the green taxi CSV dataset for January 2020 into GCS and run it. Look at the logs to find out how many rows the dataset has.

How many rows does that dataset have?

- 447,770
- 766,792
- 299,234
- 822,132


The answer is **447,770**

```
Created task run 'fetch-b4598a4a-0' for task 'fetch'
08:08:39 PM
Executing 'fetch-b4598a4a-0' immediately...
08:08:39 PM
Finished in state Completed()
08:08:41 PM
fetch-b4598a4a-0
Created task run 'clean-b9fd7e03-0' for task 'clean'
08:08:41 PM
Executing 'clean-b9fd7e03-0' immediately...
08:08:41 PM
columns: VendorID                 float64
lpep_pickup_datetime      object
lpep_dropoff_datetime     object
store_and_fwd_flag        object
RatecodeID               float64
PULocationID               int64
DOLocationID               int64
passenger_count          float64
trip_distance            float64
fare_amount              float64
extra                    float64
mta_tax                  float64
tip_amount               float64
tolls_amount             float64
ehail_fee                float64
improvement_surcharge    float64
total_amount             float64
payment_type             float64
trip_type                float64
congestion_surcharge     float64
dtype: object
08:08:41 PM
clean-b9fd7e03-0
rows: 447770
```

