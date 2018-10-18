# kafka-docker
kafka-poc kafka-docker : Run kafka with docker.

----
## Install Oracle Virtualbox
  * https://www.virtualbox.org/

----  
## Install docker toolbox
* https://docs.docker.com/toolbox/toolbox_install_windows/
* Start the docker toolbox from the Docker Quickstart shortcut:
  * {path to git}\bin\bash.exe --login -i {path to DockerToolbox}\start.sh
    * -i makes the shell interactive
    * --login makes the shell act as if it had been directly invoked by login
  * For example: 
    * D:\Dev\tools\Git\bin\bash.exe --login -i D:\Dev\Tools\DockerToolbox\start.sh

----
## Landoop Docker Image for Zookeeper/Kafka Development Cluster
* See https://github.com/Landoop/fast-data-dev
* This docker image includes a user interface to view kafka and the kafka topics, very nice for getting started (http://192.168.99.100:3030)

To run the the Landoop zookeeper/kafka cluster 
* docker run --rm -it -p 2181:2181 -p3030:3030 -p 8081:8081  -p 8082:8082  -p 8083:8083  -p 9092:9092 -e ADV_HOST=192.168.99.100  landoop/fast-data-dev
	* To stop it use Ctrl-c
* This will give you zookeeper and one kafka node with a user interface at http://192.168.99.100:3030

* To run the kafka command line tools:
  * docker run --rm -it --net=host landoop/fast-data-dev bash
  * To create the topic for this poc (with 3 partitions:
    * kafka-topics --zookeeper 192.168.99.100:2181 --create --topic dst.hs.kafkapoc.personevent --partitions 3 --replication-factor 1
  * To delete the topic and start over:
    * kafka-topics --zookeeper 192.168.99.100:2181 --topic dst.hs.kafkapoc.personevent --delete
  
### General Notes
* Topic operations: list, create, describe, delete, 
  * kafka-topics --zookeeper 192.168.99.100:2181 --list
  * kafka-topics --zookeeper 192.168.99.100:2181 --create --topic first_topic --partitions 3 --replication-factor 1
  * kafka-topics --zookeeper 192.168.99.100:2181 --describe --topic first_topic
  * kafka-topics --zookeeper 192.168.99.100:2181 --topic second_topic --delete
  * Publishing data to a topic using the console producer
	* kafka-console-producer --broker-list 192.168.99.100:9092 --topic first_topic
  * Consuming data from a topic using the console consumer
	* kafka-console-consumer  --bootstrap-server 192.168.99.100:9092 --topic first_topic --from-beginning
	* kafka-console-consumer  --bootstrap-server 192.168.99.100:9092 --topic first_topic
* Kafka Topics UI
 * http://192.168.99.100:3030

----  
## Wurstmeister Docker image

* See 
  * https://github.com/wurstmeister/kafka-docker
  * http://wurstmeister.github.io/kafka-docker/
  * https://jaceklaskowski.gitbooks.io/apache-kafka/kafka-docker.html
  
* Start the docker toolbox from the Docker Quickstart shortcut.
  * Change to the wurstmeister directory
  * To start the cluster with one broker:
    * docker-compose up
    * To stop it use Ctrl-c
  * To start the cluster with n brokers (where n is the number of brokers to start)r:
    * docker-compose scale kafka=n
  * To start the kafka shell in another docker toolbox window run:  
    * ./start-kafka-shell.sh 192.168.99.100 192.168.99.100:2181
      * To list topics:
        * $KAFKA_HOME/bin/kafka-topics.sh --zookeeper $ZK --list
  	  * To create the topic for this poc (with 3 partitions:
         * $KAFKA_HOME/bin/kafka-topics.sh --zookeeper $ZK --create --topic dst.hs.kafkapoc.personevent --partitions 3 --replication-factor 1   
      * To start a producer: 
        *   $KAFKA_HOME/bin/kafka-console-producer.sh --topic=topic --broker-list=`broker-list.sh`
    
  * Note: the same command line for Landoop will also work here, e.g in another docker toolbox window run:
    * docker run --rm -it --net=host landoop/fast-data-dev bash
    


