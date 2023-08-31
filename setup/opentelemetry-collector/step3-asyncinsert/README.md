Hardware: [n3.xlarge.x86](https://deploy.equinix.com/product/servers/n3-xlarge/)

Please use the specific config file for each no batching, small batching, large batching benchmark.

To benchmark ClickHouse async insert, please use [this branch of the ClickHouse exporter](https://github.com/haanhvu/opentelemetry-collector-contrib/tree/newtypes-asyncinsert).

There's a detailed explanation in the [README of the branch](https://github.com/haanhvu/opentelemetry-collector-contrib/blob/newtypes-asyncinsert/exporter/clickhouseexporter/README.md).
