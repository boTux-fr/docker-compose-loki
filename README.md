Loki docker-compose
===================

[Loki](https://grafana.com/docs/loki/latest/overview/) log aggregation system with docker-compose.

Whith this project you can test out loki with : 

* Cassandra as index db
* Minio as chunks db
* Promtail
* Prometheus and cadvisor
* Traefik reverse-proxy for ssl

We recommand to use [helm chart](https://grafana.com/docs/loki/latest/installation/helm/) or check out the [doc](https://grafana.com/docs/loki/latest/installation/) for production use. This repository will kickstart a loki instance to check out capabilities and configurations tweaks.

## Running

Copy `.env.dist` to `.env` and `/config/loki-config.yaml.dist` to `/config/loki-config.yaml` and edit the two files.

You need to set minio access_key and secret in `/config/loki-config.yaml`.

To get a fully running stack, run [docker-service-traefik](https://gitlab.com/botux-fr/docker/docker-service-traefik/-/tree/upgrade/2.2) with this docker-compose.

1. Copy configurations files
    ```bash
    # Copy env.dist
    cp .env.dist .env && cp config/loki-config.yaml.dist config/loki-config.yaml
    # Edit .env with your editor
    vim .env config/loki-config.yaml
    ```
1. Check configuration
    ```bash
    docker-compose -f docker-compose.yml -f compose/cassandra.yml -f compose/minio.yml -f compose/prometheus.yaml -f compose/traefik.yml config
    ```
1. Run the stack
    ```bash
    docker-compose -f docker-compose.yml -f compose/cassandra.yml -f compose/minio.yml -f compose/prometheus.yaml -f compose/traefik.yml up -d
    ```
1. Access grafana
   1. Add sources : 
      * loki : https://$LOKI_DOMAIN with header `X-Scope-OrgI: loki-internal`.
      * prometheus: http://prometheus:9090
   1. Go to explorer and have fun !

## Author

Romain Fluttaz