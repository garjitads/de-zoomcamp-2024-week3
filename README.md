## Data Warehouse

[Source:] (https://github.com/ziritrion/dataeng-zoomcamp/blob/main/notes/3_data_warehouse.md)

### What is a Data Warehouse?

A Data Warehouse (DW) is an OLAP solution meant for reporting and data analysis. Unlike Data Lakes, which follow the ELT model, DWs commonly use the ETL model which was explained in lesson 2.

A DW receives data from different data sources which is then processed in a staging area before being ingested to the actual warehouse (a database) and arranged as needed. DWs may then feed data to separate Data Marts; smaller database systems which end users may use for different purposes.

![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/d0634f23-d25c-438e-982e-e1627ed5be0a)

### BigQuery

BigQuery (BQ) is a Data Warehouse solution offered by Google Cloud Platform.

- BQ is serverless. There are no servers to manage or database software to install; this is managed by Google and it's transparent to the customers.
- BQ is scalable and has high availability. Google takes care of the underlying software and infrastructure.
- BQ has built-in features like Machine Learning, Geospatial Analysis and Business Intelligence among others.
- BQ maximizes flexibility by separating data analysis and storage in different compute engines, thus allowing the customers to budget accordingly and reduce costs.

Some alternatives to BigQuery from other cloud providers would be AWS Redshift or Azure Synapse Analytics.

### wget yellow taxi dataset
```
Welcome to Cloud Shell! Type "help" to get started.
Your Cloud Platform project in this session is set to dtc-de-course-2024-411803.
Use “gcloud config set project [PROJECT_ID]” to change to a different project.
garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-01.parquet
--2024-02-09 09:15:28--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-01.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.231, 18.161.108.77, 18.161.108.184, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.231|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 110439634 (105M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-01.parquet’

yellow_tripdata_2019-01.parquet    100%[==============================================================>] 105.32M  16.6MB/s    in 7.4s    

2024-02-09 09:15:37 (14.2 MB/s) - ‘yellow_tripdata_2019-01.parquet’ saved [110439634/110439634]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-02.parquet 
--2024-02-09 09:15:46--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-02.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.231, 18.161.108.77, 18.161.108.184, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.231|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 103356025 (99M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-02.parquet’

yellow_tripdata_2019-02.parquet    100%[==============================================================>]  98.57M  16.3MB/s    in 7.1s    

2024-02-09 09:15:54 (13.9 MB/s) - ‘yellow_tripdata_2019-02.parquet’ saved [103356025/103356025]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-03.parquet
--2024-02-09 09:16:32--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-03.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.231, 18.161.108.77, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 116017372 (111M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-03.parquet’

yellow_tripdata_2019-03.parquet    100%[==============================================================>] 110.64M  16.4MB/s    in 7.9s    

2024-02-09 09:16:41 (14.1 MB/s) - ‘yellow_tripdata_2019-03.parquet’ saved [116017372/116017372]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-04.parquet
--2024-02-09 09:19:24--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-04.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.77, 18.161.108.231, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 110139137 (105M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-04.parquet’

yellow_tripdata_2019-04.parquet    100%[==============================================================>] 105.04M  7.92MB/s    in 9.8s    

2024-02-09 09:19:36 (10.7 MB/s) - ‘yellow_tripdata_2019-04.parquet’ saved [110139137/110139137]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-05.parquet
--2024-02-09 09:19:44--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-05.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.77, 18.161.108.231, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 111478943 (106M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-05.parquet’

yellow_tripdata_2019-05.parquet    100%[==============================================================>] 106.31M  16.2MB/s    in 7.7s    

2024-02-09 09:19:53 (13.8 MB/s) - ‘yellow_tripdata_2019-05.parquet’ saved [111478943/111478943]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-06.parquet
--2024-02-09 09:20:02--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-06.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.77, 18.161.108.184, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 102903344 (98M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-06.parquet’

yellow_tripdata_2019-06.parquet    100%[==============================================================>]  98.14M  16.2MB/s    in 7.1s    

2024-02-09 09:20:09 (13.9 MB/s) - ‘yellow_tripdata_2019-06.parquet’ saved [102903344/102903344]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-07.parquet
--2024-02-09 09:20:16--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-07.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.77, 18.161.108.184, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 93877343 (90M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-07.parquet’

yellow_tripdata_2019-07.parquet    100%[==============================================================>]  89.53M  16.2MB/s    in 6.6s    

2024-02-09 09:20:24 (13.5 MB/s) - ‘yellow_tripdata_2019-07.parquet’ saved [93877343/93877343]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-08.parquet
--2024-02-09 09:20:49--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-08.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.77, 18.161.108.231, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.77|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 89999675 (86M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-08.parquet’

yellow_tripdata_2019-08.parquet    100%[==============================================================>]  85.83M  12.9MB/s    in 7.2s    

2024-02-09 09:20:57 (11.9 MB/s) - ‘yellow_tripdata_2019-08.parquet’ saved [89999675/89999675]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-09.parquet
--2024-02-09 09:21:03--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-09.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.77, 18.161.108.231, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.77|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 97110325 (93M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-09.parquet’

yellow_tripdata_2019-09.parquet    100%[==============================================================>]  92.61M  16.3MB/s    in 6.8s    

2024-02-09 09:21:11 (13.7 MB/s) - ‘yellow_tripdata_2019-09.parquet’ saved [97110325/97110325]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-10.parquet
--2024-02-09 09:21:20--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-10.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.77, 18.161.108.231, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.77|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 106293373 (101M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-10.parquet’

yellow_tripdata_2019-10.parquet    100%[==============================================================>] 101.37M  16.5MB/s    in 7.2s    

2024-02-09 09:21:29 (14.0 MB/s) - ‘yellow_tripdata_2019-10.parquet’ saved [106293373/106293373]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-11.parquet
--2024-02-09 09:21:35--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-11.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.77, 18.161.108.231, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.77|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 100872983 (96M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-11.parquet’

yellow_tripdata_2019-11.parquet    100%[==============================================================>]  96.20M  14.7MB/s    in 7.3s    

2024-02-09 09:21:43 (13.1 MB/s) - ‘yellow_tripdata_2019-11.parquet’ saved [100872983/100872983]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-12.parquet
--2024-02-09 09:21:50--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2019-12.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.231, 18.161.108.141, 18.161.108.77, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.231|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 101044777 (96M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2019-12.parquet’

yellow_tripdata_2019-12.parquet    100%[==============================================================>]  96.36M  16.2MB/s    in 7.0s    

2024-02-09 09:21:58 (13.8 MB/s) - ‘yellow_tripdata_2019-12.parquet’ saved [101044777/101044777]
```
```
garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-01.parquet
--2024-02-09 09:27:04--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-01.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.231, 18.161.108.77, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 93562858 (89M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-01.parquet’

yellow_tripdata_2020-01.parquet    100%[==============================================================>]  89.23M  16.2MB/s    in 6.6s    

2024-02-09 09:27:11 (13.6 MB/s) - ‘yellow_tripdata_2020-01.parquet’ saved [93562858/93562858]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-02.parquet
--2024-02-09 09:27:18--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-02.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.231, 18.161.108.77, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 92134881 (88M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-02.parquet’

yellow_tripdata_2020-02.parquet    100%[==============================================================>]  87.87M  16.5MB/s    in 6.4s    

2024-02-09 09:27:25 (13.8 MB/s) - ‘yellow_tripdata_2020-02.parquet’ saved [92134881/92134881]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-03.parquet
--2024-02-09 09:27:30--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-03.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.141, 18.161.108.231, 18.161.108.77, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.141|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 44442590 (42M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-03.parquet’

yellow_tripdata_2020-03.parquet    100%[==============================================================>]  42.38M  13.3MB/s    in 3.7s    

2024-02-09 09:27:35 (11.6 MB/s) - ‘yellow_tripdata_2020-03.parquet’ saved [44442590/44442590]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-04.parquet
--2024-02-09 09:27:42--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-04.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4442620 (4.2M) [binary/octet-stream]
Saving to: ‘yellow_tripdata_2020-04.parquet’

yellow_tripdata_2020-04.parquet    100%[==============================================================>]   4.24M  3.21MB/s    in 1.3s    

2024-02-09 09:27:45 (3.21 MB/s) - ‘yellow_tripdata_2020-04.parquet’ saved [4442620/4442620]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-05.parquet
--2024-02-09 09:27:52--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-05.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6229864 (5.9M) [binary/octet-stream]
Saving to: ‘yellow_tripdata_2020-05.parquet’

yellow_tripdata_2020-05.parquet    100%[==============================================================>]   5.94M  4.12MB/s    in 1.4s    

2024-02-09 09:27:54 (4.12 MB/s) - ‘yellow_tripdata_2020-05.parquet’ saved [6229864/6229864]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-06.parquet
--2024-02-09 09:30:07--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-06.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 108.157.184.33, 108.157.184.223, 108.157.184.53, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|108.157.184.33|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9505358 (9.1M) [binary/octet-stream]
Saving to: ‘yellow_tripdata_2020-06.parquet’

yellow_tripdata_2020-06.parquet    100%[==============================================================>]   9.06M  6.16MB/s    in 1.5s    

2024-02-09 09:30:09 (6.16 MB/s) - ‘yellow_tripdata_2020-06.parquet’ saved [9505358/9505358]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-07.parquet
--2024-02-09 09:30:15--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-07.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 108.157.184.33, 108.157.184.223, 108.157.184.53, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|108.157.184.33|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13387778 (13M) [binary/octet-stream]
Saving to: ‘yellow_tripdata_2020-07.parquet’

yellow_tripdata_2020-07.parquet    100%[==============================================================>]  12.77M  7.33MB/s    in 1.7s    

2024-02-09 09:30:18 (7.33 MB/s) - ‘yellow_tripdata_2020-07.parquet’ saved [13387778/13387778]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-08.parquet
--2024-02-09 09:30:24--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-08.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 108.157.184.33, 108.157.184.223, 108.157.184.53, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|108.157.184.33|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16601463 (16M) [binary/octet-stream]
Saving to: ‘yellow_tripdata_2020-08.parquet’

yellow_tripdata_2020-08.parquet    100%[==============================================================>]  15.83M  8.61MB/s    in 1.8s    

2024-02-09 09:30:27 (8.61 MB/s) - ‘yellow_tripdata_2020-08.parquet’ saved [16601463/16601463]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-09.parquet
--2024-02-09 09:30:40--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-09.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 21381938 (20M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-09.parquet’

yellow_tripdata_2020-09.parquet    100%[==============================================================>]  20.39M  8.77MB/s    in 2.3s    

2024-02-09 09:30:43 (8.77 MB/s) - ‘yellow_tripdata_2020-09.parquet’ saved [21381938/21381938]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-10.parquet
--2024-02-09 09:30:50--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-10.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 26306876 (25M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-10.parquet’

yellow_tripdata_2020-10.parquet    100%[==============================================================>]  25.09M  9.69MB/s    in 2.6s    

2024-02-09 09:30:54 (9.69 MB/s) - ‘yellow_tripdata_2020-10.parquet’ saved [26306876/26306876]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-11.parquet
--2024-02-09 09:30:59--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-11.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 23583368 (22M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-11.parquet’

yellow_tripdata_2020-11.parquet    100%[==============================================================>]  22.49M  9.41MB/s    in 2.4s    

2024-02-09 09:31:03 (9.41 MB/s) - ‘yellow_tripdata_2020-11.parquet’ saved [23583368/23583368]

garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ wget https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-12.parquet
--2024-02-09 09:31:08--  https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2020-12.parquet
Resolving d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)... 18.161.108.184, 18.161.108.77, 18.161.108.141, ...
Connecting to d37ci6vzurychx.cloudfront.net (d37ci6vzurychx.cloudfront.net)|18.161.108.184|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 23020036 (22M) [application/x-www-form-urlencoded]
Saving to: ‘yellow_tripdata_2020-12.parquet’

yellow_tripdata_2020-12.parquet    100%[==============================================================>]  21.95M  9.16MB/s    in 2.4s    

2024-02-09 09:31:12 (9.16 MB/s) - ‘yellow_tripdata_2020-12.parquet’ saved [23020036/23020036]
```




### Hands-on learning

