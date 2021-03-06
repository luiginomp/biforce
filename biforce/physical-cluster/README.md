## 1. Find IP address of machine:                          

`ifconfig`

## 2. Disable firewall restrictions:

`sudo ufw disable`

#### UFW stands for Uncomplicated Firewall.
#### Other helpful commands:

`sudo ufw enable`
`sudo ufw status`

## 3. Add this IP address to hosts file:  

`sudo vim /etc/hosts`
	
#### Remember, `[ESC] :x!` to save and exit. To exit without saving, use `:q!`.

## 4. Restart SSH:

`sudo service ssh restart`
`sudo service ssh status`

#### (option to help)

#### May need to reinstall:

`sudo apt-get install openssh-server`

#### If getting 404 errors, may try:
        
`sudo apt-get clean`

#### and

`sudo apt-get update`

#### Then do:

`sudo apt-get install openssh-server`

## 5. Create SSH key:

`ssh-keygen -t rsa -P “”`
	
#### It does include two double-quotes at end.
#### Hit `Enter` when asked “Enter file in which to save the key (`/home/developer/.ssh/id_rsa`)”:

## 6. Copy SSH key to master node’s authorized keys:

`cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys`

## 7. Copy master node’s SSH key to slave’s authorized keys:

`ssh-copy-id -i $HOME/.ssh/id_rsa.pub edureka@slave`

## 8. Install Java

## 9. View Java version:

`java -version`

#### Ensure the Java version is `1.8.0_181`

## 10. Download Hadoop on all nodes:

`wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz`

## 11. Extract Hadoop tar file on all nodes:

`tar -xvf hadoop-2.7.3.tar.gz`

## 12. Add Hadoop and Java paths in the bash file (`.bashrc`) on all nodes:

`sudo vim .bashrc`

#### To set these commands to the current terminal:

`source .bashrc`

#### To ensure these changes have been made:

`java -version`
`hadoop version`

## 13. If you get the error, `0.0.0.0: Error: JAVA_HOME is not set and could not be found.`, after running `start-all.sh`, you need to go into `hadoop-env.sh` and change Java_Home to its home path `whereis java`.

## 14. Edit the configuration files in master node in `hadoop-2.6.0/etc/hadoop`:

`cd hadoop-2.6.0/etc/hadoop`
`sudo vim master`

#### Add master to first line. Then save and exit

`sudo vim slaves`

#### Add master to first line, slave to second line. Then save and exit

## 15. Edit slaves file in slave nodes, which is also under `/home/developer/hadoop-2.6.0/etc/hadoop/`:

`cd <dir>`
`sudo vim slaves`

#### Add slave to first line of this file. Then save and exit

## 16. Edit `core-site.xml` file on both master and slave nodes:

`sudo vim /home/developer/hadoop-2.6.0/etc/hadoop/core-site.xml`

#### Insert the following between the `<configuration>` flags:

```xml
<property>
	<name>fs.default.name</name>
	<value>hdfs://master:9000</value>
</property>
```

## 17. Edit `hdfs-site.xml` file on master as follows:

`sudo vim /home/developer/hadoop-2.6.0/etc/hadoop/hdfs-site.xml`

#### Insert the following between the `<configuration>` flags:

```xml
<property>
	<name>dfs.replication</name>
	<value>2</value>
</property>
<property>
	<name>dfs.permissions</name>
	<value>false</value>
</property>
<property>
	<name>dfs.namenode.name.dir</name>
        <value>/home/developer/hadoop-2.6.0/namenode</value>
</property>
<property>
	<name>dfs.datanode.data.dir</name>
        <value>/home/developer/hadoop-2.6.0/datanode</value>
</property>
```

## 18. Edit `hdfs-site.xml` file on slave machine as follows:

`sudo vim /home/developer/hadoop-2.6.0/etc/hadoop/hdfs-site.xml`

#### Insert the following between the `<configuration>` flags:
        
```xml
<property>
	<name>dfs.replication</name>
	<value>2</value>
</property>
<property>
	<name>dfs.permissions</name>
	<value>false</value>
</property>
<property>
	<name>dfs.datanode.data.dir</name>
	<value>/home/developer/hadoop-2.6.0/datanode</value>
</property>
```

## 19. Copy the template file `mapred-site.xml.template` (within `/home/developer/hadoop-2.6.0/etc/hadoop/` directory) to same directory, named as `mapred-site.xml`:

`cp /home/developer/hadoop-2.6.0/etc/hadoop/mapred-site.xml.template /home/developer/hadoop-2.6.0/etc/hadoop/mapred-site.xml`

#### Edit this file as follows:

`sudo vim /home/developer/hadoop-2.6.0/etc/hadoop/mapred-site.xml`

#### Insert the following between the `<configuration>` tags:

```xml
<property>
	<name>mapreduce.framework.name</name>
	<value>yarn</value>
</property>
```

## 20. Edit the `yarn-site.xml` file on both master and slave nodes:

`sudo vim /home/developer/hadoop-2.6.0/etc/hadoop/yarn-site.xml`

#### Include the following between the `<configuration>` flags:

```xml
<property>
	<name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
</property>
<property>
	<name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
	<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
```

## 21. Format the namenode:

`hadoop namenode -format`

## 22. From the directory `/home/developer/hadoop-2.6.0`, Start all daemons (only on master machine):

`./sbin/start-all.sh`

## 23. `jps` and make sure all daemons are running.

## 24. If datanode is not running, possible error is it has a different cluster ID than the namenode:

`./stop-all.sh`
`sudo vim ~/hadoop-2.6.0/datanode/current/VERSION`
`sudo vim ~/hadoop-2.6.0/namenode/current/VERSION`

## 25. Go to `master:50070/dfshealth.html.`

#### This page should pull up and give some info about HDFS.
#### `hdfs dfs -ls /` (has nothing right now)
#### `hdfs fsck /` should report back all is healthy.

## 26. Install MySQL Server:

`sudo apt-get install mysql-server`

#### You will be prompted to set a password for root.                          

## 27. Log into MySQL:

`mysql -u root -p`

#### Prompted to type in password.

## 28. Create database:

`CREATE DATABASE PROJECT3_DB;`

## 29. Use this database:

`USE PROJECT3_DB;`

## 30. Create admin for this database:

`GRANT ALL ON PROJECT3_DB.* TO PROJECT3_ADMIN IDENTIFIED BY ‘p4ssw0rd’;`

## 31. Exit and log in as new admin:

`EXIT;`
`mysql -u PROJECT3_DB -p;`
`USE PROJECT3_DB;`

## 32. Download Sqoop.

#### Go to `https://sqoop.apache.org` and click link for latest addition (1.4.7 at this time).
#### Click on location and download the tar. Or can do:

`wget https://apache.claz.org/sqoop/1.4.7/sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz`

#### Here, `claz` is one of the locations. Again, visit `https://sqoop.apache.org` to see all location options.

## 33. Extract sqoop:

`tar -xvf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz`

## 34. Move extracted directory to `/usr/lib/sqoop`  (may need to create Sqoop directory):

`sudo mkdir /usr/lib/sqoop`
`sudo mv sqoop-1.4.7.bin__hadoop-2.6.0 /usr/lib/sqoop`

## 35. Insert following lines into `.bashrc` (located in home directory):

`sudo vim .bashrc`

#### Insert:
#### Sqoop

`export SQOOP_HOME=/usr/lib/sqoop`
`export PATH=$PATH:$SQOOP_HOME/bin`

#### Exit vim with `[ESC] :x!`.
#### Execute this bash file:

`source .bashrc`

## 36. Change into conf directory (`$SQOOP_HOME/conf`) and copy `sqoop-env-template` file:

`mv sqoop-env-template.sh sqoop-env.sh`

## 37. Find and uncomment the following lines and edit them as well:

`export HADOOP_COMMON_HOME=$HOME/hadoop-2.6.0`
`export HADOOP_MAPRED_HOME=$HOME/hadoop-2.6.0`

## 38. Download MySQL connector:

`wget http://ftp.ntu.edu.tw/MySQL/Downloads/Connector-J/mysql-connector-java-8.0.13.tar.gz`

#### Untar it:

`tar -zxf mysql-connector-java-8.0.13.tar.gz`

## 39. Move the jar of the connector to Sqoop’s library:

`cd mysql-connector-java-8.0.13/`
`mv mysql-connector-java-8.0.13.jar /usr/lib/sqoop/sqoop-1.4.6.bin__hadoop-2.6.0/lib`

## 40. Verify the Sqoop version:

`cd $SQOOP_HOME/bin`
`sqoop-version`

#### May get warnings about other paths not set. This is ok. There should also be:
#### <date> <time> INFO sqoop.Sqoop: Running Sqoop version 1.4.7
#### Sqoop 1.4.7 git commit id 5b34accaca7de251fc91161733f906af2eddbe83 (different though)
#### Compiled by maugli on Thu Dec 21 15:59:58 STD 2017

## 41. Create folder structure in HDFS:

`hdfs dfs -mkdir /user/`
`hdfs dfs -mkdir /user/developer/`
`hdfs dfs -mkdir /user/developer/Data/`

## 42. Download Oozie:

`wget https://www-us.apache.org/dist/oozie/5.1.0/oozie-5.1.0.tar.gz`

## 43. Download HBase:

`wget https://www.apache.org/dyn/closer.lua/hbase/2.1.2/hbase-2.1.2-bin.tar.gz`

## 44. Download Zookeeper:

`wget https://archive.apache.org/dist/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz`
