/////chrome installation
https://www.cyberciti.biz/faq/howto-install-google-chrome-on-redhat-rhel-fedora-centos-linux/

//////////

http://quickstart.cloudera:80

////////////

*******Root************
hdfs dfs -mkdir /test1/
*******Desktop command*******
gedit ss4.txt
hdfs dfs -put ss4.txt /test1/
hdfs dfs -ls /test1
hdfs dfs -cat /test1/ss4.txt
hdfs dfs -get /test1/ss4.txt ss5.txt
cd /usr/lib/hadoop-mapreduce
yarn jar hadoop-mapreduce-examples.jar wordcount /test1/ss4.txt /test2/


///////////////////////


javac -cp `hadoop classpath` WordCount.java

javac -cp `hadoop classpath` -d wordcount_classes WordCount.java

sudo tar -xvf <path to .gz>

hdfs dfsadmin -safemode leave

//////////////////////////

**inside wordcount_classes

  javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-mapreduce/* WordCount.java
/////contains library and template classes
  jar -cvf wordcount.jar *.class

  sudo cp wordcount.jar /usr/lib/hadoop-mapreduce/

hdfs dfs -mkdir /test6
gedit vv.txt
hdfs dfs -put vv.txt /test6/


  hadoop jar wordcount.jar WordCount /test6/vv.txt /test7

hdfs dfs -ls /test7/

hdfs dfs -get /test7/part-r-00000

**
NOTE
::::test6-----war-and-peace-input
::::test7-----war-and-peace-output
///////////////


*******hadoop streaming********

chmod u+x program_name

echo "foo foo quux labs foo bar quux" | ./mapper.py

echo "foo foo quux labs foo bar quux" | ./mapper.py|sort -k1,1

echo "foo foo quux labs foo bar quux" | ./mapper.py|sort -k1,1|./reducer.py


hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -file ./mapper.py -mapper ./mapper.py -file ./reducer.py -reducer ./reducer.py -input war-and-peace-input/war-and-peace.txt -output war-and-peace-output


hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -file ./mapper.py -mapper ./mapper.py -file ./reducer.py -reducer ./reducer.py -input /test6/vv.txt -output /test8


# *********Apache Pig**************

cp /etc/passwd .

hdfs dfs -put passwd passwd

hdfs dfs -ls passwd


### /////Local Pig

** $ pig -x local
** grunt> A = load 'passwd' using PigStorage(':');
** grunt> B = foreach A generate $0 as id;
** grunt> dump B;
** grunt> quit;
**

### ////To use Hadoop MapReduce, 
$ pig -x mapreduce
$ pig -x tez
////


/////
/* id.pig */

A = load 'passwd' using PigStorage(':');
B = foreach A generate $0 as id;
dump B;
store B into 'id.out';

/*end id.pig*/
/////

**remove local id.out if exists
$ /bin/rm -r id.out/
$ pig -x local id.pig



$ hdfs dfs -rm -r id.out
$ pig id.pig



## **Apache Pig example**


### /**** To use Hadoop MapReduce ****/

#### /////

$ cd /$Hadoop_Home/bin/
$ hdfs dfs -mkdir hdfs://localhost:9000/Pig_Data


**In the local file system, create an input file student_data.txt containing data as shown below
**
////////////////////////////////////

001,Rajiv,Reddy,9848022337,Hyderabad
002,siddarth,Battacharya,9848022338,Kolkata
003,Rajesh,Khanna,9848022339,Delhi
004,Preethi,Agarwal,9848022330,Pune
005,Trupthi,Mohanthy,9848022336,Bhuwaneshwar
006,Archana,Mishra,9848022335,Chennai.

////////////////////////////////////

**Now, move the file from the local file system to HDFS using put command as shown below

/*

$ cd $HADOOP_HOME/bin 
$ hdfs dfs -put /home/Hadoop/Pig/Pig_Data/student_data.txt dfs://localhost:9000/pig_data/

*/

**You can use the cat command to verify whether the file has been moved into the HDFS, as shown below.

/*

$ cd $HADOOP_HOME/bin
$ hdfs dfs -cat hdfs://localhost:9000/pig_data/student_data.txt

*/


#### /////

$ pig -x mapreduce

grunt> student = LOAD 'hdfs://localhost:9000/pig_data/student_data.txt' USING PigStorage(',') as (id:int, firstname:chararray, lastname:chararray,phone:chararray, city:chararray);




































































