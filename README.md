# bigdata-ecosystem

## Create global network

```
docker network create mynetwork
```

## Start hadoop system

```
cd docker-hadoop
docker-compose up -d
```

## Start Hbase

```
cd docker-hbase
docker-compose up -d
```

## Start kafka ecosystem, mysql 

```
export DEBEZIUM_VERSION=0.6
cd cdc-kafka-hadoop/cdc/debezium
docker-compose -f docker-compose-mysql.yaml up -d
```


## Start cdc mysql to kafka

```
cd cdc-kafka-hadoop/cdc/debezium
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mysql.json
```


## Get data from kafka

```
docker exec -it schema-registry /usr/bin/kafka-avro-console-consumer \
    --bootstrap-server kafka:9092 \
    --from-beginning \
    --property print.key=true \
    --property schema.registry.url=http://schema-registry:8081 \
    --topic dbserver1.inventory.customers

```

## Sink kafka to hbase

```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8084/connectors/ -d @register-hbase-sink-json-debe.json
```

## Access to hbase

```
docker exec -it hbase-master bash
hbase shell
describe 'hbase-products3'
scan 'hbase-products3'
get 'hbase-products3','1001'
get 'hbase-products3','1001','dbserver1.inventory.customers:first_name'
```
