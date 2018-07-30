# Apache Hadoop

Apache Hadoop menggabungkan sejumlah peningkatan yang signifikan atas jalur rilis utama sebelumnya (hadoop-2.x).

Rilis ini umumnya tersedia (GA), yang berarti bahwa itu mewakili titik stabilitas dan kualitas API yang kami anggap siap produksi.

Pengguna didorong untuk membaca set lengkap dari catatan rilis. Halaman ini memberikan ikhtisar tentang perubahan besar.

### Versi Java minimum yang dibutuhkan meningkat dari Java 7 ke Java 8
Semua Hadoop JAR sekarang dikompilasi dengan menargetkan versi runtime Java 8. Pengguna yang masih menggunakan Java 7 atau di bawahnya harus melakukan upgrade ke Java 8.

### Mendukung pengkodean penghapusan dalam HDFS
Penghapusan pengkodean adalah metode penyimpanan data yang tahan lama dengan penghematan ruang yang signifikan dibandingkan dengan replikasi. Encoding standar seperti Reed-Solomon (10,4) memiliki overhead ruang 1,4x, dibandingkan dengan overhead 3x dari replikasi HDFS standar.

Karena pengkodean penghapusan membebankan biaya tambahan selama rekonstruksi dan melakukan sebagian besar pembacaan jarak jauh, secara tradisional telah digunakan untuk menyimpan data yang lebih dingin dan kurang sering diakses. Pengguna harus mempertimbangkan jaringan dan overhead CPU dari pengkodean penghapusan ketika menerapkan fitur ini.

Detail lebih lanjut tersedia dalam dokumentasi HDFS Erasure Coding.

### YARN Timeline Service v.2
Kami memperkenalkan pratinjau awal (alfa 2) dari revisi besar dari Layanan Garis Waktu YARN: v.2. YARN Timeline Service v.2 membahas dua tantangan utama: meningkatkan skalabilitas dan keandalan Layanan Timeline, dan meningkatkan kegunaan dengan memperkenalkan arus dan agregasi.

YARN Timeline Service v.2 alpha 2 disediakan sehingga pengguna dan pengembang dapat mengujinya dan memberikan umpan balik dan saran untuk menjadikannya pengganti yang siap untuk Layanan Garis Waktu v.1.x. Itu harus digunakan hanya dalam kapasitas uji.

Detail lebih lanjut tersedia dalam dokumentasi v.2 Timeline Layanan YARN.

### Penulis ulang skrip shell
Skrip shell Hadoop telah ditulis ulang untuk memperbaiki banyak bug lama dan menyertakan beberapa fitur baru. Sementara mata telah disimpan menuju kompatibilitas, beberapa perubahan dapat merusak instalasi yang ada.

Perubahan yang tidak kompatibel didokumentasikan dalam catatan rilis, dengan diskusi terkait di HADOOP-9902.

Rincian lebih lanjut tersedia dalam dokumentasi Unix Shell Guide. Pengguna listrik juga akan senang dengan dokumentasi Unix Shell API, yang menggambarkan banyak fungsi baru, khususnya yang terkait dengan ekstensibilitas.

### Shaded client jars
Artefak hadoop-client Maven yang tersedia dalam rilis 2.x menarik dependensi transitif Hadoop ke dalam classpath aplikasi Hadoop. Ini bisa menjadi masalah jika versi dependensi transitif ini bertentangan dengan versi yang digunakan oleh aplikasi.

HADOOP-11804 menambahkan artefak hadoop-client-api dan hadoop-client-runtime baru yang menaungi ketergantungan Hadoop ke dalam satu stoples. Ini menghindari kebocoran ketergantungan Hadoop ke dalam classpath aplikasi.

### Dukungan untuk Kontainer Opportunistik dan Penjadwalan Terdistribusi.
Sebuah gagasan ExecutionType telah diperkenalkan, di mana Aplikasi sekarang dapat meminta kontainer dengan jenis eksekusi Opportunistik. Wadah jenis ini dapat dikirim untuk eksekusi pada NM bahkan jika tidak ada sumber daya yang tersedia pada saat penjadwalan. Dalam kasus seperti itu, kontainer ini akan diantrekan di NM, menunggu sumber daya tersedia untuk dimulainya. Wadah oportunistik memiliki prioritas lebih rendah daripada kontainer Terjamin default dan karena itu diutamakan, jika diperlukan, untuk memberi ruang bagi wadah yang Dijamin. Ini harus meningkatkan pemanfaatan cluster.

Kontainer oportunistik secara default dialokasikan oleh RM pusat, tetapi dukungan juga telah ditambahkan untuk memungkinkan wadah oportunistik untuk dialokasikan oleh penjadwal didistribusikan yang diimplementasikan sebagai pencegat AMRMProtocol.

Silakan lihat dokumentasi untuk lebih jelasnya.

### MapReduce optimasi asli tingkat tugas
MapReduce telah menambahkan dukungan untuk implementasi asli dari kolektor keluaran peta. Untuk pekerjaan padat-acak, ini dapat mengarah pada peningkatan kinerja 30% atau lebih.

Lihat catatan rilis untuk MAPREDUCE-2841 untuk detail lebih lanjut.

### Mendukung lebih dari 2 NameNodes.
Implementasi awal HDFS NameNode ketersediaan tinggi disediakan untuk NameNode aktif tunggal dan Single NameNode. Dengan mereplikasi suntingan ke kuorum tiga JournalNodes, arsitektur ini mampu mentoleransi kegagalan satu node dalam sistem.

Namun, beberapa penerapan memerlukan tingkat toleransi kesalahan yang lebih tinggi. Ini diaktifkan oleh fitur baru ini, yang memungkinkan pengguna untuk menjalankan beberapa NameNode siaga. Misalnya, dengan mengkonfigurasi tiga NameNodes dan lima JournalNodes, cluster ini mampu mentoleransi kegagalan dua node daripada hanya satu.

Dokumentasi ketersediaan tinggi HDFS telah diperbarui dengan petunjuk tentang cara mengonfigurasi lebih dari dua NamaNode.

### Port default dari beberapa layanan telah diubah.
Sebelumnya, port default dari beberapa layanan Hadoop berada di jangkauan port ephemeral Linux (32768-61000). Ini berarti bahwa saat startup, layanan kadang-kadang gagal untuk mengikat ke port karena konflik dengan aplikasi lain.

Port-port yang saling bertentangan ini telah dipindahkan dari jangkauan sementara, yang mempengaruhi NameNode, Secondary NameNode, DataNode, dan KMS. Dokumentasi kami telah diperbarui dengan tepat, tetapi lihat catatan rilis untuk HDFS-9427 dan HADOOP-12811 untuk daftar perubahan port.

Dukungan untuk Microsoft Azure Data Lake dan Aliyun Object Storage System konektor sistem file
Hadoop sekarang mendukung integrasi dengan Microsoft Azure Data Lake dan Aliyun Object Storage System sebagai sistem file alternatif yang kompatibel dengan Hadoop.

### Penyeimbang intra-datanode
Satu DataNode mengelola beberapa disk. Selama operasi penulisan normal, disk akan terisi secara merata. Namun, menambah atau mengganti disk dapat menyebabkan kemiringan signifikan dalam DataNode. Situasi ini tidak ditangani oleh penyeimbang HDFS yang ada, yang berkaitan dengan inter-, bukan intra-, DN miring.

Situasi ini ditangani oleh fungsionalitas penyeimbang intra-DataNode baru, yang dipanggil melalui hdfs diskbalance CLI. Lihat bagian penyeimbang disk di Panduan Perintah HDFS untuk informasi lebih lanjut.

### Dikerjakan ulang dan manajemen tumpukan tugas
Serangkaian perubahan telah dibuat untuk menumpuk manajemen untuk daemon Hadoop serta tugas MapReduce.

HADOOP-10950 memperkenalkan metode baru untuk mengkonfigurasi ukuran tumpukan daemon. Khususnya, auto-tuning sekarang dimungkinkan berdasarkan ukuran memori dari host, dan variabel HADOOP_HEAPSIZE telah ditinggalkan. Lihat catatan rilis lengkap HADOOP-10950 untuk detail lebih lanjut.

MAPREDUCE-5785 menyederhanakan konfigurasi peta dan mengurangi ukuran tumpukan tugas, sehingga ukuran tumpukan yang diinginkan tidak lagi perlu ditentukan dalam konfigurasi tugas dan sebagai opsi Java. Konfigurasi yang sudah ada yang menentukan keduanya tidak terpengaruh oleh perubahan ini. Lihat catatan rilis lengkap MAPREDUCE-5785 untuk detail lebih lanjut.

### S3Guard: Consistency and Metadata Caching untuk klien sistem berkas S3A
HADOOP-13345 menambahkan fitur opsional ke klien S3A penyimpanan Amazon S3: kemampuan untuk menggunakan tabel DynamoDB sebagai penyimpanan file dan metadata direktori yang cepat dan konsisten.

Lihat S3Guard untuk detail lebih lanjut.

### Federasi Berbasis Router HDFS
Federasi berbasis router HDFS menambahkan lapisan perutean RPC yang menyediakan tampilan gabungan dari beberapa ruang nama HDFS. Ini mirip dengan fungsi ViewFs yang ada) dan Fungsionalitas Federasi HDFS, kecuali tabel mount dikelola di sisi server oleh lapisan perutean alih-alih pada klien. Ini menyederhanakan akses ke cluster federasi untuk klien HDFS yang ada.

Lihat HDFS-10467 dan dokumentasi Federasi berbasis Router HDFS untuk lebih jelasnya.

### Konfigurasi konfigurasi antrean Kapasitas Scheduler berbasis API
Ekstensi OrgQueue ke penjadwal kapasitas menyediakan cara terprogram untuk mengubah konfigurasi dengan menyediakan REST API yang dapat dipanggil pengguna untuk mengubah konfigurasi antrian. Ini memungkinkan otomatisasi pengelolaan konfigurasi antrian oleh administrator di administrator queque_queue ACL.

Lihat YARN-5734 dan dokumentasi Penjadwal Kapasitas untuk informasi lebih lanjut.

### Sumber Daya Sumber Daya YARN
Model sumber daya YARN telah digeneralisasikan untuk mendukung jenis sumber daya yang dapat dihitung oleh pengguna di luar CPU dan memori. Misalnya, administrator klaster dapat menentukan sumber daya seperti GPU, lisensi perangkat lunak, atau penyimpanan yang dilampirkan secara lokal. Tugas YARN kemudian dapat dijadwalkan berdasarkan ketersediaan sumber daya ini.
