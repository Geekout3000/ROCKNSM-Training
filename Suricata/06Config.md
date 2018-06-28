### Installing Suricata

1. ssh into sensor 'ssh sg10' Password123!@#
2. c
3. sudo -s (temporarily makes us root)
4. cd /etc/suricata
5. ls rules/
6. $ `curl -L -O http://perched-repo/sensor_configs/suricata/emerging.rules.tar.gz`
    (once downloaded do a long listing and see the emerging.rules.tar.gz)
7. tar -zxf emerging.rules.tar.gz
8. ls rules/ (all emerging rules should be there)
9. Fix permission issue to still be owned by suricata
      * chown -R suricata:root rules/
      * cd rules
      * ll | grep .rules | wc -l
           should be 59
10. cd /etc/suricata
          * vi suricata.yaml
          * underneath vars line 15  (set nu)
          * HOMENET: change to 172.16.100.0/24,192.168.2.0/24,10.0.100.0/24
          * line 122: change default-log-dir" to /data/suricata/logs
          * line 126: stats changed enabled to no
          * line 135: fasts change enabled to no
          * line 142: extensible event format change enable to yes
          * line 256: unified to alert set enable to no
          * http log line 298, set to no
          * tls, line 308 set to no
          * dns, line 326 set to no
          * stats.log, line 400 change to no
          * line 547 af-packet change the interface to enp2s0
          * wq
12. sudo mkdir -p /data/suricata/logs
13. sudo chown -R suricata:suricata /data/suricata
14. sudo vi /etc/sysconfig/suricata
        * change options to "--af-packet=enp2s0"
          * wq
15. while in /etc/suricata
        #### script to fix rules
                * curl -L -O http://perched-repo/sensor_configs/suricata/add-rules.sh
        #### make it executable
                * chmod +x add-rules.sh
                * ./add-rules.sh
                * run long listing and suricata.yaml.orig should be there
16. sudo vi /etc/logrotate.d/suricata.conf and paste the below script
/data/suricata/*.log /data/suricata/*.json
{
  rotate 3
  missingok
  nocompress
  create
  sharedscripts
  postrotate
      /bin/kill -HUP $(cat /var/run/suricata.pid)
  endscript
}
          save: wq

  17. sudo systemctl start suricata
  18. sudo systemctl status suricata
            ignore rules that the status gives you: Look for the 8 packet processing as measure of success.
  19. sudo systemctl enable suricata

  20. Validation of successful suricata Install
          cd /data/suricata/logs/
          'll'
          you should see the eve.json file.

  Note the add-rules should be scp over to local box. But this is a one time thing,./add-rules is already on box local box and it's in ATOM under addrules.sh, if we need to recopy.

  Task complete, see step 7: 07Config.md (Bro)
