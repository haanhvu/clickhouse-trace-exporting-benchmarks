The config is the same as [step1-originalschema](https://github.com/haanhvu/jaeger-clickhouse-benchmark/tree/main/setup/opentelemetry-collector/step1-originalschema).


## New ORDER BY

Hardware: [n3.xlarge.x86](https://deploy.equinix.com/product/servers/n3-xlarge/)

To benchmark the new `ORDER BY`, we need to manually create the table with the new `ORDER BY` in ClickHouse server before setting up collector-tracegen pipeline:
```
CREATE TABLE IF NOT EXISTS otel_traces (
     Timestamp DateTime64(9) CODEC(Delta, ZSTD(1)),
     TraceId String CODEC(ZSTD(1)),
     SpanId String CODEC(ZSTD(1)),
     ParentSpanId String CODEC(ZSTD(1)),
     TraceState String CODEC(ZSTD(1)),
     SpanName LowCardinality(String) CODEC(ZSTD(1)),
     SpanKind LowCardinality(String) CODEC(ZSTD(1)),
     ServiceName LowCardinality(String) CODEC(ZSTD(1)),
     ResourceAttributes Map(LowCardinality(String), String) CODEC(ZSTD(1)),
     SpanAttributes Map(LowCardinality(String), String) CODEC(ZSTD(1)),
     Duration Int64 CODEC(ZSTD(1)),
     StatusCode LowCardinality(String) CODEC(ZSTD(1)),
     StatusMessage String CODEC(ZSTD(1)),
     Events Nested (
         Timestamp DateTime64(9),
         Name LowCardinality(String),
         Attributes Map(LowCardinality(String), String)
     ) CODEC(ZSTD(1)),
     Links Nested (
         TraceId String,
         SpanId String,
         TraceState String,
         Attributes Map(LowCardinality(String), String)
     ) CODEC(ZSTD(1)),
     INDEX idx_trace_id TraceId TYPE bloom_filter(0.001) GRANULARITY 1,
     INDEX idx_res_attr_key mapKeys(ResourceAttributes) TYPE bloom_filter(0.01) GRANULARITY 1,
     INDEX idx_res_attr_value mapValues(ResourceAttributes) TYPE bloom_filter(0.01) GRANULARITY 1,
     INDEX idx_span_attr_key mapKeys(SpanAttributes) TYPE bloom_filter(0.01) GRANULARITY 1,
     INDEX idx_span_attr_value mapValues(SpanAttributes) TYPE bloom_filter(0.01) GRANULARITY 1,
     INDEX idx_duration Duration TYPE minmax GRANULARITY 1
) ENGINE MergeTree()

PARTITION BY toDate(Timestamp)
ORDER BY (ServiceName, SpanName, toUnixTimestamp(Timestamp), Duration, SpanAttributes, TraceId)
SETTINGS index_granularity=8192, ttl_only_drop_parts = 1;
```

Docker command: `docker run --network host --name otel -p 147.28.187.86:4317:4317 -p 147.28.187.86:8888:8888 -v $(pwd)/config.yaml:/etc/otelcol-contrib/config.yaml otel/opentelemetry-collector-contrib`


## New data types

To benchmark the new data types, please use [this branch of the ClickHouse exporter](https://github.com/haanhvu/opentelemetry-collector-contrib).

There's a detailed explanation in the [README of the branch](https://github.com/haanhvu/opentelemetry-collector-contrib/blob/newtypes/exporter/clickhouseexporter/README.md).

Hardware: [n3.xlarge.x86](https://deploy.equinix.com/product/servers/n3-xlarge/) (We need bigger hardware to build collector from source)
