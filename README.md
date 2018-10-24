# kafka-docker
Person Event Topic: dst.hs.kafkapoc.personevent

----
## Install Oracle Virtualbox
  * https://www.virtualbox.org/

----  
## Install Docker Toolbox
* https://docs.docker.com/toolbox/toolbox_install_windows/
* Start the docker toolbox from the Docker Quickstart shortcut:
  * {path to git}\bin\bash.exe --login -i {path to DockerToolbox}\start.sh
    * -i makes the shell interactive
    * --login makes the shell act as if it had been directly invoked by login
  * For example: 
    * D:\Dev\tools\Git\bin\bash.exe --login -i D:\Dev\Tools\DockerToolbox\start.sh


----
## Landoop Lenses Docker Image for Zookeeper/Kafka Development Cluster
 * https://docs.lenses.io/dev/lenses-box/
* To get a license: https://www.landoop.com/downloads/lenses/
* landoop/kafka-lenses-dev
* Docker run: (from the docker toolbox command line started from the docker toolbox shortcut)
  * change to the lenses directory
  
        ```
        docker run -e ADV_HOST=192.168.99.100 -e LICENSE="$(cat license.json)" --rm --name lenses -p 3030:3030 -p 9092:9092 -p 2181:2181 -p 8081:8081 -p 9581:9581 -p 9582:9582 -p 9584:9584 -p 9585:9585 landoop/kafka-lenses-dev
        ```
 * Command Line tools for kafka:
    * docker exec -it lenses bash       
      * >  kafka-topics --zookeeper 192.168.99.100:2181 --list
  * docker run -e ADV_HOST=192.168.99.100 -e EULA="https://dl.lenses.stream/d/?id=028aae84-0e58-4bcf-9333-6c00d7025799" --rm --name lenses -p 3030:3030 -p 9092:9092 -p 2181:2181 -p 8081:8081 -p 9581:9581 -p 9582:9582 -p 9584:9584 -p 9585:9585 landoop/kafka-lenses-dev
    * Lenses and Kafka will be available in about 30-45 seconds at http://192.168.99.100:3030. 
    * The default credentials are admin / admin. 
    * To disable the sample data generators you can add -e SAMPLEDATA=0. 

----
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
    
----
## Yahoo Kafka Monitor
* Opensource kafka monitoring app
* https://github.com/yahoo/kafka-manager
* https://hub.docker.com/r/dockerkafka/kafka-manager/
* https://github.com/DockerKafka/kafka-manager-docker
* https://www.youtube.com/watch?v=JInLMIKLEOI (totorial - Kafka Manager - Multi Node Single Broker)
* docker run -d --name kafkadocker_zookeeper_1  dockerkafka/zookeeper
* docker run -d --name kafkadocker_kafka_1 --link kafkadocker_zookeeper_1:zookeeper dockerkafka/kafka
* docker run -it --rm --link kafkadocker_zookeeper_1:zookeeper --link kafkadocker_kafka_1:kafka -p 9000:9000 -e ZK_HOSTS=zookeeper:2181 dockerkafka/kafka-manager
docker run -it --rm  -p 9000:9000 -e ZK_HOSTS="192.168.99.100:2181" --name kafka-manager dockerkafka/kafka-manager

#docker run -it --rm --link kafkadocker_zookeeper_1:zookeeper --link kafkadocker_kafka_1:kafka -p 9000:9000 -e ZK_HOSTS=zookeeper:2181 dockerkafka/kafka-manager



----
## KafDrop
* Opensource spring boot app for monitoring kafka clusters
* https://github.com/HomeAdvisor/Kafdrop
* https://homeadvisor.tech/kafdrop-2-0-kafka-apis-search-more/



----
==For proxy problems
* https://stackoverflow.com/questions/31189123/docker-machine-behind-corporate-proxy/39734908#39734908
	As previously mentioned, you can edit the file at
	
	$HOME\.docker\machine\machines\default\config.json
	and set the HTTP_PROXY, HTTPS_PROXY and NO_PROXY variables (or delete them):
	
	 "HostOptions": {
	        "Driver": "",
	        ...
	        "EngineOptions": {
	           ...
	            "Env": [
	              "HTTP_PROXY=http://10.121.8.110:8080",
	              "HTTPS_PROXY=http://10.121.8.110:8080",
	              "NO_PROXY=localhost, 127.0.0.*, 192.168.*, *.dstcorp.net, *.docker.io, *.docker.com, dl.lenses.stream"
	            ],

If you have already the machine (VM) created, you can configure the proxy like this :

1- SSH into the docker dev host: docker-machine ssh dev
2- Add the following lines to /var/lib/boot2docker/profile (this file is read-only, use sudo)
    export HTTP_PROXY=http://<proxy>:<port>
    export HTTPS_PROXY=http://<proxy>:<port>
3- Exit the ssh session and restart the docker machine: docker-machine restart dev             