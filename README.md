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

### Hands-on learning

#### wget datasets and load into a bucket

[Source]: (https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)

Files : yellow_tripdata_2019-*.parquet & yellow_tripdata_2020-*.parquet

**wget**

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
```
garjita_ds@cloudshell:~ (dtc-de-course-2024-411803)$ ls -al
total 1597312
drwxr-xr-x 10 garjita_ds garjita_ds      4096 Feb  9 09:31 .
drwxr-xr-x  4 root       root            4096 Jan 22 23:49 ..
-rw-------  1 garjita_ds garjita_ds     11337 Feb  9 09:31 .bash_history
-rw-r--r--  1 garjita_ds garjita_ds       220 Mar 27  2022 .bash_logout
-rw-r--r--  1 garjita_ds garjita_ds      3564 Jan  3 08:14 .bashrc
-rw-------  1 garjita_ds garjita_ds     21485 Feb  2 02:59 .boto
drwxr-xr-x  3 garjita_ds garjita_ds      4096 Jan 28 02:22 .cache
drwx------  4 garjita_ds garjita_ds      4096 Jan 28 02:21 .codeoss
drwxr-xr-x  3 garjita_ds garjita_ds      4096 Jan  3 08:05 .config
drwxr-xr-x  2 garjita_ds garjita_ds      4096 Feb  2 05:15 .docker
-rw-r--r--  1 garjita_ds garjita_ds   1254291 Jun 30  2022 green_tripdata_2022-01.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1428262 Jun 30  2022 green_tripdata_2022-02.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1615562 Jun 30  2022 green_tripdata_2022-03.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1570363 Aug 30  2022 green_tripdata_2022-04.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1589636 Aug 30  2022 green_tripdata_2022-05.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1529658 Aug 30  2022 green_tripdata_2022-06.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1312353 Nov 14  2022 green_tripdata_2022-07.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1346660 Nov 14  2022 green_tripdata_2022-08.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1445166 Nov 30  2022 green_tripdata_2022-09.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1444642 Dec 19  2022 green_tripdata_2022-10.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1270324 Jan 25  2023 green_tripdata_2022-11.parquet
-rw-r--r--  1 garjita_ds garjita_ds   1519259 Mar 20  2023 green_tripdata_2022-12.parquet
drwxr-xr-x 11 garjita_ds garjita_ds      4096 Feb  2 02:26 mage-ai-terraform-templates
drwxr-xr-x  2 garjita_ds garjita_ds      4096 Jan 28 02:44 mybucket
drwxr-xr-x  3 garjita_ds garjita_ds      4096 Jan 22 23:49 .npm
-rw-r--r--  1 garjita_ds garjita_ds       807 Mar 27  2022 .profile
-rw-r--r--  1 garjita_ds garjita_ds       913 Feb  9 09:05 README-cloudshell.txt
drwxr-xr-x  2 garjita_ds garjita_ds      4096 Feb  2 02:38 .terraform.d
-rw-------  1 garjita_ds garjita_ds     19815 Feb  2 10:18 .viminfo
-rw-r--r--  1 garjita_ds garjita_ds 110439634 Jun 30  2022 yellow_tripdata_2019-01.parquet
-rw-r--r--  1 garjita_ds garjita_ds 103356025 Jun 30  2022 yellow_tripdata_2019-02.parquet
-rw-r--r--  1 garjita_ds garjita_ds 116017372 Jun 30  2022 yellow_tripdata_2019-03.parquet
-rw-r--r--  1 garjita_ds garjita_ds 110139137 Jun 30  2022 yellow_tripdata_2019-04.parquet
-rw-r--r--  1 garjita_ds garjita_ds 111478943 Jun 30  2022 yellow_tripdata_2019-05.parquet
-rw-r--r--  1 garjita_ds garjita_ds 102903344 Jun 30  2022 yellow_tripdata_2019-06.parquet
-rw-r--r--  1 garjita_ds garjita_ds  93877343 Jun 30  2022 yellow_tripdata_2019-07.parquet
-rw-r--r--  1 garjita_ds garjita_ds  89999675 Jun 30  2022 yellow_tripdata_2019-08.parquet
-rw-r--r--  1 garjita_ds garjita_ds  97110325 Jun 30  2022 yellow_tripdata_2019-09.parquet
-rw-r--r--  1 garjita_ds garjita_ds 106293373 Jun 30  2022 yellow_tripdata_2019-10.parquet
-rw-r--r--  1 garjita_ds garjita_ds 100872983 Jun 30  2022 yellow_tripdata_2019-11.parquet
-rw-r--r--  1 garjita_ds garjita_ds 101044777 Jun 30  2022 yellow_tripdata_2019-12.parquet
-rw-r--r--  1 garjita_ds garjita_ds  93562858 Jun 30  2022 yellow_tripdata_2020-01.parquet
-rw-r--r--  1 garjita_ds garjita_ds  92134881 Jun 30  2022 yellow_tripdata_2020-02.parquet
-rw-r--r--  1 garjita_ds garjita_ds  44442590 Jun 30  2022 yellow_tripdata_2020-03.parquet
-rw-r--r--  1 garjita_ds garjita_ds   4442620 Jun 30  2022 yellow_tripdata_2020-04.parquet
-rw-r--r--  1 garjita_ds garjita_ds   6229864 Jun 30  2022 yellow_tripdata_2020-05.parquet
-rw-r--r--  1 garjita_ds garjita_ds   9505358 Jun 30  2022 yellow_tripdata_2020-06.parquet
-rw-r--r--  1 garjita_ds garjita_ds  13387778 Jun 30  2022 yellow_tripdata_2020-07.parquet
-rw-r--r--  1 garjita_ds garjita_ds  16601463 Jun 30  2022 yellow_tripdata_2020-08.parquet
-rw-r--r--  1 garjita_ds garjita_ds  21381938 Jun 30  2022 yellow_tripdata_2020-09.parquet
-rw-r--r--  1 garjita_ds garjita_ds  26306876 Jun 30  2022 yellow_tripdata_2020-10.parquet
-rw-r--r--  1 garjita_ds garjita_ds  23583368 Jun 30  2022 yellow_tripdata_2020-11.parquet
-rw-r--r--  1 garjita_ds garjita_ds  23020036 Jun 30  2022 yellow_tripdata_2020-12.parquet
```

**Copy files into a bucket**
```
gsutil cp yellow*.parquet gs://de-zoomcamp-garjita-bucket/trip_data

Copying file://yellow_tripdata_2019-01.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-02.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-03.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-04.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-05.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-06.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-07.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-08.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-09.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-10.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-11.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2019-12.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-01.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-02.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-03.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-04.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-05.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-06.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-07.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-08.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-09.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-10.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-11.parquet [Content-Type=application/octet-stream]...
Copying file://yellow_tripdata_2020-12.parquet [Content-Type=application/octet-stream]...

- [24 files][  1.5 GiB/  1.5 GiB]   39.3 MiB/s                                  
==> NOTE: You are performing a sequence of gsutil operations that may
run significantly faster if you instead use gsutil -m cp ... Please
see the -m section under "gsutil help options" for further information
about when gsutil -m can be advantageous.


Operation completed over 24 objects/1.5 GiB.      
```

## External tables
BigQuery supports a few external data sources: you may query these sources directly from BigQuery even though the data itself isn't stored in BQ.

An external table is a table that acts like a standard BQ table. The table metadata (such as the schema) is stored in BQ storage but the data itself is external.

You may create an external table from a CSV or Parquet file stored in a Cloud Storage bucket:
```
CREATE OR REPLACE EXTERNAL TABLE `dtc-de-course-2024-411803.nytaxi.external_yellow_tripdata`
OPTIONS (
  format = 'PARQUET',
  uris = ['gs://de-zoomcamp-garjita-bucket/trip_data/yellow_tripdata_2019-*.parquet', 'gs://de-zoomcamp-garjita-bucket/trip_data/yellow_tripdata_2020-*.parquet']
);
```
![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/88f8d62d-7b20-428b-9e1b-075f24601163)









