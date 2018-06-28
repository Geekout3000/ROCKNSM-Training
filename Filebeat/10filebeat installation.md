### Installing filebeat (sends pcap and eve.json to kafka)
**** note, you will need to know the file locations of both suricata and fsf **** /data/suricata/logs and /data/fsf/logs

ssh into sensor
* sudo yum install filebeat
* sudo vi /etc/filebeat/filebeat.yml
    * : set num
    * down to line 17, highlight, escape 48 dd, or d48d (this one yanks)
    * add the following under filebeat.prospectors: (line 15)
    see filebeat.yml


  *** If the above does not work, see the file located in Documents labeled filebeat.yml on the user box. And copy the content under: Filebeat prospectors, Processors, and Elasticsearch output ***

  * Now run the filebeat
      * sudo systemctl start filebeat
      * sudo systemctl status filebeat
      * sudo system enable filebeat








### Validation
        cd /data/kafka/logs
            * ls
          you will see fsf.raw and suricata.raw

End of tasks, move to step 11 Elasticsearch

###Troubleshooting:

Additional details:  journalctl -xe

FSF:  /opt/fsf/fsf-client/fsf_client.py --full meta.properties

Only do the below if you run filebeat without first changing the partitions to 2 in the server.properties.
  * /opt/kafka/bin/kafka-topic.sh --alter --topic suricata-raw --partitions 2 --zookeeper 172.16.100.100:2181

  * /opt/kafka/bin/kafka-topic.sh --alter --topic fsf-raw --partitions 2 --zookeeper 172.16.100.100:2181
