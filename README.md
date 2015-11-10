## Stream Time Series Data into Hbase with Apache Spark


### Workshop Details

Open the ./doc/index.html page in your browser


### Run the Application

Create an hbase table to write to:
launch the hbase shell
$hbase shell

create '/user/user01/sensor', {NAME=>'data'}, {NAME=>'alert'}, {NAME=>'stats'}

Commands to run labs:

Step 1: First compile the project on eclipse: Select project  -> Run As -> Maven Install

Step 2: use scp to copy the sparkstreaminglab-1.0.jar to the mapr sandbox or cluster

scp  sparkstreaminglab-1.0.jar user01@ipaddress:/user/user01/.
if you are using virtualbox:
scp -P 2222 sparkstreaminglab-1.0.jar user01@127.0.0.1:/user/user01/.

To run the  streaming:

Step 3: start the streaming app

spark-submit --driver-class-path `hbase classpath` --class solutions.HBaseSensorStream sparkstreaminglab-1.0.jar

Step 4: copy the streaming data file to the stream directory
cp sensordata.csv  /user/user01/stream/.

Step 5: you can scan the data written to the table, however the values in binary double are not readable from the shell
launch the hbase shell,  scan the data column family and the alert column family 
$hbase shell
scan '/user/user01/sensor',  {COLUMNS=>['data'],  LIMIT => 10}
scan '/user/user01/sensor',  {COLUMNS=>['alert'],  LIMIT => 10 }

Step 6: launch one of the programs below to read data and calculatecalculate stats for one column
/opt/mapr/spark/spark-<version>/bin/spark-submit --driver-class-path `hbase classpath` --class solutions.HBaseReadWrite sparkstreaminglab-1.0.jar
calculate stats for whole row
spark-submit --driver-class-path `hbase classpath` --class solutions.HBaseReadRowWriteStats sparkstreaminglab-1.0.jar

launch the shell and scan for statistics
scan '/user/user01/sensor',  {COLUMNS=>['stats']}


