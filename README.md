# Docker-compose for EFK 
- Elasticsearch
- Fluent Bit
- Fluentd
- Kibana

A basic docker compose file that will set up Elasticsearch, Fluent Bit, and Kibana.

The following docker compose allows to ingest data through Forward protocol or Syslog in UDP mode, examples:

### Send data through Forward protocol

```json
echo "{\"key\": 1234}" | fluent-cat test
```

### Send data through Syslog UDP socket

```bash
logger -d -n 127.0.0.1 --port 5140 "hello"
```

### 使用方式

#### Elsaticserach
- 加大預設 mmapfs 避免 es 啟動錯誤 (https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html)
```bash
$ sudo sysctl -w vm.max_map_count=262144
```

- 產生 es certs
```bash
$ docker-compose -f create-certs.yml run --rm create_certs

Archive:  /certs/bundle.zip
   creating: /certs/ca/
  inflating: /certs/ca/ca.crt        
   creating: /certs/es01/
  inflating: /certs/es01/es01.crt    
  inflating: /certs/es01/es01.key    
   creating: /certs/es02/
  inflating: /certs/es02/es02.crt    
  inflating: /certs/es02/es02.key    
   creating: /certs/es03/
  inflating: /certs/es03/es03.crt    
  inflating: /certs/es03/es03.key    
   creating: /certs/kib01/
  inflating: /certs/kib01/kib01.crt  
  inflating: /certs/kib01/kib01.key  
```

- 啟動 docker-compose
```bash
$ docker-compose up -d
```

- 設定 elasticsearch password
```bash
$ docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords \
auto --batch --url https://es01:9200"
Changed password for user apm_system
PASSWORD apm_system = qOufxoBPtaWAASO2BSbT

Changed password for user kibana_system
PASSWORD kibana_system = Yhsqh1YcalpO8eTQF3UZ

Changed password for user kibana
PASSWORD kibana = Yhsqh1YcalpO8eTQF3UZ

Changed password for user logstash_system
PASSWORD logstash_system = Bpl23sHOLweBvzKhqOrB

Changed password for user beats_system
PASSWORD beats_system = Z4KiVwbat7i2JvNree0t

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = UlRa7vORpv5IRf544Veq

Changed password for user elastic
PASSWORD elastic = ISZIF78ohvyCZhdgv1M5
```

- 取得上一步驟 `kibana_system` password 修改 docker-compose kib01
```yaml
      ELASTICSEARCH_PASSWORD: t9O0n51j2sO1mVwMtD2M
```

- 重新啟動 docker-compose
```bash
$ docker-compose stop
$ docker-compose up -d
```

- 使用 `elastic` password 登入 kibana (https://localhost:5601)

## Reference
- https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html
