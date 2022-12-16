# FLiP-Pi-Iceberg-Thermal
Apache Iceberg + Apache Pulsar + Thermal Sensor Data from a Raspberry Pi

![ice](https://raw.githubusercontent.com/streamnative/pulsar-io-lakehouse/v2.10.1.12/docs/lakehouse-sink.png)


### Steps

* Run Apache Pulsar 2.10.2 (standalone, docker, baremetal cluster, VM cluster, K8 cluster, AWS Marketplace Pulsar, StreamNative Cloud)
* Run Apache Iceberg (docker, ...) 1.1.0
* Run Apache Spark 3.2
* Deploy Pulsar connector
* Send data to Pulsar topic
* Query Iceberg in Spark

### Sensor Python App Sending messages

````

{'uuid': 'thrml_wse_20221216202136', 'ipaddress': '192.168.1.179', 'cputempf': 122, 'runtime': 0, 'host': 'thermal', 'hostname': 'thermal', 'macaddress': 'e4:5f:01:7c:3f:34', 'endtime': '1671222096.350368', 'te': '0.0005612373352050781', 'cpu': 5.5, 'diskusage': '101858.0 MB', 'memory': 9.9, 'rowid': '20221216202136_a0e9eae8-3b4f-4222-95c6-7657ba0e12e2', 'systemtime': '12/16/2022 15:21:41', 'ts': 1671222101, 'starttime': '12/16/2022 15:21:36', 'datetimestamp': '2022-12-16 20:21:40.012859+00:00', 'temperature': 30.5959, 'humidity': 26.07, 'co2': 767.0, 'totalvocppb': 0.0, 'equivalentco2ppm': 400.0, 'pressure': 99773.53, 'temperatureicp': 86.0}

````

### Pulsar Sink Deploy

````
bin/pulsar-admin sink stop --name iceberg_sink --namespace default --tenant public

bin/pulsar-admin sinks delete --tenant public --namespace default --name iceberg_sink

bin/pulsar-admin sink create --sink-config-file conf/iceberg.json

````

### Pulsar Sink Status

````
bin/pulsar-admin sinks status --tenant public --namespace default --name iceberg_sink

{
  "numInstances" : 1,
  "numRunning" : 1,
  "instances" : [ {
    "instanceId" : 0,
    "status" : {
      "running" : true,
      "error" : "",
      "numRestarts" : 0,
      "numReadFromPulsar" : 10,
      "numSystemExceptions" : 0,
      "latestSystemExceptions" : [ ],
      "numSinkExceptions" : 0,
      "latestSinkExceptions" : [ ],
      "numWrittenToSink" : 10,
      "lastReceivedTime" : 1671220772536,
      "workerId" : "c-standalone-fw-127.0.0.1-8080"
    }
  } ]
}

````

### Iceberg data written via Pulsar Lakehouse Cloud Sink

````
ls -lt /Users/tspann/Downloads/iceberg/iceberg_sink_test/ice_sink_thermal
total 0
drwxr-xr-x  94 tspann  staff  3008 Dec 16 15:45 metadata
drwxr-xr-x  34 tspann  staff  1088 Dec 16 15:45 data

ice_sink_thermal/metadata
total 856
-rw-r--r--  1 tspann  staff      2 Dec 16 15:45 version-hint.text
-rw-r--r--  1 tspann  staff  19283 Dec 16 15:45 v15.metadata.json
-rw-r--r--  1 tspann  staff   4352 Dec 16 15:45 snap-8802315029762513718-1-78627844-0d69-4c2e-87db-016b9fdac119.avro
-rw-r--r--  1 tspann  staff   7536 Dec 16 15:45 78627844-0d69-4c2e-87db-016b9fdac119-m0.avro
-rw-r--r--  1 tspann  staff  18303 Dec 16 15:43 v14.metadata.json
-rw-r--r--  1 tspann  staff   4315 Dec 16 15:43 snap-1218246990201737819-1-1253a40d-fae5-4919-9d71-be51af402899.avro

iceberg_sink_test/ice_sink_thermal/data
total 360
-rw-r--r--  1 tspann  staff  9771 Dec 16 15:45 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00014.parquet
-rw-r--r--  1 tspann  staff  9782 Dec 16 15:43 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00013.parquet
-rw-r--r--  1 tspann  staff  9733 Dec 16 15:41 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00012.parquet
-rw-r--r--  1 tspann  staff  9637 Dec 16 15:39 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00011.parquet
-rw-r--r--  1 tspann  staff  9722 Dec 16 15:37 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00010.parquet
-rw-r--r--  1 tspann  staff  9663 Dec 16 15:35 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00009.parquet
-rw-r--r--  1 tspann  staff  9671 Dec 16 15:33 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00008.parquet
-rw-r--r--  1 tspann  staff  9652 Dec 16 15:31 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00007.parquet
-rw-r--r--  1 tspann  staff  9716 Dec 16 15:29 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00006.parquet
-rw-r--r--  1 tspann  staff  9731 Dec 16 15:27 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00005.parquet
-rw-r--r--  1 tspann  staff  9639 Dec 16 15:25 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00004.parquet
-rw-r--r--  1 tspann  staff  9721 Dec 16 15:23 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00003.parquet
-rw-r--r--  1 tspann  staff  7414 Dec 16 15:21 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00002.parquet
-rw-r--r--  1 tspann  staff  8492 Dec 16 14:59 00000-1-951a37fc-5069-4201-94fa-4ef9975f6293-00001.parquet
-rw-r--r--  1 tspann  staff  7978 Dec 16 14:56 00000-1-2acfc6ba-4f49-44c7-8f17-1f3491484fd1-00001.parquet
-rw-r--r--  1 tspann  staff  7886 Dec 16 14:54 00000-1-7e714ed7-0ba5-41a4-b8e1-1e1d261e3b83-00001.parquet

Schema Embedded in Parquet File

 {"type":"struct","schema-id":0,"fields":[{"id":1,"name":"uuid","required":true,"type":"string"},{"id":2,"name":"ipaddress","required":true,"type":"string"},{"id":3,"name":"cputempf","required":true,"type":"int"},{"id":4,"name":"runtime","required":true,"type":"int"},{"id":5,"name":"host","required":true,"type":"string"},{"id":6,"name":"hostname","required":true,"type":"string"},{"id":7,"name":"macaddress","required":true,"type":"string"},{"id":8,"name":"endtime","required":true,"type":"string"},{"id":9,"name":"te","required":true,"type":"string"},{"id":10,"name":"cpu","required":true,"type":"float"},{"id":11,"name":"diskusage","required":true,"type":"string"},{"id":12,"name":"memory","required":true,"type":"float"},{"id":13,"name":"rowid","required":true,"type":"string"},{"id":14,"name":"systemtime","required":true,"type":"string"},{"id":15,"name":"ts","required":true,"type":"int"},{"id":16,"name":"starttime","required":true,"type":"string"},{"id":17,"name":"datetimestamp","required":true,"type":"string"},{"id":18,"name":"temperature","required":true,"type":"float"},{"id":19,"name":"humidity","required":true,"type":"float"},{"id":20,"name":"co2","required":true,"type":"float"},{"id":21,"name":"totalvocppb","required":true,"type":"float"},{"id":22,"name":"equivalentco2ppm","required":true,"type":"float"},{"id":23,"name":"pressure","required":true,"type":"float"},{"id":24,"name":"temperatureicp","required":true,"type":"float"}]}Jparquet-mr version 1.12.0 (build db75a6815f2ba1d1ee89d1a90aeb296f1f3a8f20)
 
````

### Setup

* Download Spark 3.2_2.12
* Download iceberg-spark-runtime-3.2_2.12:1.1.0

### Run Spark Shell

````
bin/spark-sql --packages org.apache.iceberg:iceberg-spark-runtime-3.2_2.12:1.1.0\
    --conf spark.sql.extensions=org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions \
    --conf spark.sql.catalog.spark_catalog=org.apache.iceberg.spark.SparkSessionCatalog \
    --conf spark.sql.catalog.spark_catalog.type=hive \
    --conf spark.sql.catalog.local=org.apache.iceberg.spark.SparkCatalog \
    --conf spark.sql.catalog.local.type=hadoop \
    --conf spark.sql.catalog.local.warehouse=/Users/tspann/Downloads/iceberg/iceberg_sink_test
````

### Spark Shell

````
desc local.ice_sink_thermal;
uuid                	string
ipaddress           	string
cputempf            	int
runtime             	int
host                	string
hostname            	string
macaddress          	string
endtime             	string
te                  	string
cpu                 	float
diskusage           	string
memory              	float
rowid               	string
systemtime          	string
ts                  	int
starttime           	string
datetimestamp       	string
temperature         	float
humidity            	float
co2                 	float
totalvocppb         	float
equivalentco2ppm    	float
pressure            	float
temperatureicp      	float

# Partitioning
Not partitioned

select * from local.ice_sink_thermal limit 10;

thrml_zlq_20221216202731	192.168.1.179	122	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222451.063736	0.0005979537963867188	11.1	101858.0 MB	10.0	20221216202731_e414e311-7928-4408-91de-b44666cd14db	12/16/2022 15:27:35	1671222455	12/16/2022 15:27:31	2022-12-16 20:27:34.827283+00:00	26.8868	31.76	771.0	14.0	405.0	99783.77	86.0
thrml_fpa_20221216202735	192.168.1.179	122	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222455.8950574	0.00046181678771972656	5.5	101858.0 MB	10.0	20221216202735_92a2af14-ebcb-42fd-a935-5a01a47ff95e	12/16/2022 15:27:40	1671222460	12/16/2022 15:27:35	2022-12-16 20:27:39.659337+00:00	26.8761	31.74	771.0	6.0	400.0	99784.8	86.0
thrml_gpv_20221216202740	192.168.1.179	122	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222460.7061012	0.0006053447723388672	5.5	101858.0 MB	10.0	20221216202740_68f88218-1b40-4f0a-85e3-3fb8f447d65b	12/16/2022 15:27:45	1671222465	12/16/2022 15:27:40	2022-12-16 20:27:44.369295+00:00	26.8761	31.72	770.0	7.0	65535.0	99782.18	85.0
thrml_gxu_20221216202745	192.168.1.179	121	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222465.500708	0.0006241798400878906	6.0	101858.0 MB	10.0	20221216202745_28685c57-88f5-422b-be23-b32ea12a0d75	12/16/2022 15:27:50	1671222470	12/16/2022 15:27:45	2022-12-16 20:27:49.161722+00:00	26.8681	31.83	771.0	12.0	65535.0	99778.97	85.0
thrml_nyn_20221216202750	192.168.1.179	122	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222470.212329	0.0006175041198730469	5.5	101858.0 MB	10.0	20221216202750_bbbb7fa7-cebf-4414-b538-30ce84828cea	12/16/2022 15:27:55	1671222475	12/16/2022 15:27:50	2022-12-16 20:27:53.976872+00:00	26.8601	31.78	771.0	6.0	65535.0	99783.55	86.0
thrml_oxl_20221216202755	192.168.1.179	123	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222475.1313043	0.0006723403930664062	6.6	101858.0 MB	10.0	20221216202755_bda36e1c-246f-4d99-8b39-b558111a1d9e	12/16/2022 15:27:59	1671222479	12/16/2022 15:27:55	2022-12-16 20:27:58.794626+00:00	26.8307	31.73	771.0	7.0	65535.0	99785.26	86.0
thrml_rvg_20221216202759	192.168.1.179	121	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222479.8391178	0.00048804283142089844	5.5	101858.0 MB	10.4	20221216202759_25b8ee3b-6b59-42e6-8c4c-a8419b76ea40	12/16/2022 15:28:04	1671222484	12/16/2022 15:27:59	2022-12-16 20:28:03.601685+00:00	26.8441	31.76	771.0	6.0	406.0	99786.36	86.0
thrml_wbl_20221216202804	192.168.1.179	123	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222484.6672618	0.0006029605865478516	5.5	101858.0 MB	10.0	20221216202804_7bd3de16-dbcd-4107-8ab0-b184a2eaf523	12/16/2022 15:28:09	1671222489	12/16/2022 15:28:04	2022-12-16 20:28:08.431971+00:00	26.8842	31.79	770.0	9.0	65535.0	99787.27	86.0
thrml_vwj_20221216202809	192.168.1.179	122	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222489.4752936	0.0006010532379150391	9.9	101858.0 MB	10.0	20221216202809_fed14b6d-b211-48ad-b31f-0a486b8d0f0d	12/16/2022 15:28:14	1671222494	12/16/2022 15:28:09	2022-12-16 20:28:13.137929+00:00	26.9002	31.78	770.0	5.0	400.0	99782.4	86.0
thrml_oox_20221216202814	192.168.1.179	123	0	thermal	thermal	e4:5f:01:7c:3f:34	1671222494.2805836	0.0005502700805664062	4.8	101858.0 MB	10.0	20221216202814_89526e7c-980b-4dbe-8257-c1c6944cdbd3	12/16/2022 15:28:18	1671222498	12/16/2022 15:28:14	2022-12-16 20:28:17.943010+00:00	26.9162	31.72	769.0	1.0	65535.0	99789.35	86.0
Time taken: 0.835 seconds, Fetched 10 row(s)

````

### References

* https://github.com/tspannhw/FLiP-Pi-DeltaLake-Thermal
* https://iceberg.apache.org/docs/latest/getting-started/
* https://github.com/streamnative/pulsar-io-lakehouse/blob/master/docs/lakehouse-sink.md
* https://streamnative.io/blog/release/2022-12-14-announcing-the-iceberg-sink-connector-for-apache-pulsar/
* https://hub.streamnative.io/connectors/lakehouse-sink/v2.10.1.12/
* https://github.com/tspannhw/FLiP-Pi-Thermal
* https://dzone.com/articles/pulsar-in-python-on-pi
* https://github.com/tabular-io/docker-spark-iceberg
* https://iceberg.apache.org/docs/latest/getting-started/
* https://stackoverflow.com/questions/73791829/delta-lake-sink-connector-for-apache-pulsar-with-minio-throws-java-lang-illegal


2022/2023 - Tim Spann - @PaaSDev
