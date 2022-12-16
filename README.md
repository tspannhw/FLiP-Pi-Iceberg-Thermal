# FLiP-Pi-Iceberg-Thermal
Apache Iceberg + Apache Pulsar + Thermal Sensor Data from a Raspberry Pi

![ice](https://raw.githubusercontent.com/streamnative/pulsar-io-lakehouse/v2.10.1.12/docs/lakehouse-sink.png)


### Steps

* Run Apache Pulsar (standalone, docker, baremetal cluster, VM cluster, K8 cluster, AWS Marketplace Pulsar, StreamNative Cloud)
* Run Iceberg (docker, ...)
* Deploy Pulsar connector
* Send data to Pulsar topic
* Query Iceberg


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

### Spark Shell

````


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
