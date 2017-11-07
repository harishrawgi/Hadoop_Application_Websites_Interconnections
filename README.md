### NP_Project
A repository containing our term project for the Network Programming Course of Fall semester 2017, NIT Raipur.

This project has been made by a collaborative teamwork of the following students-
1. Anshul Verma(14115011)
1. Hari Shrawgi(14115035)
1. Harshdeep Kaur(14115037)
1. Prerana Agrawal(14115071)
1. Shriya Thawait(14115083)

## Import Project into Eclipse 

If you import the project in Eclipse, it may first show a lot of errors. It is recommended to use Eclipse Mars or later. Update the project using the following steps: 
1. Make sure that you can see the package view on the left-hand side of the Eclipse window. 
1. Right-click on the project (webFinder) in the package view. 
1. In the opening pop-up menu, left-click on Maven. 
1. In the opening sub-menu, left-click on Update Project. 
1. In the opening window make sure the project (webFinder) is selected. 
1. Make sure that Update project configuration from pom.xml is selected. 
1. You can also select Clean projects. 
1. Click OK. 
1. Now the structure of the project in the package view should slightly change, the project will be re-compiled, and the errors should disappear. 

## Build project in Eclipse 
Now we will build the project and generate a jar file that can be passed to Hadoop. Follow the steps given below to build the project.

1. Make sure that you can see the ```package view``` on the left-hand side of the eclipse window.
1. Right-click on the project (webFinder) in the ```package view```.
1. In the opening pop-up menu, choose ```Run As```.
1. In the opening sub-menu choose ```Run Configurations```.
1. In the opening window, choose ```Maven Build```.
1. In the new window ```Run Configurations / Create, manage, and run configurations``` , choose ``` Maven Build```.
1. Click  New launch configuration. 
1. Write a name for this configuration in the Name field. You can use this configuration again later. 
1. In the tab Main enter the Base directory of the project, this is the folder called hadoop/webFinder containing the Eclipse/Maven project.
1. Under Goals, enter clean compile package. This will build a jar archive.
1. Click Apply. 
1. Click Run. 
1. The build will start; you will see its status output in the console window. 
1. The folder target will contain a file webFinder-full.jar after the build. This is the executable archive with our application. 

## Installation instruction for Haddop (for Linux and Mac)

1. Install prerequisites by running sudo apt-get install ssh rsync.
2.	Go into a base folder where you want to install Hadoop. Let's call this folder X.
3.	Download Hadoop from one of the mirrors provided. We have chosen http://www-eu.apache.org/dist/hadoop/common/ and from there hadoop-2.7.4 from where we have download hadoop-2.7.4.tar.gz into X.
4.	Once the file has been fully downloaded, extract the file.
5.	A new folder named X/hadoop-2.7.4 should have appeared. If you chose a different Hadoop version, replace 2.7.4.accordingly in the following steps.
6.	In order to run Hadoop, JAVA_HOME should be set correctly. Open the file X/etc/hadoop/hadoop-env.sh. Find the line export JAVA_HOME=${JAVA_HOME} and replace it with export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which javac)))).
Now to test if everything is working correctly, 
7.	In the terminal, enter X/hadoop-2.7.4/ and execute the command bin/hadoop. It should display some help and command line options.
8.	In your terminal enter:
```
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.4.jar grep input output 'dfs[a-z.]+'
cat output/*
```
This third command should produce a lot of logging output and the last one should say something like 1 dfsadmin.

## Setup for Single-Computer Pseudo-Distributed Execution
 
9.	Enter the directory X/hadoop-2.7.4/etc in order to create the basic configuration.

10.	Open the file core-site.xml in the text editor. Remove everything in the file and store the following text, then save and close the file.
```
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
<name>fs.defaultFS</name>
<value>hdfs://localhost:9000</value>
</property>
</configuration>
```
11.	Open the file hdfs-site.xml in the text editor. Remove everything in the file and store the following text, then save and close the file.
```
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
</configuration>
```
In order to run Hadoop in a pseudo-distributed fashion, we need to enable passwordless SSH connections to the local host.

12.	In the terminal, execute ssh localhost to test if you can open a secure shell connection to your current, local computer without needing a password.

13.	It may say something like:
```
ssh: connect to host localhost port 22: Connection refused
```
If it does say this, then do
```
sudo apt-get install ssh
```
Now you've got SSH installed. Do ssh localhost again.

14.	It may ask you something like
```
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:HZUVFF77GAh5cF/sg8YhjRf1gSGJ9ui5ksdf2GAl5Ha.
Are you sure you want to continue connecting (yes/no)? 
```
Just type yes and hit enter (it may then say something like Warning:``` Permanently added 'localhost' (ECDSA) to the list of known hosts.). ```

15.	If it asks you something like ```xyz@localhost's password:```, hit Ctrl-C and do the things below. Otherwise, you can directly skip to the next point. So, If you were asked for a password, enter the following into your terminal:
```
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```
16.	After completing the above commands, you should test the result by again executing ssh localhost. You will now no longer be asked for a password and directly receive a welcome message. Via a ssh connection, you can, basically, open a terminal to and run commands on a remote computer (which, in this case, is your own, current computer). You can return to the normal (non-ssh) terminal by entering exit and pressing return, 
We now want to test whether our installation and setup works correctly by further following the steps given-

17.	Format the HDFS file system by entering bin/hdfs namenode -format followed by return.
18.	Start the NameNode and DataNode daemons by running sbin/start-dfs.sh. You may get some logging output messages, which would be followed by something like
```
The authenticity of host '0.0.0.0 (0.0.0.0)' can't be established.
ECDSA key fingerprint is SHA256:HZUVFF77GAh5cF/sg8YhjRf1gSGJ9ui5ksdf2GAl5Ha.
Are you sure you want to continue connecting (yes/no)? 
```
which you would answer with yes followed by enter. If, after that, you get a message like ```0.0.0.0: packet_write_wait: Connection to 127.0.0.1: Broken pipe, enter sbin/stop-dfs.sh```, hit return, and do ```sbin/start-dfs.sh``` again.

19.	In your web browser, open http://localhost:50070/. It should display a web page giving an overview about the Hadoop system now running on your local computer.
20.	Now we can setup the required stuff for making HDFS directories and copying the input files. ```Replace <userName>``` with your user/login name on your current machine.
```
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/<userName>
bin/hdfs dfs -put etc/hadoop input
```

21.	We can now run the job via ```bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar grep input output 'dfs[a-z.]+'```

22.	We obtain the output of the job via
```
bin/hdfs dfs -get output output
cat output/*
```

23.	Finally, we need to shutdown Hadoop by running ```sbin/stop-dfs.sh```.

## Installation instruction for Hadoop(for Windows)

Download Hadoop from one of the mirrors provided. We have chosen http://www-eu.apache.org/dist/hadoop/common/ and from there hadoop-2.7.4 from where we have download hadoop-2.7.4.tar.gz.

Extract the hadoop folder in a suitable location.

Make the following changes-

1. Go to  ```etc->hadoop``` folder. 

2. Open ```hadoop-env.cmd``` file and add the following commands to the end of this file-
```
set HADOOP_IDENT_STRING=%USERNAME%  
set HADOOP_PREFIX=e:\hadoop-2.7.4  
set HADOOP_CONF_DIR=%HADOOP_PREFIX%\etc\hadoop  
set YARN_CONF_DIR=%HADOOP_CONF_DIR%  
set PATH=%PATH%;%HADOOP_PREFIX%\bin
```

You can replace ```e:\hadoop-2.4.7``` with the path to the hadoop folder that you have extracted in your computer.

3. Open ```yarn-site.xml``` and add the following commands inside the configuration tags(```<configuration> Insert code here </configuration>```).
```
<property>  
	<name>yarn.nodemanager.disk-health-checker.min-healthy-disks</name>   
	<value>0.0</value>
</property>
<property> 
	<name>yarn.nodemanager.disk-health-checker.max-disk-utilization-          per-diskpercentage</name> 
	<value>100.0</value>
</property>
<property>      
	<name>yarn.nodemanager.resource.memory-mb</name>   
	<value>1024</value>   
	<description>Amount of physical memory, in MB, that can be allocated for containers.</description> 
</property> 
<property>   
	<name>yarn.nodemanager.resource.cpu-vcores</name>   
	<value>1</value>   
	<description>Amount of physical memory, in MB, that can be allocated for containers.</description> 
</property> 
<property>   
	<name>yarn.scheduler.minimum-allocation-mb</name>   
	<value>1024</value>   
</property> 
<property>   
	<name>yarn.scheduler.minimum-allocation-vcore</name>   
	<value>1</value> 
</property>  
<property>
	<name>yarn.nodemanager.disk-health-checker.max-disk-utilization-per-diskpercentage</name>    
	<value>100</value> 
</property> 
<property>    
	<name>yarn.application.classpath</name>        
	<value>%HADOOP_CONF_DIR%,%HADOOP_COMMON_HOME%/share/hadoop /common/*,%HADOOP_COMMON_HOME%/share/hadoop/common/lib/*,%HADO 				OP_HDFS_HOME%/share/hadoop/hdfs/*,%HADOOP_HDFS_HOME%/share/hadoo p/hdfs/lib/*,%HADOOP_MAPRED_HOME%/share/hadoop/mapreduce/*,%HADOO P_MAPRED_HOME%/share/hadoop/mapreduce/lib/*,%HADOOP_YARN_HOME% /share/hadoop/yarn/*,%HADOOP_YARN_HOME%/share/hadoop/yarn/lib/*</value></property>
```

4. Open ```mapred-site.xml``` and add the following commands inside the configuration tags(```<configuration> Insert code here </configuration>```).  
```
<property>  
<name>mapreduce.application.classpath</name>   
<value>/hadoop-2.7.4/share/hadoop/mapreduce/*,  /hadoop-2.7.4/share/hadoop/mapreduce/lib/*,  /hadoop-2.7.4/share/hadoop/common/*,  /hadoop-2.7.4/share/hadoop/common/lib/*,  /hadoop-2.7.4/share/hadoop/yarn/*,  /hadoop-2.7.4/share/hadoop/yarn/lib/*,  /hadoop-2.7.4/share/hadoop/hdfs/*,  /hadoop-2.7.4/share/hadoop/hdfs/lib/*,  </value> 
</property>
```

5. Open   ```hdfs-site.xml``` and add the following commands inside the configuration tags(```<configuration> Insert code here </configuration>```).  
```
<property>       
<name>dfs.replication</name>       
<value>1</value> 
</property>
```
6. Open ```core-site.xml``` and add the following commands inside the configuration tags(```<configuration> Insert code here </configuration>```).
```
<property>  
<name>fs.defaultFS</name>  
<value>hdfs://localhost:9000</value> 
</property> 
```
## Instructions for Running the Compiled Project

Open command prompt and execute the following commands one by one.  

In the first command, replace ```E:\hadoop-2.7.4\etc\hadoop``` with the path to the folder where you have extracted hadoop in your computer.  

In the sixth command, replace ```C:\Users\hp-15\Desktop\distributedComputingExamplesmaster\hadoop\webFinder``` with path to the folder where project is saved.

In the eighth command, replace ```C:\Users\hp15\Desktop\distributedComputingExamples-master\hadoop\webFinder\target\webFinderfull.jar``` with the path for the jar file where project is saved.
```
cd E:\hadoop-2.7.4\etc\hadoop  
hadoop-env.cmd  
%HADOOP_PREFIX%\bin\hdfs namenode –format  
%HADOOP_PREFIX%\sbin\start-dfs.cmd  
%HADOOP_PREFIX%/bin/hdfs dfs -mkdir /input  
%HADOOP_PREFiX%/bin/hdfs dfs -put C:\Users\hp15\Desktop\distributedComputingExamples-master\hadoop\webFinder\input /input  
%HADOOP_PREFIX%\bin\hdfs dfs -ls /input  
%HADOOP_PREFIX%/bin/hadoop jar C:\Users\hp15\Desktop\distributedComputingExamples-master\hadoop\webFinder\target\webFinderfull.jar /input/input /output  
%HADOOP_PREFIX%/bin/hdfs dfs -cat /output/part-r-00000 
```
