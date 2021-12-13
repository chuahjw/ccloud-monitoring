# Confluent Cloud monitoring flow (ccloud-monitoring)

## Background

Based on the following repositories: 
* https://github.com/Dabz/ccloudexporter
* https://github.com/confluentinc/jmx-monitoring-stacks
* https://github.com/lightbend/kafka-lag-exporter

Changes:
* Updated the environment to use the new Confluent Cloud metrics export object (in Early Access as of 13 December 2021, request access through the button in this link): https://api.telemetry.confluent.cloud/docs#tag/Version-2/paths/~1v2~1metrics~1{dataset}~1export/get
* Added consumer lag monitoring using `kafka-lag-exporter`

## How to run

1. **Metrics API**: Edit the file `prometheus.yml` to include the Confluent Cloud API Key and Secret, and the resource IDs (for each of the cluster, connectors, ksqlDB applications); note that this is the Cloud API Key, not the Cluster API Key (see: https://docs.confluent.io/cloud/current/monitoring/metrics-api.html#metrics-api-quick-start)
2. **Kafka Lag Exporter**: Edit the file `kafka-lag-exporter/application.conf` to include the `bootstrap-brokers` to the Confluent Cloud cluster, and the Cluster API Key and Secret under `sasl.jaas.config` for both `admin-client-properties` and `consumer-properties`; Kafka Lag Exporter uses an AdminClient to retrieve the offsets information from the cluster (see: https://github.com/lightbend/kafka-lag-exporter)
3. Start the environment using: `docker-compose up -d`
4. Navigate to `http://localhost:3000` (the Grafana Dashboard) in the browser, log in using `admin:admin`; for production environments, please harden accordingly (see: https://grafana.com/docs/grafana/latest/auth/grafana/)
5. View the dashboards: `Confluent Cloud` and `Kafka Lag Exporter`
6. Stop the environment using: `docker-compose down -v`