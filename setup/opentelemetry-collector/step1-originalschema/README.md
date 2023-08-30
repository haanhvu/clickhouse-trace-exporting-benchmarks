Hardware: [m3.small.x86](https://deploy.equinix.com/product/servers/m3-small/)

Docker command: `docker run --network host --name otel -p 147.28.187.86:4317:4317 -p 147.28.187.86:8888:8888 -v $(pwd)/config.yaml:/etc/otelcol-contrib/config.yaml otel/opentelemetry-collector-contrib`
