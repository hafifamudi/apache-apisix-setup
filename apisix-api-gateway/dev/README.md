---
title: Deploy Apache APISIX Api Gateway with Docker
---

## Manual deploy apisix api gateway via docker

### Create a network `apisix`

```
docker network create \
  --driver=bridge \
  --subnet=172.18.0.0/16 \
  --ip-range=172.18.5.0/24 \
  --gateway=172.18.5.254 \
  apisix
```

### Run etcd server with `apisix` network

Specify ip `172.18.5.10`

```
docker run -it --name etcd-server \
-v `pwd`/config/etcd/config.yml:/opt/bitnami/etcd/conf/etcd.conf.yml \
-p 2379:2379 \
-p 2380:2380  \
--network apisix \
--ip 172.18.5.10 \
--env ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd:3.4.9
```

### Run Apache APISIX server

You need etcd docker to work with Apache APISIX. You can refer to [the docker-compose example](example.md).

Or you can run APISIX with Docker directly（Docker name is api-gateway）:

Check or Modify etcd address to `http: //172.18.5.10:2379` in `pwd` / example / apisix_conf / config.yaml: /usr/local/apisix/conf/config.yaml

```
docker run --name api-gateway \
 -v `pwd`/example/apisix_conf/config.yaml:/usr/local/apisix/conf/config.yaml \
 -v `pwd`/example/apisix_log:/usr/local/apisix/logs  \
 -p 9080:9080 \
 -p 9091:9091  \
 -p 9443:9443 \
 --network apisix \
 --ip 172.18.5.11 \
 -d apache/apisix
```

### Have a test

Test with admin api

e.g. Get route list, should be return

```
curl http://127.0.0.1:9080/apisix/admin/routes/
...
{"node":{"createdIndex":4,"modifiedIndex":4,"key":"\/apisix\/routes","dir":true},"action":"get"}
```

### Clean

```
docker rm test-api-gateway
docker rm etcd-server
docker network rm apisix
```

### Automate deploy with docker compose

- run

```
docker compose up -d
```

- clean

```
docker compose down
```
