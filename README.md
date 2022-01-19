# kafka-debezium-example
Example for a dev setup for Kafka and Debezium. Inspired by [tutorial of Debezium](https://github.com/debezium/debezium-examples/tree/main/tutorial).

The docker-compose file defines a single node kafka cluster along with connect. Additionally [akhq](https://akhq.io) is added, a kafka ui to more easily what data is in your local kafka instance.


## Registering connector
### SQLServer
To register a connector for the sample sqlserver instance you can use the rest inferface of Kafka-Connect.


To get started easily, I've added an example connector.
```curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver-local.json```

## AKHQ Gui
To access the gui, go to the following url to open AKHQ: http://localhost:8080 . In AKQH you can see topics along with all the data in a more accessable way then using the console consumers.

## Docker volumes and persistency


For Kafka and Zookeeper volumes are used, so your kafka data is persisted, even if you shutdown and restart your containers. To see what volumes are created run:
```docker volume ls```

Optionally delete volumes, in case you want to start all over using command below:
```docker volume rm kafka-debezium-docker_zk-txns kafka-debezium-docker_zk-logs kafka-debezium-docker_kafka-logs kafka-debezium-docker_zk-data  kafka-debezium-docker_kafka-data```

See ```docker volume --help`` for more information on how to handle volumes.