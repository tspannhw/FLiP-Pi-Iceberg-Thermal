# FLiP-Pi-Iceberg-Thermal
Apache Iceberg + Apache Pulsar + Thermal Sensor Data from a Raspberry Pi

![ice](https://raw.githubusercontent.com/streamnative/pulsar-io-lakehouse/v2.10.1.12/docs/lakehouse-sink.png)


### Steps

* Run Apache Pulsar (standalone, docker, baremetal cluster, VM cluster, K8 cluster, AWS Marketplace Pulsar, StreamNative Cloud)
* Run Iceberg (docker, ...)
* Deploy Pulsar connector
* Send data to Pulsar topic
* Query Iceberg

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

### References

* https://github.com/tspannhw/FLiP-Pi-DeltaLake-Thermal
* https://github.com/streamnative/pulsar-io-lakehouse/blob/master/docs/lakehouse-sink.md
* https://streamnative.io/blog/release/2022-12-14-announcing-the-iceberg-sink-connector-for-apache-pulsar/
* https://hub.streamnative.io/connectors/lakehouse-sink/v2.10.1.12/
* https://github.com/tspannhw/FLiP-Pi-Thermal
* https://dzone.com/articles/pulsar-in-python-on-pi
* https://github.com/tabular-io/docker-spark-iceberg
* https://iceberg.apache.org/docs/latest/getting-started/
* https://stackoverflow.com/questions/73791829/delta-lake-sink-connector-for-apache-pulsar-with-minio-throws-java-lang-illegal


2022/2023 - Tim Spann - @PaaSDev
