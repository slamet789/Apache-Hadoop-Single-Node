# Konfigurasi Apache Hadoop Single Node

Setelah instalasi selesai, lakukan konfigurasi seperti di bawah ini.

Masuk ke direktori hadoop  
```cd hadoop-2.7.3/etc/hadoop/```

Edit konfigurasi core-site.xml dengan menggunakan editor (nano)
```nano core-site.xml```

Buat seperti di bawah ini

    <configuration>  
	    <property>  
		    <name>fs.default.name</name>
		    <value>hdfs://localhost:9000</value>
	    </property>
    </configuration>

Edit juga konfigurasi hdfs-site.xml

	<configuration>
		<property>
			<name>dfs.replication</name>
			<value>1</value>
		</property>
		<property>
			<name>dfs.permission</name>
			<value>false</value>
		</property>	
	</configuration>
	
Edit konfigurasi mapred-site.xml

	<configuration>
		<property>
			<name>mapreduce.framework.name</name>
			<value>yarn</value>
		</property>
	</configuration>
	
Edit konfigurasi yarn-site.xml

	<configuration>
		<property>
			<name>yarn.nodemanager.aux-services</name>
			<value>mapreduce_shuffle</value>
		</property>
		<property>
			<name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
			<value>org.apache.hadoop.mapred.ShuffleHandler</value>
		</property>
	</configuration>
	
Lakukan format  
```bin/hadoop namenode -format```

Lakukan start service

	./hadoop-daemon.sh start namenode
	./hadoop-daemon.sh start datanode
	./yarn-daemon.sh start resourcemanager
	./yarn-daemon.sh start nodemanager
	./mr-jobhistory-daemon.sh start historyserver
	
Kemudian untuk mengecek service tersebut sudah berjalan atau belum, gunakan ```jps```