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


### Actual behavior

On logstash-6.1.0 this leads to:

```
filebeat_1         | 2017/12/17 22:21:09.785211 output.go:92: ERR Failed to publish events: client is not connected
logstash-ingest_1  | [2017-12-17T22:21:09,794][INFO ][org.logstash.beats.BeatsHandler] [local: 192.168.0.2:5044, remote: 192.168.0.3:47964] Exception: -1
filebeat_1         | 2017/12/17 22:21:09.795726 async.go:235: ERR Failed to publish events caused by: read tcp 192.168.0.3:47964->192.168.0.2:5044: read: connection reset by peer
filebeat_1         | 2017/12/17 22:21:09.796199 async.go:235: ERR Failed to publish events caused by: client is not connected

filebeat_1         | 2017/12/17 22:21:10.796564 output.go:92: ERR Failed to publish events: client is not connected
logstash-ingest_1  | [2017-12-17T22:21:10,808][INFO ][org.logstash.beats.BeatsHandler] [local: 192.168.0.2:5044, remote: 192.168.0.3:47966] Exception: -1
filebeat_1         | 2017/12/17 22:21:10.809409 async.go:235: ERR Failed to publish events caused by: read tcp 192.168.0.3:47966->192.168.0.2:5044: read: connection reset by peer
filebeat_1         | 2017/12/17 22:21:10.810772 async.go:235: ERR Failed to publish events caused by: client is not connected

filebeat_1         | 2017/12/17 22:21:11.812427 output.go:92: ERR Failed to publish events: client is not connected
logstash-ingest_1  | [2017-12-17T22:21:11,818][INFO ][org.logstash.beats.BeatsHandler] [local: 192.168.0.2:5044, remote: 192.168.0.3:47968] Exception: -1
filebeat_1         | 2017/12/17 22:21:11.819983 async.go:235: ERR Failed to publish events caused by: read tcp 192.168.0.3:47968->192.168.0.2:5044: read: connection reset by peer
filebeat_1         | 2017/12/17 22:21:11.821797 async.go:235: ERR Failed to publish events caused by: client is not connected

... (repeating indefinitely)
```