Hardware for most benchmarks: [c3.small.x86](https://deploy.equinix.com/product/servers/c3-small/)

Hardware for async-insert benchmarks: [m3.small.x86](https://deploy.equinix.com/product/servers/m3-small/) (For some reason(s), using c3.small.x86 gave us back pressures at various points of the benchmarks).

Docker command: `docker run -it -p 147.75.85.195:18123:8123 -p 147.75.85.195:9000:9000 --name clickhouse --ulimit nofile=262144:262144 clickhouse/clickhouse-server`
