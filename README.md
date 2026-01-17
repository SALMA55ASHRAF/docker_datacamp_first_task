# docker_datacamp_first_task
# Module 1 Homework: Docker & SQL Solutions

This repository contains my solutions for the Data Engineering Zoomcamp Module 1 homework focusing on Docker and SQL.

## Question 1: Understanding Docker Images

**Task:** Run docker with the `python:3.13` image using an entrypoint `bash` to interact with the container and find the pip version.

**Command used:**
```bash
docker run -it --entrypoint bash python:3.13
pip --version
```

**Answer:** 25.3

---

## Question 2: Understanding Docker Networking and docker-compose

**Task:** Given a docker-compose.yaml file, determine the hostname and port that pgadmin should use to connect to the postgres database.

**Answer:** postgres:5432

**Explanation:** Within a Docker network, containers communicate using the container name as the hostname. The postgres service is named "postgres" in the container_name field, and it exposes port 5432 internally (the second number in the ports mapping `5433:5432`). PgAdmin, being inside the same Docker network, connects to `postgres:5432`.

---

## Question 3: Counting Short Trips

**Task:** For trips in November 2025 (lpep_pickup_datetime between '2025-11-01' and '2025-12-01', exclusive), count trips with trip_distance ≤ 1 mile.

**SQL Query:**
```sql
SELECT COUNT(*) 
FROM green_trips 
WHERE lpep_pickup_datetime >= '2025-11-01' 
  AND lpep_pickup_datetime < '2025-12-01' 
  AND trip_distance <= 1;
```

**Answer:** 8,007

---

## Question 4: Longest Trip for Each Day

**Task:** Find the pickup day with the longest trip distance (only considering trips with trip_distance < 100 miles).

**SQL Query:**
```sql
SELECT lpep_pickup_datetime  
FROM green_trips 
WHERE trip_distance <= 100 
ORDER BY trip_distance DESC 
LIMIT 1;
```

**Answer:** 2025-11-14

**Details:** The longest trip was 88.03 miles on 2025-11-14 at 15:36:27

---

## Question 5: Biggest Pickup Zone

**Task:** Find the pickup zone with the largest total_amount (sum of all trips) on November 18th, 2025.

**SQL Query:**
```sql
SELECT t.zone, SUM(gt.total_amount) AS total_amount
FROM zones AS t 
INNER JOIN green_trips AS gt ON gt.PULocationID = t.LocationID 
WHERE DATE(gt.lpep_pickup_datetime) = '2025-11-18' 
GROUP BY t.zone 
ORDER BY total_amount DESC 
LIMIT 1;
```

**Answer:** East Harlem North

**Note:** The query showing trip counts was used initially. The correct query should sum total_amount instead of counting trips.

---

## Question 6: Largest Tip

**Task:** For passengers picked up in "East Harlem North" in November 2025, find the drop-off zone with the largest single tip.

**SQL Query:**
```sql
SELECT dz.Zone AS dropoff_zone, gt.tip_amount 
FROM green_trips gt 
JOIN zones pz ON gt.PULocationID = pz.LocationID 
JOIN zones dz ON gt.DOLocationID = dz.LocationID 
WHERE pz.Zone = 'East Harlem North' 
  AND gt.lpep_pickup_datetime >= '2025-11-01' 
  AND gt.lpep_pickup_datetime < '2025-12-01' 
ORDER BY gt.tip_amount DESC 
LIMIT 1;
```

**Answer:** Yorkville West

**Details:** The largest tip was $81.89 to Yorkville West

---

## Question 7: Terraform Workflow

**Task:** Identify the correct sequence for:
1. Downloading provider plugins and setting up backend
2. Generating proposed changes and auto-executing the plan
3. Removing all resources managed by terraform

**Answer:** terraform init, terraform apply -auto-approve, terraform destroy

**Explanation:**
- `terraform init`: Initializes the working directory and downloads provider plugins
- `terraform apply -auto-approve`: Creates/updates resources without manual confirmation
- `terraform destroy`: Removes all resources managed by Terraform

---



