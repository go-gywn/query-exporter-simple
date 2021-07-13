# query-exporter-simple

Simple query exporter to explain

## Build & Run
```bash
$ go build .
```

## Config example
```yaml
dsn: test:test123@tcp(127.0.0.1:3306)/information_schema
metrics:
  process_count_by_host:
    query: "select user, substring_index(host, ':', 1) host, count(*) sessions from information_schema.processlist group by 1,2 "
    type: gauge
    description: "process count by host"
    labels: ["user","host"]
    value: sessions
  process_count_by_user:
    query: "select user, count(*) sessions from information_schema.processlist group by 1 "
    type: gauge
    description: "process count by user"
    labels: ["user"]
    value: sessions
```

## Run
```bash
$ ./query-exporter --bind="0.0.0.0:9104" --config="config.yml"
INFO[0000] Regist version collector - query_exporter    
INFO[0000] metric description for "process_count_by_host" registerd 
INFO[0000] metric description for "process_count_by_user" registerd 
INFO[0000] HTTP handler path - /metrics                 
INFO[0000] Starting http server - 0.0.0.0:9104  
```

## Metrics
```bash
curl 127.0.0.1:9104/metrics 
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 0
go_gc_duration_seconds{quantile="0.25"} 0

.. skip ..

# HELP go_threads Number of OS threads created.
# TYPE go_threads gauge
go_threads 7
# HELP query_exporter_build_info A metric with a constant '1' value labeled by version, revision, branch, and goversion from which query_exporter was built.
# TYPE query_exporter_build_info gauge
query_exporter_build_info{branch="",goversion="go1.16.5",revision="",version=""} 1
# HELP query_exporter_process_count_by_host process count by host
# TYPE query_exporter_process_count_by_host gauge
query_exporter_process_count_by_host{host="localhost",user="event_scheduler"} 1
query_exporter_process_count_by_host{host="localhost",user="test"} 1
# HELP query_exporter_process_count_by_user process count by user
# TYPE query_exporter_process_count_by_user gauge
query_exporter_process_count_by_user{user="event_scheduler"} 1
query_exporter_process_count_by_user{user="test"} 1
```

Enjoy your own exporter!!