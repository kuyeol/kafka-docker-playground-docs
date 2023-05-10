# 🧑‍🎓 Playground Academy

Below is a collection of real use cases/issues for which a reproduction model was done using the playground.

Each example (categorized by difficulty) is composed of:

* 🔥 A small description of the issue.
* 🤯 A detailed description with all required information.
* 📍 Series of step to follow. An (optional) solution for each step is provided in following step.
* 🌟 Results with a link to the full reproduction model, which can be executed directly using playground.

🧠 The goal is to let users do the reproduction by themselves. This is the only way to learn 😀.

> [!TIP]
> This is the counter of ![reproduction models](https://img.shields.io/endpoint?url=https://gist.githubusercontent.com/vdesabou/b7d51da11b2e8a7bdd1d2e45d4aaa2e5/raw/badges.json) made with playground so far..
> 


## ⭐ Beginner

### StackOverflowError with S3 sink connector

<!-- tabs:start -->
#### **🔥 Description**

User is getting StackOverflowError with S3 sink connector. See stack trace in Details section.

#### **🤯 Details**

Versions used:

* 🎯 CP: 7.3.1

* 🔗 S3 sink: 10.3.3

S3 sink config:

```json
{
  "aws.access.key.id": "xxx",
  "aws.secret.access.key": "xxx",
  "connector.class": "io.confluent.connect.s3.S3SinkConnector",
  "errors.log.enable": "true",
  "errors.tolerance": "all",
  "flush.size": "50000",
  "format.class": "io.confluent.connect.s3.format.parquet.ParquetFormat",
  "key.converter": "org.apache.kafka.connect.storage.StringConverter",
  "locale": "de_DE",
  "partitioner.class": "io.confluent.connect.storage.partitioner.DailyPartitioner",
  "rotate.interval.ms": "3600000",
  "s3.bucket.name": "xxx",
  "s3.region": "eu-west-1",
  "storage.class": "io.confluent.connect.s3.storage.S3Storage",
  "tasks.max": "1",
  "timestamp.extractor": "Record",
  "timezone": "UTC",
  "topics.dir": "xxx",
  "topics": "topic1",
  "value.converter.schema.registry.url": "xxx",
  "value.converter": "io.confluent.connect.avro.AvroConverter"
}
```

AVRO schema for topic `topic1` (value):

```json
{
    "type": "record",
    "namespace": "acme",
    "name": "Characteristic",
    "fields": [
        {
            "name": "physicalCharacteristic",
            "type": [
                "null",
                {
                    "type": "record",
                    "name": "PhysicalCharacteristic",
                    "fields": [
                        {
                            "name": "children",
                            "type": [
                                "null",
                                {
                                    "type": "array",
                                    "items": "PhysicalCharacteristic"
                                }
                            ],
                            "default": null
                        }
                    ]
                }
            ]
        }
    ]
}
```

User is getting in logs:

```log
[2022-12-12 16:44:06,186] ERROR [s3-sink|task-0] WorkerSinkTask{id=s3-sink-0} Task threw an uncaught and unrecoverable exception. Task is being killed and will not recover until manually restarted (org.apache.kafka.connect.runtime.WorkerTask:208)
org.apache.kafka.connect.errors.ConnectException: Exiting WorkerSinkTask due to unrecoverable exception.
	at org.apache.kafka.connect.runtime.WorkerSinkTask.deliverMessages(WorkerSinkTask.java:618)
	at org.apache.kafka.connect.runtime.WorkerSinkTask.poll(WorkerSinkTask.java:334)
	at org.apache.kafka.connect.runtime.WorkerSinkTask.iteration(WorkerSinkTask.java:235)
	at org.apache.kafka.connect.runtime.WorkerSinkTask.execute(WorkerSinkTask.java:204)
	at org.apache.kafka.connect.runtime.WorkerTask.doRun(WorkerTask.java:201)
	at org.apache.kafka.connect.runtime.WorkerTask.run(WorkerTask.java:256)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
Caused by: java.lang.StackOverflowError
	at org.apache.parquet.avro.AvroSchemaConverter.convertFields(AvroSchemaConverter.java:130)
	at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:163)
	at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:169)
	at org.apache.parquet.avro.AvroSchemaConverter.convertUnion(AvroSchemaConverter.java:226)
	at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:182)
	at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:141)
```

#### **📍 Step 1**

*  🎯 Choose the best [example](/content) to use as basis.

* 🛠 [Bootstrap](/reusables?id=%f0%9f%9b%a0-bootstrap-reproduction-model) your reproduction model!

> [!TIP]
> Do not forget to [🪄 Specify versions](/how-to-use?id=%f0%9f%aa%84-specify-versions) for CP and Connector before running the test !

#### **📍 Step 2**
<!-- select:start -->
<!-- select-menu-labels: 🙋 See solution for previous step ? -->
#### --No--
#### --Yes--


🛠 Bootstrap reproduction model was done as following (use tab completion to select the files s3-sink.sh using `fzf`):

```bash
playground bootstrap-reproduction-model --file s3-sink<tab> --description "000001 StackOverflowError with S3 sink connector" --producer avro --tag 7.3.1 --connector-tag 10.3.3
```

💡 Explanations:

* [s3-sink.sh](https://github.com/vdesabou/kafka-docker-playground/blob/master/connect/connect-aws-s3-sink/s3-sink.sh) is the closest example.
* The problem seems related to input data, so using [avro java producer](/reusables?id=%e2%99%a8%ef%b8%8f-java-producers) seems the right thing to do.

> [!NOTE]
> As the key converter is StringConverter, we should not use `avro-with-key` for `--producer` flag.

<!-- select:end -->

* 👉 Follow steps from [♨️ Java producers](/reusables?id=%e2%99%a8%ef%b8%8f-java-producers) in order to produce same data as user.

> [!TIP]
> Since we did not use `--producer-schema-value` flag when bootstrapping the reproduction model, step 3 in [♨️ Java producers](/reusables?id=%e2%99%a8%ef%b8%8f-java-producers) should be done manually.

#### **📍 Step 3**
<!-- select:start -->
<!-- select-menu-labels: 🙋 See solution for previous step ? -->
#### --No--
#### --Yes--

⌨️ Here are the steps to follow:

Update `producer-repro-000001/src/main/resources/schema/customer.avsc` file with content of user schema:

```
{
    "type": "record",
    "namespace": "acme",
    "name": "Characteristic",
    "fields": [
        {
            "name": "physicalCharacteristic",
            "type": [
                "null",
                {
                    "type": "record",
                    "name": "PhysicalCharacteristic",
                    "fields": [
                        {
                            "name": "children",
                            "type": [
                                "null",
                                {
                                    "type": "array",
                                    "items": "PhysicalCharacteristic"
                                }
                            ],
                            "default": null
                        }
                    ]
                }
            ]
        }
    ]
}
```

And replace:

```json
    "namespace": "acme",
    "name": "Characteristic",
```

by

```json
    "namespace": "com.github.vdesabou",
    "name": "Customer",
```

Note that you could have use step 2 when bootstrapping reproduction model and this step would be fully automated:

Example:

```bash
playground bootstrap-reproduction-model -f s3-sink<tab> -d "000001 StackOverflowError with S3 sink connector" -p avro --producer-schema-value schema<tab>
```

<!-- select:end -->
👉 Adapt the example to user details and run it !

#### **📍 Step 4**
<!-- select:start -->
<!-- select-menu-labels: 🙋 See solution for previous step ? -->
#### --No--
#### --Yes--

* The only relevant connector configuration for that use case is the fact that Parquet format is used, so I just replaced `format.class`to use Parquet instead of Avro, i.e:

```json
"format.class": "io.confluent.connect.s3.format.parquet.ParquetFormat"`:
```

* Full connector config:

```json
curl -X PUT \
     -H "Content-Type: application/json" \
     --data '{
               "connector.class": "io.confluent.connect.s3.S3SinkConnector",
               "key.converter": "org.apache.kafka.connect.storage.StringConverter",
               "value.converter": "io.confluent.connect.avro.AvroConverter",
               "value.converter.schema.registry.url": "http://schema-registry:8081",
               "tasks.max": "1",
               "topics": "customer_avro",
               "s3.region": "'"$AWS_REGION"'",
               "s3.bucket.name": "'"$AWS_BUCKET_NAME"'",
               "topics.dir": "'"$TAG"'",
               "s3.part.size": 52428801,
               "flush.size": "3",
               "aws.access.key.id" : "'"$AWS_ACCESS_KEY_ID"'",
               "aws.secret.access.key": "'"$AWS_SECRET_ACCESS_KEY"'",
               "storage.class": "io.confluent.connect.s3.storage.S3Storage",
               "format.class": "io.confluent.connect.s3.format.parquet.ParquetFormat",
               "schema.compatibility": "NONE"
          }' \
     http://localhost:8083/connectors/s3-sink/config | jq .
```

<!-- select:end -->
🥁 So...did you reproduce ??

#### **🌟 Results**

* Check the connector status

```bash
$ 15:18:58 ℹ️ 🧩 Displaying connector(s) status
Name                           Status       Tasks                          Stack Trace                                       
-------------------------------------------------------------------------------------------------------------
s3-sink                        ✅ RUNNING  0:🛑 FAILED                   tasks: org.apache.kafka.connect.errors.ConnectException: Exiting WorkerSinkTask due to unrecoverable exception.
        at org.apache.kafka.connect.runtime.WorkerSinkTask.deliverMessages(WorkerSinkTask.java:618)
        at org.apache.kafka.connect.runtime.WorkerSinkTask.poll(WorkerSinkTask.java:334)
        at org.apache.kafka.connect.runtime.WorkerSinkTask.iteration(WorkerSinkTask.java:235)
        at org.apache.kafka.connect.runtime.WorkerSinkTask.execute(WorkerSinkTask.java:204)
        at org.apache.kafka.connect.runtime.WorkerTask.doRun(WorkerTask.java:201)
        at org.apache.kafka.connect.runtime.WorkerTask.run(WorkerTask.java:256)
        at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
        at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
        at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
        at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
        at java.base/java.lang.Thread.run(Thread.java:829)
Caused by: java.lang.StackOverflowError
        at org.apache.avro.Schema$RecordSchema.getFields(Schema.java:902)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:163)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:169)
        at org.apache.parquet.avro.AvroSchemaConverter.convertUnion(AvroSchemaConverter.java:226)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:182)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:141)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:244)
        at org.apache.parquet.avro.AvroSchemaConverter.convertFields(AvroSchemaConverter.java:135)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:163)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:169)
        at org.apache.parquet.avro.AvroSchemaConverter.convertUnion(AvroSchemaConverter.java:226)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:182)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:141)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:244)
        at org.apache.parquet.avro.AvroSchemaConverter.convertFields(AvroSchemaConverter.java:135)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:163)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:169)
        at org.apache.parquet.avro.AvroSchemaConverter.convertUnion(AvroSchemaConverter.java:226)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:182)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:141)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:244)
        at org.apache.parquet.avro.AvroSchemaConverter.convertFields(AvroSchemaConverter.java:135)
        at org.apache.parquet.avro.AvroSchemaConverter.convertField(AvroSchemaConverter.java:163)
```

👍 It failed as expected 

Note: You can also check the logs for a pattern `StackOverflowError` ERROR using

```bash
$ playground container logs -c connect --wait-for-log "StackOverflowError"
15:25:00 ℹ️ ⌛ Waiting up to 600 seconds for message StackOverflowError to be present in connect container logs...
java.lang.StackOverflowError
Caused by: java.lang.StackOverflowError
15:25:00 ℹ️ The log is there !
```

* Full example is available [here](https://github.com/vdesabou/kafka-docker-playground/blob/master/docs-examples/connect-connect-aws-s3-sink/s3-sink-repro-000001-stackoverflowerror-with-s3-sink-connector.sh)


<!-- tabs:end -->

## ⭐⭐ Intermediate

Work in Progress...

## ⭐⭐⭐ Expert

Work in Progress...