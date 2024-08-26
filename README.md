# Docker Compose Elastic Search and Kibana

## Introduction

This repo contains a basic compose file to launch a single node docker cluster, running Elasticsearch and Kibana in their respective containers.
This is not a secure set of services; Requests to ES are not encrypted, communications between ES and Kibana are not encrypted either. This is intended for local development use only.

## References

Elasticsearch docker-compose.yml used as reference.

https://github.com/elastic/elasticsearch/blob/8.10/docs/reference/setup/install/docker/docker-compose.yml

Elasticsearch setup with Docker instructions.

https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

Step-by-step:

```unix
$ docker network create elastic

$ docker pull docker.elastic.co/elasticsearch/elasticsearch:8.15.0

$ docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.15.0

$ docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic

$ docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

$ export ELASTIC_PASSWORD="your_password"

$ source .bashrc

$ docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .

$ curl --cacert http_ca.crt -u elastic:$ELASTIC_PASSWORD https://localhost:9200

$ docker pull docker.elastic.co/kibana/kibana:8.15.0

$ docker run --name kib01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.15.0

$ docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana

$ docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic

$ docker network rm elastic

$ docker rm es01
$ docker rm es02

$ docker rm kib01
```

## Usage

-   Make a copy of the .env.example, named .env.
-   Uncomment ENV variables, supply values for the empty variables
-   Edit the default values as needed.

Start container services as daemon

```bash

docker compose -f compose-local.yml up -d
```

Stop services and remove containers

```bash

docker compose -f compose-local.yml down -v
```

Both services will be availible at http://localhost:<em>PORT</em>
