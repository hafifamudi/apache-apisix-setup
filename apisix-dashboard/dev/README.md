---
title: Deploy Apache APISIX Dashboard with Docker
---

## Manual deploy apisix dashboard via docker

- Linux command version

```
docker pull apache/apisix-dashboard
docker run -d --name dashboard \
           -p 9000:9000        \
           -v `pwd`/config/conf.yml:/usr/local/apisix-dashboard/conf/conf.yaml \
           apache/apisix-dashboard
```

- Windows command version

1.  open powershell then execute this

```
$pwd = Convert-Path (Get-Location)
$configPath = "$pwd\config\conf.yaml"
$configPath = $configPath -replace '\\', '/'
```

2.  then install the dashboard

```
docker pull apache/apisix-dashboard
docker run -d --name dashboard `
           -p 9000:9000        `
           -v ${configPath}:/usr/local/apisix-dashboard/conf/conf.yaml `
           apache/apisix-dashboard
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
