#!/bin/bash

# Start Hadoop services
$HADOOP_HOME/sbin/start-all.sh

# Check Hadoop processes
jps

# Set up Hive environment
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin

# Initialize Hive schema
$HIVE_HOME/bin/schematool -dbType derby -initSchema

# Start Hive
$HIVE_HOME/bin/hive

# Sample table creation
$HIVE_HOME/bin/hive -e "
CREATE TABLE emp (empname STRING, empid INT, \`emp sal\` INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
"
