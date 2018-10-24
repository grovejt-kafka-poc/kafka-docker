* landoop/kafka-lenses-dev

* landoop/fast-data-dev


wurstmeister/zookeeper
wurstmeister/kafka  
 

dockerkafka/zookeeper   
dockerkafka/kafka 
dockerkafka/kafka-manager

https://hub.docker.com/r/sheepkiller/kafka-manager/
sheepkiller/kafka-manager 
docker run -it --rm  -p 9000:9000 -e ZK_HOSTS="192.168.99.100:2181" -e APPLICATION_SECRET=letmein sheepkiller/kafka-manager


https://hub.docker.com/r/prom/prometheus/
prom/prometheus x

https://hub.docker.com/r/grafana/grafana/
grafana/grafana


https://hub.docker.com/r/thomsch98/kafdrop/
thomsch98/kafdrop
# docker run -e ZK_HOSTS=192.168.99.100:2181 -e LISTEN=9010 thomsch98/kafdrop
docker run -e ZK_HOSTS=192.168.99.100:2181 -e LISTEN=9010 --name kafdrop thomsch98/kafdrop

