Hardware: [c3.small.x86](https://deploy.equinix.com/product/servers/c3-small/)

To export traces from Jaeger Collector to ClickHouse, we need jaeger-clickhouse plugin binary, which is available in the [jaeger-clickhouse repo](https://github.com/jaegertracing/jaeger-clickhouse/releases).

Docker command: `docker run --network host --name jaeger -e JAEGER_DISABLED=true -p 139.178.81.65:4317:4317 -p 139.178.81.65:14269:14269 -v "${PWD}:/data" -e SPAN_STORAGE_TYPE=grpc-plugin jaegertracing/jaeger-collector --collector.queue-size=100000 --collector.num-workers=100 --grpc-storage-plugin.binary=/data/jaeger-clickhouse-linux-amd64 --grpc-storage-plugin.configuration-file=/data/config.yaml --grpc-storage-plugin.log-level=debug`
