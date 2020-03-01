##### /////chrome installation
https://www.cyberciti.biz/faq/howto-install-google-chrome-on-redhat-rhel-fedora-centos-linux/

//////////

> Cloudera home
> http://quickstart.cloudera:80

////////////

## Hadoop mapreduce demo

**Root**
```
hdfs dfs -mkdir /test1/
```
**Desktop command**
```
gedit ss4.txt
hdfs dfs -put ss4.txt /test1/
hdfs dfs -ls /test1
hdfs dfs -cat /test1/ss4.txt
hdfs dfs -get /test1/ss4.txt ss5.txt
cd /usr/lib/hadoop-mapreduce
yarn jar hadoop-mapreduce-examples.jar wordcount /test1/ss4.txt /test2/
```

///////////////////////

### Wordcount demo
```
javac -cp `hadoop classpath` WordCount.java
or
javac -cp `hadoop classpath` -d wordcount_classes WordCount.java
```

:pushpin: hdfs dfsadmin -safemode leave


//////////////////////////

**inside wordcount_classes**
```
  javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-mapreduce/* WordCount.java
  // contains library and template classes
  jar -cvf wordcount.jar *.class

  sudo cp wordcount.jar /usr/lib/hadoop-mapreduce/

hdfs dfs -mkdir /test6
gedit vv.txt
hdfs dfs -put vv.txt /test6/

hadoop jar wordcount.jar WordCount /test6/vv.txt /test7


  hdfs dfs -ls /test7/

  hdfs dfs -get /test7/part-r-00000
```

*NOTE*
::::test6-----war-and-peace-input
::::test7-----war-and-peace-output


## Hadoop Streaming
```
chmod u+x program_name

echo "foo foo quux labs foo bar quux" | ./mapper.py

echo "foo foo quux labs foo bar quux" | ./mapper.py|sort -k1,1

echo "foo foo quux labs foo bar quux" | ./mapper.py|sort -k1,1|./reducer.py


hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -file ./mapper.py -mapper ./mapper.py -file ./reducer.py -reducer ./reducer.py -input war-and-peace-input/war-and-peace.txt -output war-and-peace-output

hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -file ./mapper.py -mapper ./mapper.py -file ./reducer.py -reducer ./reducer.py -input /test6/vv.txt -output /test8
```

## Apache Pig

```
$ cp /etc/passwd .

$ hdfs dfs -put passwd passwd

$ hdfs dfs -ls passwd
```

#### Local Pig
```
$ pig -x local
grunt> A = load 'passwd' using PigStorage(':');
grunt> B = foreach A generate $0 as id;
grunt> dump B;
grunt> quit;
```

#### To use Hadoop MapReduce, 
```
$ pig -x mapreduce
or
$ pig -x tez
```

__*id.pig*__
```
A = load 'passwd' using PigStorage(':');
B = foreach A generate $0 as id;
dump B;
store B into 'id.out';
```
__*end id.pig*__


*remove local id.out if exists*
```
$ /bin/rm -r id.out/
$ pig -x local id.pig
```

```
$ hdfs dfs -rm -r id.out
$ pig id.pig
```


## Apache Pig example

####  To use Hadoop MapReduce [not working block]
<!-- ![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1") -->
:warning:
:x:
>
  > ```
  > $ cd /$Hadoop_Home/bin/
  > $ hdfs dfs -mkdir hdfs://localhost:9000/Pig_Data
  > ```
  > 
  > **
  > In the local file system, create an input file student_data.txt containing data as shown below
  > **
  > ////////////////////////////////////
  > > Student data
  > ```
  > 001,Rajiv,Reddy,9848022337,Hyderabad
  > 002,siddarth,Battacharya,9848022338,Kolkata
  > 003,Rajesh,Khanna,9848022339,Delhi
  > 004,Preethi,Agarwal,9848022330,Pune
  > 005,Trupthi,Mohanthy,9848022336,Bhuwaneshwar
  > 006,Archana,Mishra,9848022335,Chennai.
  > ```
  > ////////////////////////////////////
  > 
  > **Now, move the file from the local file system to HDFS using put command as shown below
  > 
  > ```
  > $ cd $HADOOP_HOME/bin 
  > $ hdfs dfs -put /home/Hadoop/Pig/Pig_Data/student_data.txt dfs://localhost:9000/pig_data/
  > ```
  > 
  > **You can use the cat command to verify whether the file has been moved into the HDFS, as shown below.
  > 
  > ```
  > $ cd $HADOOP_HOME/bin
  > $ hdfs dfs -cat hdfs://localhost:9000/pig_data/student_data.txt
  > ```
  > ```
  > $ pig -x mapreduce
  > ```
  > ```
  > grunt> student = LOAD 'hdfs://localhost:9000/pig_data/student_data.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray,phone:chararray, city:chararray);
  > 
  > grunt> student = LOAD '/Pig_Data/student_data.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray,phone:chararray, city:chararray);
  > ```



## easy way

```
$ mkdir pig_data
$ gedit student_data.txt
```

> *for hdfs*
```
$ hdfs dfs -mkdir /Pig_Data/
$ hdfs dfs -put student_data.txt /Pig_Data/
$ pig -x mapreduce 
```

> *for local*
```
$ pig -x local
```
###### hdfs
```
grunt> student = LOAD '/Pig_Data/student_data.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray,phone:chararray, city:chararray);

grunt> dump student;

grunt> store student into 'student_data_out';
```

###### local
```
grunt> student = LOAD 'student_data.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray,phone:chararray, city:chararray);

grunt> dump student;

grunt> store student into 'student_data_out';
```

## Apache Pig - Join operator
```
$ cd pig_data

$ gedit customers.txt
```
> customers.txt
```
1,Ramesh,32,Ahmedabad,2000.00
2,Khilan,25,Delhi,1500.00
3,kaushik,23,Kota,2000.00
4,Chaitali,25,Mumbai,6500.00 
5,Hardik,27,Bhopal,8500.00
6,Komal,22,MP,4500.00
7,Muffy,24,Indore,10000.00
```

```
$ gedit orders.txt
```
> orders.txt
```
102,2009-10-08 00:00:00,3,3000
100,2009-10-08 00:00:00,3,1500
101,2009-11-20 00:00:00,2,1560
103,2008-05-20 00:00:00,4,2060
```

> Load data
```
$ pig -x local

grunt> customers = LOAD 'customers.txt' USING PigStorage(',') as (id:int, name:chararray, age:int, address:chararray, salary:int);
  
grunt> orders = LOAD 'orders.txt' USING PigStorage(',') as (oid:int, date:chararray, customer_id:int, amount:int);
```

>Self Join
```
grunt> customers1 = LOAD 'customers.txt' USING PigStorage(',') as (id:int, name:chararray, age:int, address:chararray, salary:int);
  
grunt> customers2 = LOAD 'customers.txt' USING PigStorage(',') as (id:int, name:chararray, age:int, address:chararray, salary:int); 

grunt> customers3 = JOIN customers1 BY id, customers2 BY id;

grunt> dump customers3;

grunt> store customers3 into 'cust_self_joined';
```


>Inner Join
```
grunt> customer_orders = JOIN customers BY id, orders BY customer_id;

grunt> store customer_orders into 'cust_inner_joined';
```

>Left outer Join
```
grunt> outer_left = JOIN customers BY id LEFT OUTER, orders BY customer_id;

grunt> store outer_left into 'left_outer_joined';
```

## Apache Pig - Group operator
```
$ cd pig_data
$ gedit student_details.txt

```
> student_details.txt
```
001,Rajiv,Reddy,21,9848022337,Hyderabad
002,siddarth,Battacharya,22,9848022338,Kolkata
003,Rajesh,Khanna,22,9848022339,Delhi
004,Preethi,Agarwal,21,9848022330,Pune
005,Trupthi,Mohanthy,23,9848022336,Bhuwaneshwar
006,Archana,Mishra,23,9848022335,Chennai
007,Komal,Nayak,24,9848022334,trivendram
008,Bharathi,Nambiayar,24,9848022333,Chennai
```

> Load
```
$ pig -x local

grunt> student_details = LOAD 'student_details.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray, age:int, phone:chararray, city:chararray);
```

> Group by age
```
grunt> group_data = GROUP student_details by age;

grunt> store group_data into 'group_by_age';

```
## Apache Hive
```
cd Desktop
```
```
hive> CREATE TABLE pokes (foo INT, bar STRING);

hive> CREATE TABLE logs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ';

hive> LOAD DATA LOCAL INPATH 'student/student.txt' OVERWRITE INTO TABLE logs;

hive> SELECT t4 AS sev, COUNT(*) AS cnt FROM logs WHERE t4 LIKE '[%' GROUP BY t4;

```





# Apache Hadoop Fundamentals

LESSON 6.4 APACHE SQOOP 

OS: Linux
Platform: RHEL 6.3
Hadoop Version: 2.4
Hadoop Version: Hortonworks HDP 2.1
Sqoop Version: 1.4.4

Reference:

> http://sqoop.apache.org/docs/1.4.5/SqoopUserGuide.html
 

### Step 1: Download and Load Sample MySQL Data
===========================================
#### Assume mysql is installed and working on the host
#### Ref: http://dev.mysql.com/doc/world-setup/en/index.html
#### Get the database:

> wget http://downloads.mysql.com/docs/world_innodb.sql.gz

#### Load into MySQL
```
mysql -u root -p
mysql> CREATE DATABASE world;
mysql> USE world;
mysql> SOURCE world_innodb.sql;
mysql> SHOW TABLES;
```
```
   +-----------------+
   | Tables_in_world |
   +-----------------+
   | City            |
   | Country         |
   | CountryLanguage |
   +-----------------+
   3 rows in set (0.01 sec)
```
#### To see table details:
```
mysql> SHOW CREATE TABLE Country;
mysql> SHOW CREATE TABLE City;
mysql> SHOW CREATE TABLE CountryLanguage;
```

### Step 2: Add Sqoop User Permissions for Local Machine and Cluster
================================================================
```
mysql> GRANT ALL PRIVILEGES ON world.* To 'sqoop'@'localhost' IDENTIFIED BY 'sqoop';
mysql> GRANT ALL PRIVILEGES ON world.* To 'sqoop'@'10.0.%.%' IDENTIFIED BY 'sqoop';
mysql> quit
```
#### Login as sqoop to test 
```
mysql -u sqoop -p
mysql> USE world;
mysql> SHOW TABLES;
```
```
   +-----------------+
   | Tables_in_world |
   +-----------------+
   | City            |
   | Country         |
   | CountryLanguage |
   +-----------------+
   3 rows in set (0.01 sec)
```
```
mysql> quit
```

### Step 3: Import Data Using Sqoop
===============================

#### Use Sqoop to List Databases
```
sqoop list-databases --connect jdbc:mysql://localhost/world --username sqoop --password sqoop
```

  > information_schema
  > test
  > world

#### List Tables
```
sqoop list-tables --connect jdbc:mysql://localhost/world --username sqoop --password sqoop
```

#### Make directory for data
```
hdfs dfs -mkdir sqoop-mysql-import
```
#### Do the import
#### -m is number of map tasks
```
sqoop import --connect jdbc:mysql://localhost/world  --username sqoop --password sqoop --table Country  -m 1 --target-dir /user/hdfs/sqoop-mysql-import/country

hdfs dfs -ls /user/hdfs/sqoop-mysql-import/country

hdfs dfs  -cat /user/hdfs/sqoop-mysql-import/country/part-m-00000
```
### Using and Options File
>   Can use and options file to avoid rewriting same options
####   Example (vi world-options.txt):
```
import
--connect
jdbc:mysql://localhost/world
--username
sqoop
--password
sqoop
```
```
sqoop --options-file world-options.txt --table City -m 1 --target-dir /user/hdfs/sqoop-mysql-import/city
```
> Include a SQL Query in the Import Step
> If we want just cities inf Canada
> The $Conditions variable is needed for more than one mapper.
> If you want to import the results of a query in parallel, then each map task will need
> to execute a copy of the query, with results partitioned by bounding conditions inferred 
> by Sqoop. Your query must include the token $CONDITIONS which each Sqoop process will 
> replace with a unique condition expression based on the "--split-by" option.  
> You may need to select another splitting column with --split-by option if your
> primary key is not uniformly distributed.

#### First use a single mapper "-m 1"
```
sqoop  --options-file world-options.txt -m 1 --target-dir /user/hdfs/sqoop-mysql-import/canada-city --query "SELECT ID,Name from City WHERE CountryCode='CAN' AND \$CONDITIONS"

hdfs dfs  -cat /user/hdfs/sqoop-mysql-import/canada-city/part-m-00000
```

> Since -m 1 is one map, we don't need to specify a --split-by option.
> Now use multiple mappers, clear resutls from previous import. 
```
hdfs dfs -rm -r -skipTrash /user/hdfs/sqoop-mysql-import/canada-city

sqoop --options-file world-options.txt -m 4 --target-dir /user/hdfs/sqoop-mysql-import/canada-city --query "SELECT ID,Name from City WHERE CountryCode='CAN' AND \$CONDITIONS" --split-by ID

hdfs dfs -ls /user/hdfs/sqoop-mysql-import/canada-city
```
> Found 5 items






























