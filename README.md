# kafka-debezium-example
Example for a dev setup for Kafka and Debezium. Inspired by [tutorial of Debezium](https://github.com/debezium/debezium-examples/tree/main/tutorial).

The docker-compose file defines a single node kafka cluster along with connect. Additionally [akhq](https://akhq.io) is added, a kafka ui to more easily what data is in your local kafka instance.

## Prerequises
To run the example, docker and docker-compose have to be installed. When using Linux, you could use the docker as packaged for your distribution. On MS-Windows and Mac, you could use [Docker Desktop](https://www.docker.com/products/docker-desktop), which is [free for personal use](https://www.docker.com/products/personal).

## Starting
To run the example install docker and docker-compose.

## Registering connector
### SQLServer
To register a connector for the sample sqlserver instance you can use the rest inferface of Kafka-Connect.


To get started easily, I've added an example connector.  
```curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver-local.json```

The example database does not contain any tables besides default system table, and sqlserver is not configured for CDC yet. I plan to to add this in the example later, for now it's left as an [excersise for the reader](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server).

To test to your own database, you could start by copying and update the above json file.

After registering, you might want to see the follow the log-output of kafka connect to see if and what's happening:  
```docker-compose logs -f --tail 100 connect```

After adding a connector, Debezium should start to a snapshot scan of the complete database. To limit the number of tables, you could update the whitelist or excludelist in the connector configuration. Note also that Debezium will do a snapshot scan of *all* tables that you included, but it will only able to track changes for tables for which use have enabled [cdc]((https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server)).

#### Remarks
* Minor remark, the example Kafka Connectors use a Java version in which one or more encryption codes are disabled. Undoubtly for very good reason, but this may result in Debezium not be able to connect to your old sql-server instance. A quick work around is to use the older version of Debezium, by replacing 1.8 with 1.6 in the .env file. But of course, use this tip your own risk and certainly don't do that for any sensitive information.
* As mentioned, CDC has to be enabled. For SQLServer, you'll have to [CDC per table](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server?view=sql-server-ver15#enable-for-a-table).

## AKHQ Gui
To access the gui, go to the following url to open AKHQ: http://localhost:8080 . In AKQH you can see topics along with all the data in a more accessable way then using the console consumers.

## Docker volumes and persistency


For Kafka and Zookeeper volumes are used, so your kafka data is persisted, even if you shutdown and restart your containers. To see what volumes are created run:  
```docker volume ls```

Optionally delete volumes, in case you want to start all over using command below:  
```docker volume rm kafka-debezium-docker_zk-txns kafka-debezium-docker_zk-logs kafka-debezium-docker_kafka-logs kafka-debezium-docker_zk-data  kafka-debezium-docker_kafka-data```

See ```docker volume --help``` for more information on how to handle volumes.