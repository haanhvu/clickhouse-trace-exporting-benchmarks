Hardware: [c3.small.x86](https://deploy.equinix.com/product/servers/c3-small/) x 3

Docker command: `docker run -e OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=http://147.28.183.171:4317 jaegertracing/jaeger-tracegen -spans 10 -traces 15000000 -services 2 -trace-exporter otlp-grpc`

Running this Docker command on 3 hardwares, we will generate 495 million spans. Each trace will have 11 spans (1 parent span and 10 child spans), each child span will have 11 attributes, the atttributes will have 97 distinct keys and 1000 distinct values.
