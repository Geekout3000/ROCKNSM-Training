## Kafka and Zookeeper Installation
ssh to sensor: sg10/PAssword123!@#
* sudo yum install zookeeper
* sudo yum install kafka
* cd /etc/kafka
* sudo vi server.properties
    * line 31, uncomment and add ip address listeners=PLAINTEXT://172.16.100.100:9092
    * line 36, uncomment the advertised.listerners=PLAINTEXT://172.16.100.100:9092
    * line 60, log.dirs=/data/kafka/logs
    * line 65, num.partitions=2
    * line 103, log.retention.hours=12
    * line 123, zookeeper.connect=172.16.100.100:2181
    * save: wq
* sudo mkdir -p /data/kafka/logs
* sudo chown -R kafka:kafka /data/kafka
* sudo firewall-cmd --add-port=9092/tcp --permanent
* sudo firewall-cmd --add-port=2181/tcp --permanent
* sudo firewall-cmd --reload

****Real world, not for training*****
2182 and 2183 (additional ports required to be allowed for cluster with zookeeper)

##zookeeper (manages kafka)
* cd /etc/zookeeper
* sudo systemctl restart zookeeper
* run a "ss -lnt" (to verify that * 2181 is there and also 172.16.100.100:5800)
* sudo systemctl start kafka

* cd /opt/kafka/bin/
*  'll' (list kafka shell scripts)
* Can skip down to 'start sending bro logs

Only need to run the below command to manually change the partition to 2. This is previously done in the sudo vi /etc/kafka/server.properties (line 103). Only run below if wasn't changed in server.properties
  * ./kafka-topics.sh --create --topic bro-raw --partitions 2 --replication-factor 1 --zookeeper 172.16.100.100:2181
  * ./kafka-topics.sh --describe --topic bro-raw --zookeeper 172.16.100.100:2181
        this will show that the partitions is set to 2

### Validation

### Start sending Bro logs
  * sudo broctl start
  * cd /data/kafka/logs/ (to view kafka logs)
  * ls (you should see bro-raw0 and 1)
### To view all information in the Bro Topic
  * /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server 172.16.100.100:9092 --topic bro-raw --from-beginning
        * This should flow with Bro logs as it's pushing data out

Task complete, move to step 10: 10filebeat installation
