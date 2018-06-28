## Making an exact copy of traffic from Pfsense to Switch/Sensor
 ### option 1 to create a span
1. In pfsense GUI go to interfaces and click opt2
    * enable the interface and save
2. In the pfsense GUI, click on Interface > Interface Assignments > Bridges > add
      * hightlight both the WAN and LAN
      * click display advance
      * under span port select opt2
      * save it

Task Complete; go back to step03 for shutdown procedures
