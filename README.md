# Docker Compose Elastic Search and Kibana

## Introduction

This repo contains a basic compose file to launch a single node docker cluster, running Elasticsearch and Kibana in their respective containers.
This is not a secure set of services; Requests to ES are not encrypted, communications between ES and Kibana are not encrypted either. This is intended for local development use only.

## References

Elasticsearch docker-compose.yml used as reference.

https://github.com/elastic/elasticsearch/blob/8.10/docs/reference/setup/install/docker/docker-compose.yml

Elasticsearch setup with Docker instructions.

https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

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

Both services will be availible at http://localhost:**_PORT_**
