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
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-01.csv.gz
--2024-02-09 12:33:17--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-01.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/afb2f0a6-bb8b-4958-9818-834bda641e9e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123317Z&X-Amz-Expires=300&X-Amz-Signature=4f5dfc630c79541ecaaa36b83a9250acb557c94364877b04f5ba747dcf5cb20e&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-01.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:33:17--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/afb2f0a6-bb8b-4958-9818-834bda641e9e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123317Z&X-Amz-Expires=300&X-Amz-Signature=4f5dfc630c79541ecaaa36b83a9250acb557c94364877b04f5ba747dcf5cb20e&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-01.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 134445150 (128M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-01.csv.gz’

yellow_tripdata_2019-01.csv.gz     100%[==============================================================>] 128.22M   209MB/s    in 0.6s    

2024-02-09 12:33:19 (209 MB/s) - ‘yellow_tripdata_2019-01.csv.gz’ saved [134445150/134445150]
```

```
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-02.csv.gz 
--2024-02-09 12:34:18--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-02.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7e4c9388-9d67-4eed-949c-aa5cebd0c15d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123419Z&X-Amz-Expires=300&X-Amz-Signature=a7bc8dee612b71ee6b0ae39ebd1e60ab3461b4e7f9fc4e435766d6b8dde9da2f&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-02.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:34:19--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7e4c9388-9d67-4eed-949c-aa5cebd0c15d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123419Z&X-Amz-Expires=300&X-Amz-Signature=a7bc8dee612b71ee6b0ae39ebd1e60ab3461b4e7f9fc4e435766d6b8dde9da2f&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-02.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 128275506 (122M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-02.csv.gz’

yellow_tripdata_2019-02.csv.gz     100%[==============================================================>] 122.33M  16.1MB/s    in 7.7s    

2024-02-09 12:34:28 (15.9 MB/s) - ‘yellow_tripdata_2019-02.csv.gz’ saved [128275506/128275506]
```

```
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-03.csv.gz 
--2024-02-09 12:34:36--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-03.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/79070eb2-7bbb-4ea6-970e-8dbec572f6f4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123436Z&X-Amz-Expires=300&X-Amz-Signature=e8e51727c800bb965997edd58963d516c2291f129323f70dc6cc395f38421b37&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-03.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:34:36--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/79070eb2-7bbb-4ea6-970e-8dbec572f6f4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123436Z&X-Amz-Expires=300&X-Amz-Signature=e8e51727c800bb965997edd58963d516c2291f129323f70dc6cc395f38421b37&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-03.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 143692931 (137M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-03.csv.gz’

yellow_tripdata_2019-03.csv.gz     100%[==============================================================>] 137.04M  17.6MB/s    in 8.8s    

2024-02-09 12:34:46 (15.6 MB/s) - ‘yellow_tripdata_2019-03.csv.gz’ saved [143692931/143692931]
```
```
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-04.csv.gz 
--2024-02-09 12:34:57--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-04.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/2e003b08-ce42-487d-b666-12f43d408d18?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123457Z&X-Amz-Expires=300&X-Amz-Signature=e6c7c5eff86f80a51f7556d30efe9efa63923a3cd4bb9acc12e5f5fec18d170c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-04.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:34:57--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/2e003b08-ce42-487d-b666-12f43d408d18?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123457Z&X-Amz-Expires=300&X-Amz-Signature=e6c7c5eff86f80a51f7556d30efe9efa63923a3cd4bb9acc12e5f5fec18d170c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-04.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 136233383 (130M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-04.csv.gz’

yellow_tripdata_2019-04.csv.gz     100%[==============================================================>] 129.92M  16.5MB/s    in 7.9s    

2024-02-09 12:35:05 (16.5 MB/s) - ‘yellow_tripdata_2019-04.csv.gz’ saved [136233383/136233383]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-05.csv.gz 
--2024-02-09 12:35:11--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-05.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/29c6f6d1-f585-4ba5-a52b-6b4e0b359542?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123512Z&X-Amz-Expires=300&X-Amz-Signature=54a6fa5aaf15a81e83497a1bd134dc42c71aa6ab1784120075f845ca8fd52983&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-05.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:35:12--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/29c6f6d1-f585-4ba5-a52b-6b4e0b359542?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123512Z&X-Amz-Expires=300&X-Amz-Signature=54a6fa5aaf15a81e83497a1bd134dc42c71aa6ab1784120075f845ca8fd52983&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-05.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 138963071 (133M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-05.csv.gz’

yellow_tripdata_2019-05.csv.gz     100%[==============================================================>] 132.53M   286MB/s    in 0.5s    

2024-02-09 12:35:13 (286 MB/s) - ‘yellow_tripdata_2019-05.csv.gz’ saved [138963071/138963071]
```
```
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-06.csv.gz 
--2024-02-09 12:35:23--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-06.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/88c34148-b51d-46a9-bbb5-a5df806d7cfc?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123523Z&X-Amz-Expires=300&X-Amz-Signature=e5dd53743c5bd730809cc33643203fe47d4e17b14b0334dcf58e80aa1bbdd961&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-06.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:35:24--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/88c34148-b51d-46a9-bbb5-a5df806d7cfc?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123523Z&X-Amz-Expires=300&X-Amz-Signature=e5dd53743c5bd730809cc33643203fe47d4e17b14b0334dcf58e80aa1bbdd961&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-06.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 127685708 (122M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-06.csv.gz’

yellow_tripdata_2019-06.csv.gz     100%[==============================================================>] 121.77M   227MB/s    in 0.5s    

2024-02-09 12:35:25 (227 MB/s) - ‘yellow_tripdata_2019-06.csv.gz’ saved [127685708/127685708]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-07.csv.gz 
--2024-02-09 12:35:32--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-07.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/2a11064d-04da-40fa-8a4d-3fd93cfb07e1?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123532Z&X-Amz-Expires=300&X-Amz-Signature=e488d6e50419a279bd7648fb315aa774e164123d8a355a343060e61ca78ee0ee&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-07.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:35:32--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/2a11064d-04da-40fa-8a4d-3fd93cfb07e1?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123532Z&X-Amz-Expires=300&X-Amz-Signature=e488d6e50419a279bd7648fb315aa774e164123d8a355a343060e61ca78ee0ee&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-07.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 115817922 (110M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-07.csv.gz’

yellow_tripdata_2019-07.csv.gz     100%[==============================================================>] 110.45M  16.7MB/s    in 7.7s    

2024-02-09 12:35:41 (14.3 MB/s) - ‘yellow_tripdata_2019-07.csv.gz’ saved [115817922/115817922]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-08.csv.gz 
--2024-02-09 12:35:48--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-08.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/cabf77d2-e711-4bf4-a64e-aea7824bdc4e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123549Z&X-Amz-Expires=300&X-Amz-Signature=a892dbae68d9bb031fb0438aa737e98e05e16e5d49fde0b4aba50343b9db59cb&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-08.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:35:49--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/cabf77d2-e711-4bf4-a64e-aea7824bdc4e?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123549Z&X-Amz-Expires=300&X-Amz-Signature=a892dbae68d9bb031fb0438aa737e98e05e16e5d49fde0b4aba50343b9db59cb&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-08.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 111465721 (106M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-08.csv.gz’

yellow_tripdata_2019-08.csv.gz     100%[==============================================================>] 106.30M  15.7MB/s    in 7.0s    

2024-02-09 12:35:56 (15.2 MB/s) - ‘yellow_tripdata_2019-08.csv.gz’ saved [111465721/111465721]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-09.csv.gz 
--2024-02-09 12:36:03--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-09.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/58ca11b3-be2e-4186-8495-b5b1f58d2955?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123603Z&X-Amz-Expires=300&X-Amz-Signature=9a2848290e41d9b1828b2d655ae9683d66d2c48ba7f2510ed9564fd800eb63ff&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-09.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:36:03--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/58ca11b3-be2e-4186-8495-b5b1f58d2955?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123603Z&X-Amz-Expires=300&X-Amz-Signature=9a2848290e41d9b1828b2d655ae9683d66d2c48ba7f2510ed9564fd800eb63ff&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-09.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 120807645 (115M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-09.csv.gz’

yellow_tripdata_2019-09.csv.gz     100%[==============================================================>] 115.21M  17.5MB/s    in 6.6s    

2024-02-09 12:36:11 (17.4 MB/s) - ‘yellow_tripdata_2019-09.csv.gz’ saved [120807645/120807645]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-10.csv.gz 
--2024-02-09 12:36:24--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-10.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7f6b0888-e4fc-4c28-9a76-a3c47e6216aa?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123624Z&X-Amz-Expires=300&X-Amz-Signature=6b0eddc6c16b28b6f062b0523d3f7ba3324d641e3d232de95042d2e3b4c47b60&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-10.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:36:24--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7f6b0888-e4fc-4c28-9a76-a3c47e6216aa?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123624Z&X-Amz-Expires=300&X-Amz-Signature=6b0eddc6c16b28b6f062b0523d3f7ba3324d641e3d232de95042d2e3b4c47b60&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-10.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 132413025 (126M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-10.csv.gz’

yellow_tripdata_2019-10.csv.gz     100%[==============================================================>] 126.28M   211MB/s    in 0.6s    

2024-02-09 12:36:26 (211 MB/s) - ‘yellow_tripdata_2019-10.csv.gz’ saved [132413025/132413025]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-11.csv.gz 
--2024-02-09 12:36:36--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-11.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/c476a0ee-966a-4f46-b838-7e4932deb657?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123636Z&X-Amz-Expires=300&X-Amz-Signature=3663922683fb9a36471483818791e725c33af2748cc45443434f74971805196d&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-11.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:36:36--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/c476a0ee-966a-4f46-b838-7e4932deb657?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123636Z&X-Amz-Expires=300&X-Amz-Signature=3663922683fb9a36471483818791e725c33af2748cc45443434f74971805196d&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-11.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 125953067 (120M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-11.csv.gz’

yellow_tripdata_2019-11.csv.gz     100%[==============================================================>] 120.12M   238MB/s    in 0.5s    

2024-02-09 12:36:38 (238 MB/s) - ‘yellow_tripdata_2019-11.csv.gz’ saved [125953067/125953067]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-12.csv.gz 
--2024-02-09 12:36:52--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2019-12.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/a5b0d394-f159-46fe-8dac-b79fa44da6b0?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123652Z&X-Amz-Expires=300&X-Amz-Signature=bd93f85a99b3b0aa5ea78860b7f35bae6472610886eea9910665d59835f775ea&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-12.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:36:52--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/a5b0d394-f159-46fe-8dac-b79fa44da6b0?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T123652Z&X-Amz-Expires=300&X-Amz-Signature=bd93f85a99b3b0aa5ea78860b7f35bae6472610886eea9910665d59835f775ea&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2019-12.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 126446810 (121M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2019-12.csv.gz’

yellow_tripdata_2019-12.csv.gz     100%[==============================================================>] 120.59M   277MB/s    in 0.4s    

2024-02-09 12:36:54 (277 MB/s) - ‘yellow_tripdata_2019-12.csv.gz’ saved [126446810/126446810]
```

```
$ gsutil -m cp yellow* gs://de-zoomcamp-garjita-bucket/trip_data
Copying file://yellow_tripdata_2019-01.csv.gz [Content-Type=text/csv]...
Copying file://yellow_tripdata_2019-03.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-06.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-05.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-02.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-04.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-11.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-07.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-08.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-09.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-10.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-12.csv.gz [Content-Type=text/csv]...        
/ [12/12 files][  1.4 GiB/  1.4 GiB] 100% Done 125.5 MiB/s ETA 00:00:00         
Operation completed over 12 objects/1.4 GiB.
```


```
$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-01.csv.gz 
--2024-02-09 12:49:30--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-01.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1bc3de9c-af03-46e3-baa6-c4475782682d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124930Z&X-Amz-Expires=300&X-Amz-Signature=3bd51ca6d490288e7289829ac56adb5389bacbb03399422a8a785d6277f20b7c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-01.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:49:30--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1bc3de9c-af03-46e3-baa6-c4475782682d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124930Z&X-Amz-Expires=300&X-Amz-Signature=3bd51ca6d490288e7289829ac56adb5389bacbb03399422a8a785d6277f20b7c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-01.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 116338814 (111M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-01.csv.gz’

yellow_tripdata_2020-01.csv.gz     100%[==============================================================>] 110.95M   273MB/s    in 0.4s    

2024-02-09 12:49:32 (273 MB/s) - ‘yellow_tripdata_2020-01.csv.gz’ saved [116338814/116338814]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-02.csv.gz 
--2024-02-09 12:49:38--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-02.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/fdaae067-f8d4-4f9b-9e45-2e760539d309?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124939Z&X-Amz-Expires=300&X-Amz-Signature=b6db3347db0fe1bc2c91d63a435c107bbc2a8675d85e8191250aa82a1371f1d2&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-02.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:49:39--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/fdaae067-f8d4-4f9b-9e45-2e760539d309?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124939Z&X-Amz-Expires=300&X-Amz-Signature=b6db3347db0fe1bc2c91d63a435c107bbc2a8675d85e8191250aa82a1371f1d2&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-02.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 114701110 (109M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-02.csv.gz’

yellow_tripdata_2020-02.csv.gz     100%[==============================================================>] 109.39M   287MB/s    in 0.4s    

2024-02-09 12:49:40 (287 MB/s) - ‘yellow_tripdata_2020-02.csv.gz’ saved [114701110/114701110]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-03.csv.gz 
--2024-02-09 12:49:44--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-03.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/46d9461c-89fe-4084-a06f-92256b7e035c?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124945Z&X-Amz-Expires=300&X-Amz-Signature=c7ad0bea989f0f348384daaa75855b368df578ade063711c5011b07e3e1cc7b5&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-03.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:49:45--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/46d9461c-89fe-4084-a06f-92256b7e035c?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124945Z&X-Amz-Expires=300&X-Amz-Signature=c7ad0bea989f0f348384daaa75855b368df578ade063711c5011b07e3e1cc7b5&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-03.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 55004095 (52M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-03.csv.gz’

yellow_tripdata_2020-03.csv.gz     100%[==============================================================>]  52.46M   275MB/s    in 0.2s    

2024-02-09 12:49:46 (275 MB/s) - ‘yellow_tripdata_2020-03.csv.gz’ saved [55004095/55004095]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-04.csv.gz 
--2024-02-09 12:49:53--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-04.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/3a5b385e-7c0e-40bc-a73e-ae68a584a73a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124954Z&X-Amz-Expires=300&X-Amz-Signature=4764744e17f05f6c4b6354633f0afa44ecd1188626e747f029a18fe7e1bda069&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-04.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:49:54--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/3a5b385e-7c0e-40bc-a73e-ae68a584a73a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T124954Z&X-Amz-Expires=300&X-Amz-Signature=4764744e17f05f6c4b6354633f0afa44ecd1188626e747f029a18fe7e1bda069&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-04.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4429528 (4.2M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-04.csv.gz’

yellow_tripdata_2020-04.csv.gz     100%[==============================================================>]   4.22M  --.-KB/s    in 0.02s   

2024-02-09 12:49:55 (210 MB/s) - ‘yellow_tripdata_2020-04.csv.gz’ saved [4429528/4429528]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-05.csv.gz 
--2024-02-09 12:50:02--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-05.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7c737dbd-a617-477c-9b86-a939e917baa8?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125003Z&X-Amz-Expires=300&X-Amz-Signature=4f34fca7a7dc37c9a30ac8f69628be828183ddd4c50c80011298415448e47527&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-05.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:03--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/7c737dbd-a617-477c-9b86-a939e917baa8?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125003Z&X-Amz-Expires=300&X-Amz-Signature=4f34fca7a7dc37c9a30ac8f69628be828183ddd4c50c80011298415448e47527&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-05.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 6528512 (6.2M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-05.csv.gz’

yellow_tripdata_2020-05.csv.gz     100%[==============================================================>]   6.23M  --.-KB/s    in 0.03s   

2024-02-09 12:50:04 (225 MB/s) - ‘yellow_tripdata_2020-05.csv.gz’ saved [6528512/6528512]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-06.csv.gz 
--2024-02-09 12:50:09--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-06.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/426b9080-186a-40fe-873a-d8019bf00e9a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125010Z&X-Amz-Expires=300&X-Amz-Signature=3a66bfe4c3bfafe072c5c56ad2be5cff240e0f368d7ef6828105f26a5e483cc9&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-06.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:10--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/426b9080-186a-40fe-873a-d8019bf00e9a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125010Z&X-Amz-Expires=300&X-Amz-Signature=3a66bfe4c3bfafe072c5c56ad2be5cff240e0f368d7ef6828105f26a5e483cc9&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-06.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10206684 (9.7M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-06.csv.gz’

yellow_tripdata_2020-06.csv.gz     100%[==============================================================>]   9.73M  --.-KB/s    in 0.04s   

2024-02-09 12:50:11 (222 MB/s) - ‘yellow_tripdata_2020-06.csv.gz’ saved [10206684/10206684]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-07.csv.gz 
--2024-02-09 12:50:18--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-07.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1d635856-7182-4a46-a5a0-19b4e158fe1d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125018Z&X-Amz-Expires=300&X-Amz-Signature=a4f1f1324b96191f4bcfb295864e858693276d4b21e5084236158cd274fd8e18&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-07.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:19--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1d635856-7182-4a46-a5a0-19b4e158fe1d?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125018Z&X-Amz-Expires=300&X-Amz-Signature=a4f1f1324b96191f4bcfb295864e858693276d4b21e5084236158cd274fd8e18&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-07.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14720186 (14M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-07.csv.gz’

yellow_tripdata_2020-07.csv.gz     100%[==============================================================>]  14.04M  --.-KB/s    in 0.06s   

2024-02-09 12:50:19 (253 MB/s) - ‘yellow_tripdata_2020-07.csv.gz’ saved [14720186/14720186]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-08.csv.gz 
--2024-02-09 12:50:25--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-08.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/6e36a445-c307-4a05-881b-4ba8b1083fa4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125025Z&X-Amz-Expires=300&X-Amz-Signature=cccff43b88b6f3c9999d89f02823fc3f9f1a1b84139f9dd37c9dbf044ac5cffd&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-08.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:26--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/6e36a445-c307-4a05-881b-4ba8b1083fa4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125025Z&X-Amz-Expires=300&X-Amz-Signature=cccff43b88b6f3c9999d89f02823fc3f9f1a1b84139f9dd37c9dbf044ac5cffd&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-08.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 18483087 (18M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-08.csv.gz’

yellow_tripdata_2020-08.csv.gz     100%[==============================================================>]  17.63M  --.-KB/s    in 0.07s   

2024-02-09 12:50:26 (245 MB/s) - ‘yellow_tripdata_2020-08.csv.gz’ saved [18483087/18483087]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-09.csv.gz 
--2024-02-09 12:50:33--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-09.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/b44e136e-f10d-426f-aadd-ff711f0bc7d4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125034Z&X-Amz-Expires=300&X-Amz-Signature=e8b372f08335afc45eeb38d526551dd7358c623f9b0ed83ee689c515973f5f78&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-09.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:34--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/b44e136e-f10d-426f-aadd-ff711f0bc7d4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125034Z&X-Amz-Expires=300&X-Amz-Signature=e8b372f08335afc45eeb38d526551dd7358c623f9b0ed83ee689c515973f5f78&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-09.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 24510334 (23M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-09.csv.gz’

yellow_tripdata_2020-09.csv.gz     100%[==============================================================>]  23.37M  --.-KB/s    in 0.09s   

2024-02-09 12:50:34 (258 MB/s) - ‘yellow_tripdata_2020-09.csv.gz’ saved [24510334/24510334]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-10.csv.gz 
--2024-02-09 12:50:40--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-10.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1ac8bf5c-f25b-4c3d-b9c2-fac759404378?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125041Z&X-Amz-Expires=300&X-Amz-Signature=1e233290aba26045ce8f4571973ef0da1ed7281b277ebc378e525c0dc7cb0d62&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-10.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:41--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/1ac8bf5c-f25b-4c3d-b9c2-fac759404378?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125041Z&X-Amz-Expires=300&X-Amz-Signature=1e233290aba26045ce8f4571973ef0da1ed7281b277ebc378e525c0dc7cb0d62&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-10.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.109.133, 185.199.110.133, 185.199.111.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30650867 (29M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-10.csv.gz’

yellow_tripdata_2020-10.csv.gz     100%[==============================================================>]  29.23M  --.-KB/s    in 0.1s    

2024-02-09 12:50:41 (239 MB/s) - ‘yellow_tripdata_2020-10.csv.gz’ saved [30650867/30650867]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-11.csv.gz 
--2024-02-09 12:50:50--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-11.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/fc11ebdf-8100-4fab-881b-43d073e27554?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125050Z&X-Amz-Expires=300&X-Amz-Signature=2fdb8876c2b55f2ccdf130ffa5aeea79cec78681a8565b3cc588a1201c4a8f6a&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-11.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:50:50--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/fc11ebdf-8100-4fab-881b-43d073e27554?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125050Z&X-Amz-Expires=300&X-Amz-Signature=2fdb8876c2b55f2ccdf130ffa5aeea79cec78681a8565b3cc588a1201c4a8f6a&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-11.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27531023 (26M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-11.csv.gz’

yellow_tripdata_2020-11.csv.gz     100%[==============================================================>]  26.25M  11.5MB/s    in 2.3s    

2024-02-09 12:50:53 (11.5 MB/s) - ‘yellow_tripdata_2020-11.csv.gz’ saved [27531023/27531023]

$ wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-12.csv.gz 
--2024-02-09 12:53:37--  https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2020-12.csv.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/11e337cc-1826-45df-a181-34c7366e8421?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125337Z&X-Amz-Expires=300&X-Amz-Signature=f19dd58b84cf7d959f0936c1fd5cd1d55d9cb26f6732c50748c07e2b2117728c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-12.csv.gz&response-content-type=application%2Foctet-stream [following]
--2024-02-09 12:53:38--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/513814948/11e337cc-1826-45df-a181-34c7366e8421?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240209%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240209T125337Z&X-Amz-Expires=300&X-Amz-Signature=f19dd58b84cf7d959f0936c1fd5cd1d55d9cb26f6732c50748c07e2b2117728c&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=513814948&response-content-disposition=attachment%3B%20filename%3Dyellow_tripdata_2020-12.csv.gz&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 26524738 (25M) [application/octet-stream]
Saving to: ‘yellow_tripdata_2020-12.csv.gz’

yellow_tripdata_2020-12.csv.gz     100%[==============================================================>]  25.30M  16.3MB/s    in 1.6s    

2024-02-09 12:53:40 (16.3 MB/s) - ‘yellow_tripdata_2020-12.csv.gz’ saved [26524738/26524738]

```

**Copy files into a bucket**
```
$ gsutil -m cp yellow*.csv.gz gs://de-zoomcamp-garjita-bucket/trip_data
Copying file://yellow_tripdata_2019-01.csv.gz [Content-Type=text/csv]...
Copying file://yellow_tripdata_2019-03.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-02.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-04.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-07.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-11.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-05.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-01.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-08.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-05.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-12.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-06.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-06.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-07.csv.gz [Content-Type=text/csv]...
Copying file://yellow_tripdata_2019-09.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-02.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2019-10.csv.gz [Content-Type=text/csv]...
Copying file://yellow_tripdata_2020-03.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-04.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-08.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-09.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-10.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-11.csv.gz [Content-Type=text/csv]...        
Copying file://yellow_tripdata_2020-12.csv.gz [Content-Type=text/csv]...        
- [24/24 files][  1.9 GiB/  1.9 GiB] 100% Done 169.3 MiB/s ETA 00:00:00         
Operation completed over 24 objects/1.9 GiB.  
```
![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/971a2190-9115-49fc-bde6-ac8007538a90)
![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/798decc0-6f92-4c1c-a3f8-bc698cc3e082)


## External tables
BigQuery supports a few external data sources: you may query these sources directly from BigQuery even though the data itself isn't stored in BQ.

An external table is a table that acts like a standard BQ table. The table metadata (such as the schema) is stored in BQ storage but the data itself is external.

You may create an external table from a CSV or Parquet file stored in a Cloud Storage bucket:
```
CREATE OR REPLACE EXTERNAL TABLE `nytaxi.external_yellow_tripdata`
OPTIONS (
  format = 'CSV',
  uris = ['gs://de-zoomcamp-garjita-bucket/trip_data/yellow_tripdata_2019-*.csv.gz', 'gs://de-zoomcamp-garjita-bucket/trip_data/yellow_tripdata_2020-*.csv.gz']
);
```

![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/ade14ff9-6307-4ae4-9ca3-c6d5ec1ca703)
![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/35ce1370-3cd6-4aef-a2b2-862a11b50499)


### Regular Internal Table

You may import an external table into BQ as a regular internal table by copying the contents of the external table into a new internal table. For example:
```
CREATE OR REPLACE TABLE nytaxi.yellow_tripdata_non_partitoned AS
SELECT * FROM nytaxi.external_yellow_tripdata;
```
![image](https://github.com/garjitads/de-zoomcamp-2024-week3/assets/157445647/7b09d455-0455-42e6-953d-11a4c5758d7f)

### Partitions






