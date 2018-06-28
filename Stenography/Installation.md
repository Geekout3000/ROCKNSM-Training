## Installing Stenographer
    1. ssh into sensor
    2. sudo yum install Stenographer
    3. cd /etc/stenographer
    4. listing
    5. sudo vi config (make changes to the below option)
        * PacketsDirectory should be changed to "/data/stenographer/packets"
        * IndexDirectory changed to "/data/stenographer/index"
        * Interface changed "enp2s0" (stenographer listens on this int)
        * Port changed to "8086"
        * Host changed to "172.16.100.100"
        * wq
    6. sudo stenokeys.sh stenographer stenographer  (creating keys for the group and user stenographer)
        * should get back generating key/cert for client, server, and ca
    7. sudo mkdir -p /data/stenographer/{packets,index}
    8. sudo chown -R stenographer:stenographer /data/stenographer/
    9. $ `sudo systemctl start stenographer`
    10. $ `sudo systemctl status stenographer`
### Stenographer Validation
    1. $ `cd /data/stenographer/packets`
            * list out and you should see a random string of numbers with stenographer stenographer as the user and group

### TS

  * sudo systemctl status stenographer
      look for any error messages
  * journalctl -xe
      look for any error messages
