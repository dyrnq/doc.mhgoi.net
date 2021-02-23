


## cockroachdb
CockroachDB兼容PostgreSQL协议

```bash
mkdir -p /data/lib/cockroach/data;
mkdir -p /data/lib/cockroach/certs;

## 生成ca证书和节点证书
docker run -it \
--rm \
--name cockroach \
--hostname cockroach \
--network mynet \
-v /data/lib/cockroach/data:/cockroach/cockroach-data \
-v /data/lib/cockroach/certs:/opt/certs \
--entrypoint="" \
cockroachdb/cockroach:v20.2.4 bash

# 參考https://www.cockroachlabs.com/docs/v20.2/cockroach-cert
cockroach cert create-ca --certs-dir=/opt/certs --ca-key=/opt/certs/ca.key --allow-ca-key-reuse --lifetime=878400h0m0s --overwrite
cockroach cert create-node --certs-dir=/opt/certs --ca-key=/opt/certs/ca.key --lifetime=439200h0m0s --overwrite cockroach localhost 127.0.0.1 192.168.9.119 192.168.9.106
cockroach cert create-client --certs-dir=/opt/certs --ca-key=/opt/certs/ca.key --lifetime=439200h0m0s --overwrite root

```
```bash
docker run -d \
--restart always \
--name cockroach \
--hostname cockroach \
--network mynet \
-p 18080:8080 \
-p 26257:26257 \
-v /data/lib/cockroach/data:/cockroach/cockroach-data \
-v /data/lib/cockroach/certs:/opt/certs \
cockroachdb/cockroach:v20.2.4 start-single-node --listen-addr=:26257 --store=path=/cockroach/cockroach-data --certs-dir=/opt/certs --accept-sql-without-tls --cluster-name mhgoi;

```

```bash
docker logs cockroach;


*
* INFO: Replication was disabled for this cluster.
* When/if adding nodes in the future, update zone configurations to increase the replication factor.
*
CockroachDB node starting at 2021-02-05 01:59:06.161204434 +0000 UTC (took 0.5s)
build:               CCL v20.2.4 @ 2021/01/21 00:08:24 (go1.13.14)
webui:               https://cockroach:8080
sql:                 postgresql://root@cockroach:26257?sslcert=%2Fopt%2Fcerts%2Fclient.root.crt&sslkey=%2Fopt%2Fcerts%2Fclient.root.key&sslmode=verify-full&sslrootcert=%2Fopt%2Fcerts%2Fca.crt
RPC client flags:    /cockroach/cockroach <client cmd> --host=cockroach:26257 --certs-dir=/opt/certs
logs:                /cockroach/cockroach-data/logs
temp dir:            /cockroach/cockroach-data/cockroach-temp909037312
external I/O path:   /cockroach/cockroach-data/extern
store[0]:            path=/cockroach/cockroach-data
storage engine:      pebble
status:              restarted pre-existing node
cluster name:        mhgoi
clusterID:           38e64d7c-64a2-4093-a1bd-fd41ca780ebd
nodeID:              1
```



```bash
docker exec -it cockroach bash
cockroach sql --host=cockroach:26257 --certs-dir=/opt/certs
CREATE DATABASE mhgoi_blog;
CREATE USER jack WITH PASSWORD '666666';
GRANT ALL ON DATABASE mhgoi_blog TO jack;
GRANT ALL ON TABLE mhgoi_blog.public.* TO jack;
```

