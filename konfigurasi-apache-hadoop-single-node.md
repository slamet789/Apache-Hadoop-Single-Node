# Konfigurasi Apache Hadoop Single Node

Setelah instalasi selesai, lakukan konfigurasi seperti di bawah ini.

Masuk ke direktori hadoop  
```cd hadoop-2.7.3/etc/hadoop/```

Kemudian kita bisa mengecek dengan perintah ```ls``` , seperti gambar di bawah ini :

![cdnlshadoop](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/1.jpg)

Edit konfigurasi core-site.xml dengan menggunakan editor (nano)
```nano core-site.xml```

![core-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/2.jpg)

Buat seperti di bawah ini

    <configuration>  
	    <property>  
		    <name>fs.default.name</name>
		    <value>hdfs://localhost:9000</value>
	    </property>
    </configuration>

![display-core-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/2.1.jpg)

Edit juga konfigurasi hdfs-site.xml dengan perintah

```nano hdfs-site.xml```

![hdfs-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/3.jpg)

Buat seperti di bawah ini

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
  
![display-dfs-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/3.1.jpg)  

Copy mapres-site.xml dari mapred-site.xml.template dengan perintah :

```cp mapred-site.xml.template mmapred-site.xml```

Kemudian edit konfigurasi mapred-site.xml dengan perintah :

```nano mapred-site.xml```

![mapred-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/4.jpg)

Buat seperti di bawah ini

	<configuration>
		<property>
			<name>mapreduce.framework.name</name>
			<value>yarn</value>
		</property>
	</configuration>

![display-mapred-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/4.1.jpg)  
	
Edit konfigurasi yarn-site.xml dengan perintah :

```nano yarn-site.xml```

![yarn-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/5.jpg)

Buat seperti di bawah ini

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
  
  ![display-yarn-site](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/5.1.jpg) 
	
Lakukan format  
```bin/hadoop namenode -format```

![format](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/6.jpg)

Lakukan start service

	./hadoop-daemon.sh start namenode
	./hadoop-daemon.sh start datanode
	./yarn-daemon.sh start resourcemanager
	./yarn-daemon.sh start nodemanager
	./mr-jobhistory-daemon.sh start historyserver
  
![start](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/7.jpg)
	
Kemudian untuk mengecek service tersebut sudah berjalan atau belum, gunakan ```jps```

![jps](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/8.jpg)

Maka tampilannya saat di buka mwlalui browser menjadi :

![tampilan](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/gambar/pict/tampilan.jpg)
