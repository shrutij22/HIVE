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

# Create 'emp' table in Hive
$HIVE_HOME/bin/hive -e "
CREATE TABLE emp (empname STRING, empid INT, \`emp sal\` INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
"

# Create 'dept' table in Hive
$HIVE_HOME/bin/hive -e "
CREATE TABLE dept (dept_name STRING, dept_id INT, empid INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS TEXTFILE;
"

# Load data into 'emp' table
$HIVE_HOME/bin/hive -e "
LOAD DATA LOCAL INPATH '/desktop/emptable.txt' INTO TABLE emp;
"

# Load data into 'dept' table
$HIVE_HOME/bin/hive -e "
LOAD DATA LOCAL INPATH '/desktop/depttable.txt' INTO TABLE dept;
"

# Run the SQL query to join tables and filter results
$HIVE_HOME/bin/hive -e "
SELECT e.empname, e.\`emp sal\`, d.dept_name
FROM emp e
JOIN dept d ON e.empid = d.empid
WHERE e.\`emp sal\` >= 200000;
"
