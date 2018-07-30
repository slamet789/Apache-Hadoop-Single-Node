# Instalasi Hadoop Single Node di Ubuntu #
## Penjelasan Singkat : ##
Hadoop terdiri dari empat lapisan utama terdiri dari :
* Hadoop Common : kumpulan utilitas dan pustaka yang mendukung modul Hadoop lainnya
* HDFS (Hadoop Distributed File System) : bertanggung jawab untuk meneruskan data ke disk
* YARN (Yet Another Resource Negotiator) : "sistem operasi" untuk HDFS
* MapReduce : model pemrosesan asli untuk kelompok Hadoop yang mendistribusikan pekerjaan dalam cluster atau peta, kemudian mengatur dan mengurangi hasil dari node menjadi respons terhadap permintaan
Single node berarti hanya ada satu DataNode yang berjalan dan NameCode, DataNode, ResourceManager, dan NodeManager diatur dalam satu single machine. Tujuan dari pembuatannya biasanya untuk pembelajaran dan pengujian.
## Kebutuhan : ##
* Server Ubuntu
* Java 8 Package
* Hadoop 2.7.3

## Instalasi Hadoop ##
* Pastikan terdapat Java 8 pada sistem operasi, jika belum coba unduh dan ekstrak
```
tar -xvf jdk-8u101-linux-i586.tar.gz
```
* Cek versi Hadoop terbaru yang akan digunakan, pada praktikum ini menggunakan hadoop-2.7.3 di web http://hadoop.apache.org/releases.html
* Menginstal Hadoop 2.7.3
```
wget https://archive.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
```
![Instalasi Hadoop](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop1.png "Menginstal Hadoop dari distribusi")
* Mengekstrak file hadoop-2.7.3.tar.gz
```
tar -xzvf hadoop-2.7.3.tar.gz
```
![Ekstrak File Hadoop](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop2.png "Ekstrak file Hadoop")
![Ekstrak File Hadoop](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop3.png "Ekstrak file Hadoop")
* Membuat file .bashrc untuk mengatur path home dari Hadoop dan Java
```
nano .bashrc
```
![Membuat file .bashrc](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop4.png "Membuat file .bashrc")

Isi file .bashrc sebagai berikut :

![Isi file .bashrc](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop5.png "Isi file .bashrc")
* Untuk menjalankan perubahan, jalankan perintah berikut
```
source .bashrc
```
![Menjalankan perubahan](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop6.png "Menjalankan perubahan pada file .bashrc")
* Selanjutnya cek versi Java dan Hadoop
```
java -version
```
```
hadoop version
```
Hasilnya sebagai berikut :

![Hasil cek versi Hadoop](https://github.com/slamet789/Apache-Hadoop-Single-Node/blob/1-instalasi-hadoop/instalasi-hadoop/install-hadoop7.png "Hasil cek versi Hadoop")

Referensi :
* https://www.edureka.co/blog/install-hadoop-single-node-hadoop-cluster
