## Building Bro
ssh into sensor
* sudo yum install bro broctl bro-plugin-kafka bro-plugin-af_packet
* cd /etc/bro
* ll
* sudo vi broctl.cfg
      * Line 67, LogDir = /data/bro/logs
      * Add a line to bottom of file:
          * lb_custom.InterfacePrefix=af_packet::
      * save: wq
* sudo vi networks.cfg
   * Change to reflect the below ranges
      * 10.0.0.0/16
      * 172.16.0.0/16
      * 192.168.2.0/24
   * save: wq
* sudo vi node.cfg (sets up bro based on hardware)
      * comment out the below lines, so that it looks like this:
      * line 8: #[bro]
      * line 9 #[type]
      * line 10 #[host]
      * line 11 #[interface]
    * line 20-31, REMOVE the # from each line to uncomment
    * line 20, under [manager] add:
       * pin_cpus=1 (should be line 23)
    * line 32 (it previously was line 31), change interface=enp2s0 and uncomment line
         * starting at line 33 add the following:
            * lb_method=custom (line 33)
            * lb_procs=2 (line 34)
            * pin_cpus=2,3   (line 35)
            * env_vars=fanout_id=73  (line 36)
    * save: wq
* cd /usr/share/bro/site/
* sudo mkdir scripts
* cd scripts/
* sudo vi kafka.bro (copy the bro script below in blank file)

@load Apache/Kafka/logs-to-kafka

redef Kafka::topic_name = "bro-raw";
redef Kafka::json_timestamps = JSON::TS_ISO8601;
redef Kafka::tag_json = T;
redef Kafka::kafka_conf = table (
    ["metadata.broker.list"] = "172.16.100.100:9092"
);

event bro_init() &priority=-5
{
    for (stream_id in Log::active_streams)
    {
        if (|Kafka::logs_to_send| == 0 || stream_id in Kafka::logs_to_send)
        {
            local filter: Log::Filter = [
                $name = fmt("kafka-%s", stream_id),
                $writer = Log::WRITER_KAFKAWRITER,
                $config = table(["stream_id"] = fmt("%s", stream_id))
            ];

            Log::add_filter(stream_id, filter);
        }
    }
}

    * save: wq

* sudo vi /usr/share/bro/site/scripts/afpacket.bro (this creates afpacket.bro with the below content) add:

redef AF_Packet::fanout_id = strcmp(getenv("fanout_id"),"") == 0 ? 0 : to_count(getenv("fanout_id"));

  *  save: wq
* So now in the /usr/share/bro/site/scripts you should have these files:
        *  afpacket.bro
        *  kafka.bro
* Add the rock-scripts.tar.gz to the same location (/usr/share/bro/site/scripts)
      This can be found in the home/user local box.
          * 'cd ~' in Documents folder on local box
          * scp rock-scripts.tar.gz sg10:~
          * mv rock-scripts.tar.gz /usr/share/bro/site/scripts
* cd /usr/share/bro/site/scripts (you should not see 3 files)
* Unzip the tar.gz file
  * sudo tar -zxf rock-scripts.tar.gz (this will create a new directory called rock-scripts)
* Now you should see 4 files in /usr/share/bro/site/scripts
        * afpacket.bro
        * kafka.bro
        * rock-scripts.tar.gz
        * rock-scripts      
* sudo vi /usr/share/bro/site/local.bro
        * Shift G (get to the bottom of the file)
        * insert the 3 lines:
@load ./scripts/afpacket
@load ./scripts/kafka
@load ./scripts/rock-scripts/rock
      * save: wq
* sudo mkdir -p /data/bro/logs
* sudo vi /usr/share/bro/site/scripts/rock-scripts/plugins/afpacket.bro
    * commentout   #@load scripts/rock/plugins/afpacket
    * :wq
* sudo broctl deploy
    * should be successful
### Validate
    * run the 'sudo broctl status' (all 4 should be running)
    * run 'sudo broctl check'

### Test Validation:
  * cd /data/bro/logs/current/
  * ls
        * should see conn.log

End of task; move to step 8: FSF folder
### Troubleshooting and Making Changes to Bro
  * Adding something new to bro you will need to:

  * sudo broctl stop
  * sudo broctl cleanup all
  * sudo broctl deploy
