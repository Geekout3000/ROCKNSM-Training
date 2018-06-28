### Install Elasticsearch
    database of files which allow you to search; think splunk
    Do not give elasticsearch more than 31GB of RAM. If you have a lot of RAM use a hyper-visor
ssh into sensor
sudo yum install elasticsearch
sudo vi /etc/elasticsearch/elasticsearch.yml
        line 17, uncomment cluster.name: and change to perched
        line 23, uncomment node.name: and change to sg10
        line 33, uncomment and repalce with /data/elasticsearch/data
        line 43, uncomment
        line 55, uncomment and change host to 172.16.100.100
        line 59, uncomment
        save: wq
sudo mkdir -p /data/elasticsearch/data
sudo chown -R elasticsearch:elasticsearch /data/elasticsearch/
sudo mkdir /etc/systemd/system/elasticsearch.service.d
sudo vi /etc/systemd/system/elasticsearch.service.d/override.conf (add the following)
[Service]
LimitMEMLOCK=infinity
        save: wq
sudo vi /etc/elasticsearch/jvm.options
        line 22 and 23
        change both to 16g
        save: wq
sudo firewall-cmd --add-port=9200/tcp --permanent
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
sudo systemctl status elasticsearch


### Pre-steps for last Validation

Change the permissions for etc/elasticsearch/
cd /etc
sudo chmod 755 /etc/elasticsearch



Run:  /opt/fsf/fsf-client/fsf_client.py --full meta.properties

### Validation
sudo -s
cat /var/log/elasticsearch/perched.log  (whatever the cluster was called)
      should see a bunch of logs
curl 172.16.100.100:9200
    should show an output that shows you are connected

End of task, move to step 12 installing kibana

Additional information about your cluster
curl 172.16.100.100:9200/_cat (list all cat API information)
      for example: curl 172.16.100.100:9200/_cat/nodes
