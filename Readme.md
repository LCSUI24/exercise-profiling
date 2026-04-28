![Screenshot 2026-04-28 193120.png](Screenshot%202026-04-28%20193120.png)
![Screenshot 2026-04-28 193231.png](Screenshot%202026-04-28%20193231.png)
![Screenshot 2026-04-28 193258.png](Screenshot%202026-04-28%20193258.png)
![Screenshot 2026-04-28 194129.png](Screenshot%202026-04-28%20194129.png)
![Screenshot 2026-04-28 194144.png](Screenshot%202026-04-28%20194144.png)
![Screenshot 2026-04-28 194156.png](Screenshot%202026-04-28%20194156.png)


Setelah melakukan optimisasi terjadi perubahan yang signifikan pada saat menjalankan jmeter, contohnya pada plan pertama rata-rata waktu setelah optimisasi sekitar 1000an yang itu jauh lebih cepat dari sebelumnya yang menyentuh 90000
latencynya juga jauh lebih cepat dari sebelum di optimisasi

Reflection:
1.
JMeter (Performance Testing): Pendekatan Black-box dari luar sistem. Fokus pada metrik eksternal seperti latency, throughput, dan error rate saat load/stress test.
IntelliJ Profiler (Profiling): Pendekatan White-box dari dalam kode. Fokus pada metrik internal seperti penggunaan CPU per method, alokasi memori di Heap, dan thread secara real-time di level baris kode.

2. Cara profiling mengidentifikasi titik lemah profiling bekerja dengan merekam aktivitas JVM selama aplikasi berjalan dengan:
Flame Graphs: Visualisasi yang menunjukkan metode mana yang paling banyak memakan waktu CPU.
Call Tree: Menunjukkan urutan eksekusi dan bagian mana dari stack trace yang paling lambat.
Memory Allocation: Mengidentifikasi objek yang menyebabkan memory leak atau penggunaan memori yang tidak efisien.

3. Sangat efektif karena terintegrasi langsung dalam IDE, Profiler memungkinkan pengembang untuk berpindah dari hasil analisis langsung ke baris kode yang bermasalah. Fitur seperti JFR (Java Flight Recorder) memberikan data yang sangat akurat dengan overhead rendah terhadap performa aplikasi saat diuji.

4. saat awal awal mencoba seeding data untuk yang master memakan waktu sekitar 20 menit dan student course yang bahkan sudah hampir 2 jam tidak selesai selesai. 
Solusi saya dengan menambahkan @Transactional di setiap method seedingnya agar saat menjalankan query insert membuat sebuah transaksi dan langsung memasukkan semua data dan commit, tidak commit satu satu.

5. 
Integrasi di Intelij langsung: Tidak memerlukan instalasi alat eksternal tambahan.
Visualisasi Intuitif: Memudahkan pembacaan data performa yang kompleks.
Deteksi Memory Leak: Mudah untuk menganalisis snapshot memori untuk menemukan objek yang tidak terhapus oleh Garbage Collector.

6. Jika JMeter menunjukkan hasil lambat tapi Profiler menunjukkan kode baik-baik saja dapat diperiksa berdasarkan:
Eksternal: Periksa database (query lambat), jaringan (latency), atau API pihak ketiga.
Konfigurasi: Periksa limitasi thread pool atau koneksi database di level server.
Langkah: Melakukan sinkronisasi skenario uji JMeter dengan kondisi profiling yang sama untuk memastikan jalur eksekusi yang diuji sama persis.

7. Strategi Optimasi:
Refactoring algoritma yang tidak efisien atau menerapkan caching.
 Mengoptimalkan query SQL (index) atau menggunakan pemrosesan asynchronous.

Menjaga Fungsionalitas:
Unit Testing & Regression Testing: Menjalankan rangkaian tes otomatis (JUnit/Pytest) setelah optimasi untuk memastikan logika bisnis tidak berubah.
Verification Run: Melakukan pengujian ulang dengan JMeter untuk memastikan matrix performa benar-benar meningkat tanpa merusak respons aplikasi.