Creating a Docker network (kafka-net) to enable communication between containers.
```bash
docker network create kafka-net
```

Lenses Box (or fast-data-dev) (image used in this lab) is a containerized development environment for working with Kafka and its ecosystem of tools.

It provides everything you need to quickly set up and start experimenting with Kafka, without having to manually configure and install various components.

You can access the Lenses_IO Dashboard on top right of the page once the Kafka Cluster (container) is up and running.

Start a Kafka Cluster (container) using the below Docker info:

Network:
--network kafka-net
(Attach the container to the existing Docker network kafka-net)

Ports:
2182:2181 - Zookeeper (default port for Zookeeper communication)

3030:3030 - Web UI or Management Interface (likely for Kafka monitoring)

9093:9092 - Kafka Broker (exposing Kafka's service to the outside)

8081:8081 - Kafka REST Proxy (used for RESTful communication with Kafka)

8082:8082 - Kafka REST Proxy (another service, possibly for web management or monitoring)

Environment Variable:
-e ADV_HOST=kafka-cluster
(Advertise the hostname of the Kafka cluster as kafka-cluster)

Container Name:
--name kafka-cluster
(Name the container kafka-cluster)

Image:
lensesio/fast-data-dev
(The Docker image that sets up a Kafka development environment)

Creating the Kafka cluster container:
```bash
docker run --rm -d \
  --network kafka-net \
  -p 2182:2181 \
  -p 3030:3030 \
  -p 9093:9092 \
  -p 8081:8081 \
  -p 8082:8082 \
  -e ADV_HOST=kafka-cluster \
  --name kafka-cluster \
  lensesio/fast-data-dev
```



We'll launch another container to run Kafka UI. You can explore it to get a better understanding of how it works.

Start Kafka UI by running Docker container using the below info:

Network:
--network kafka-net
(Attach the container to the kafka-net Docker network)

Ports:
7000:8080 - Expose port 8080 of the container (likely the web interface for Kafka UI) to port 7000 on the host.

Environment Variable:
-e DYNAMIC_CONFIG_ENABLED=true
(Enables dynamic configuration within the Kafka UI, allowing for changes to be made on the fly)

Container Name:
--name kafka-ui
(Name the container kafka-ui)

Image:
provectuslabs/kafka-ui
(The image for running Kafka UI, a web-based interface for managing Kafka clusters)

```bash
docker run --rm -d \
  --network kafka-net \
  -p 7000:8080 \
  -e DYNAMIC_CONFIG_ENABLED=true \
  --name kafka-ui \
  provectuslabs/kafka-ui
```
