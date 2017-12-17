This is a test case for [logstash issue 8852](https://github.com/elastic/logstash/issues/8852).

### Steps to reproduce

```bash
git clone https://github.com/lukasgraf/logstash-8852-testcase.git
cd logstash-8852-testcase
docker-compose up
cp sample.log filebeat/logs-pickup/
```

### Expected behavior

Event gets parsed correctly (works on logstash-6.0.0):

```
logstash-ingest_1  | {
logstash-ingest_1  |     "@timestamp" => 2017-12-17T22:30:34.864Z,
logstash-ingest_1  |         "offset" => 28,
logstash-ingest_1  |           "data" => {
logstash-ingest_1  |            "" => 2,
logstash-ingest_1  |         "foo" => 1
logstash-ingest_1  |     },
logstash-ingest_1  |           "beat" => {
logstash-ingest_1  |             "name" => "a72813845934",
logstash-ingest_1  |         "hostname" => "a72813845934",
logstash-ingest_1  |          "version" => "6.0.0"
logstash-ingest_1  |     },
logstash-ingest_1  |       "@version" => "1",
logstash-ingest_1  |           "host" => "a72813845934",
logstash-ingest_1  |     "prospector" => {
logstash-ingest_1  |         "type" => "log"
logstash-ingest_1  |     },
logstash-ingest_1  |         "source" => "/var/logs-pickup/sample.log",
logstash-ingest_1  |           "tags" => [
logstash-ingest_1  |         [0] "beats_input_raw_event"
logstash-ingest_1  |     ]
logstash-ingest_1  | }
```
