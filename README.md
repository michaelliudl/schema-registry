schema-registry
===============
Schema registry for Kafka

Quickstart
----------

1. Start ZooKeeper from the standard Kafka install
./bin/zookeeper-server-start.sh config/zookeeper.properties

2. Start Kafka from the standard Kafka install
./bin/kafka-server-start.sh config/server.properties

3. Start the REST server by running io.confluent.kafka.schemaregistry.rest.Main
mvn package
bin/schema-registry-start config/schema-registry.properties

4. Register a schema
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X POST -i http://localhost:8081/subjects/Kafka-key/versions -d '{"schema": "{\"type\": \"string\"}"}'

curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X POST -i http://localhost:8081/subjects/Kafka-value/versions -d '{"schema": "{\"type\": \"string\"}"}'

5. Test compatibility of a schema with the latest schema under a subject without changing the state of the registry
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X POST -i http://localhost:8081/compatibility/subjects/Kafka-value/versions/latest -d '{"schema": "{\"type\": \"string\"}"}'

6. List all subjects 
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X GET http://localhost:8081/subjects

7. List all versions of a subject's schema
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X GET http://localhost:8081/subjects/Kafka-value/versions

8. Get a particular version of a subject's schema
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X GET http://localhost:8081/subjects/Kafka-value/versions/1

9. Get top level config
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X GET http://localhost:8081/config

10. Update compatibility level globally
curl -v -H "Content-Type: application/vnd.schemaregistry.v1+json" -X PUT -i http://localhost:8081/config -d '{"compatibility":"NONE"}'

