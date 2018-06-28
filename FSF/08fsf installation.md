###FSF

ssh into sensor: sg10/Password123!@#
sudo yum install fsf
sudo vi /opt/fsf/fsf-server/conf/config.py
    *  LOG_PATH: '/data/fsf/logs'
    *  YARA_PATH: '/var/lib/yara-rules/rules.yara'
    *  PID_PATH: '/run/fsf/fsf.pid'
    *  EXPORT_PATH: '/data/fsf/archive'
    *  ACTIVE_LOGGING_MODULES: ['rockout']
    *  IP_Address: "172.16.100.100"
    *  save: wq
* sudo mkdir -p /data/fsf/{logs,archive}
* sudo chown -R fsf:fsf /data/fsf
* sudo systemctl start fsf
* sudo systemctl status fsf
* ss -lnt (should be listening on 5800 on 172.16.100.100)
#### open port
* sudo firewall-cmd --add-port=5800/tcp --permanent
* sudo firewall-cmd --reload
* sudo firewall-cmd --list-ports (to confirm)
### setting the client (point to our sensor)
sudo vi /opt/fsf/fsf-client/conf/config.py
    * IP Address: '172.16.100.100'
    * save: wq
####Validation of both server and client
Create traffic for bro to log by running curl command:
    * sudo curl -L -O http://repo/markdown-cheatsheet-online.pdf
Run command:
    * /opt/fsf/fsf-client/fsf_client.py --full /data/bro/logs/extract_files/ <TAB OUT>
    * Should output a pdf file (if curled a pdf file prior) to extract_files folder

#### starting fsf
sudo systemctl restart fsf

End of task, move to step 9: 09kafka installation.md
