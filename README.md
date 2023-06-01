# Hadoop_Installation_Ubantu

Follow these steps:
Use JDK version 1.8 OR Below Version 
use this command---sudo apt install openjdk-8-jdk
if you have jdk then update the version:
::sudo update-alternatives --config java
and select 1.8 version.

2)sudo addgroup hadoop
3)sudo adduser --ingroup hadoop hduser
4)sudo adduser hduser sudo

//To install ssh server
5)sudo apt-get install openssh-server
//Check ssh install properly or not
6)which ssh 
7)which sshd
8)su hduser
9)cd
10)ssh-keygen -t rsa -P "" (If asked for a filename just leave it blank and press the enter key to continue)

11)cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys 	

//To check if ssh works, type: 
12)ssh localhost
13)exit

Install Hadoop:
14)tar -xvf hadoop-2.9.0.tar.gz (For extraction of the downloaded hadoop file)

15)Move the extracted file to /usr/local/hadoop:
To move file open Another terminal and go to the directory of hadoop file using cd command
16)sudo mv Path of hadoop-2.9.2 file /usr/local/hadoop
To find Path go to properties of Hadoop file and paste it in above command

//Change the owner of hadoop folder: 
17)sudo chown -R hduser /usr/local

18)sudo gedit ~/.bashrc
//One file will open paste following all commands at the end of the file
#HADOOP VARIABLES START
export JAVA_HOME= "/usr/lib/jvm/java-8-openjdk-amd64"
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
#HADOOP VARIABLES END

19)source ~/.bashrc

//We need to set JAVA_HOME by modifying hadoop-env.sh file.
20)sudo gedit /usr/local/hadoop/etc/hadoop/hadoop-env.sh 
//(Add the following line in the above file:) Note: Replace the jdk version with the one in the system
21)export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

22)sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml
//Enter the following in between the <configuration></configuration> tag:
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>

23)cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml

24)sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml
//Enter the following content in between the <configuration></configuration> tag:
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>

//Run following command one by one;important step
25)sudo mkdir -p /usr/local/hadoop_tmp/hdfs/namenode

26)sudo mkdir -p /usr/local/hadoop_tmp/hdfs/datanode

27)sudo chown -R hduser:hadoop /usr/local/hadoop_tmp

29)sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml
//Enter the following content in between the <configuration></configuration> tag:

<property>
<name>dfs.replication</name>
<value>1</value>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>file:/usr/local/hadoop_tmp/hdfs/namenode</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>file:/usr/local/hadoop_tmp/hdfs/datanode</value>
</property>

30)sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml
//Paste the code given below in <configuration> tags:

<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
<property>
<name>yarn.nodemanager.aux-
services.mapreduce.shuffle.class</name>
<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>

//Format the New Hadoop Filesystem:

31)hadoop namenode -format

32)start-all.sh
  
33)jps
  
Done....Thank You
