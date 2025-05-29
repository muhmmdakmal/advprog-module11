## AdvProg - Tutorial Module 11
<h2>
Nama   : Muhammad Akmal Abdul Halim

Kelas  : B

NPM    : 2306245125
</h2>

## Reflection on Hello Minikube

1. Compare the application logs before and after you exposed it as a Service.
Try to open the app several times while the proxy into the Service is running.
What do you see in the logs? Does the number of logs increase each time you open the app?

Pada gambar pertama, terlihat hanya ada log inisialisasi aplikasi:
![fotologs](img/FotoNomor1Before.png)

Ini menunjukkan bahwa aplikasi telah berhasil dimulai dan server HTTP dan UDP telah diinisialisasi pada port masing-masing, tetapi belum ada traffic masuk karena layanan belum diekspos ke luar.

Pada gambar kedua, terlihat log yang sama dengan tambahan log baru:
![fotologs](img/FotoNomor1.png)

Setelah layanan diekspos menggunakan perintah minikube service hello-node, terlihat adanya permintaan GET yang masuk ke aplikasi. Ini ditandai dengan munculnya entri log baru pada pukul 11:00:55, yang merekam permintaan HTTP GET ke path root ("/").

Perbedaan log ini dengan jelas menunjukkan bahwa setelah layanan diekspos ke luar menggunakan Kubernetes Service, aplikasi mulai menerima traffic eksternal yang dibuktikan dengan adanya permintaan GET. Jarak waktu yang cukup signifikan antara inisialisasi aplikasi (08:33) dan permintaan pertama (11:00) menggambarkan rentang waktu ketika Service diekspos dan kemudian diakses dari luar cluster Kubernetes.


2.  Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`.

Dalam Kubernetes, flag -n pada perintah kubectl get berfungsi untuk menyebutkan namespace tertentu yang ingin diperiksa. Fitur ini sangat membantu terutama saat mengelola cluster yang memiliki beberapa service dengan penamaan identik di namespace yang berbeda-beda.

Apabila perintah kubectl get dijalankan tanpa menyertakan flag -n, sistem secara otomatis hanya akan menampilkan resource dari namespace default. Hal ini terjadi karena namespace pada Kubernetes dirancang sebagai mekanisme untuk memisahkan dan mengorganisir resource dalam sebuah cluster.

Ketika kita menambahkan parameter -n kube-system pada perintah, kubectl akan secara khusus menampilkan komponen-komponen sistem inti Kubernetes seperti layanan DNS dan server API yang berada dalam namespace tersebut.

Sebaliknya, jika kita tidak menggunakan opsi -n sama sekali, hasil yang ditampilkan hanya akan mencakup resource-resource yang telah dibuat oleh pengguna secara manual dalam namespace default.

3.  What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

Perbedaan utama antara strategi Rolling Update dan Recreate pada Kubernetes berfokus pada ketersediaan aplikasi selama proses pembaruan.

Saat menggunakan strategi Recreate, sistem akan menghentikan seluruh pod versi lama terlebih dahulu sebelum menciptakan pod dengan versi terbaru. Akibatnya, terjadi periode ketidaktersediaan layanan atau downtime selama transisi tersebut berlangsung.

Di sisi lain, Rolling Update mengadopsi pendekatan bertahap yang menjaga ketersediaan aplikasi. Dengan metode ini, pod-pod baru dihadirkan secara bergantian sambil tetap mempertahankan pod versi lama yang masih aktif. Pendekatan ini memastikan aplikasi tetap dapat diakses oleh pengguna selama proses pembaruan, sehingga menghindari terjadinya downtime.

4. 