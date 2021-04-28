# struktur-data-h-praktikum-2-2021
## Penomoran Garasi Saha
### Verdict
AC saat praktikum
#### Bukti
![verdict_pgs](/img/verdict_pgs.jpg)
### Penjelasan Soal
Diberikan beberapa bilangan yang merupakan nomor ruangan dari garasi saha. Bilangan-bilangan tersebut disusun menjadi `Binary Search Tree`. Kemudian, diminta untuk mengeluarkan bilangan dari level terbawah sampai teratas, dengan ketentuan: level paling bawah dikeluarkan bilangan yang paling rendah (minimal), level di atasnya bilangan dengan nilai maksimal, level di atasnya lagi nilai minimal lagi dan seterusnya.
### Penjelasan Solusi
Permasalahan Penomoran Garasi Saha dapat diselesaikan dengan menggunakan `BST` dan traversal `level order`/`BFS` yang telah dimodifikasi (`__bst__levelorder`) serta struktur data `Queue` yang menyimpan data dalam bentuk `BSTNode` dan ditambahkan data tentang `level` node tersebut dalam tree. Adapun hal yang dilakukan dalam fungsi `__bst__levelorder` adalah menginisialisasi 2 struktur data `queue`. Queue `myQ` digunakan untuk melakukan `BFS` seperti biasanya, sedangkan `nuQ` berfungsi untuk menyimpan data dari queue `myQ`. Hal ini dilakukan karena untuk melakukan proses traversal, __harus__ dilakukan pop pada `myQ`, sehingga pada akhirnya `myQ` akan kosong.
Pertama, `myQ` akan diisi dulu dengan nilai root dari tree (`queue_push(&myQ, *root, 1)`, 1 merupakan level dari node root tree). Lalu, dibuat variabel `level` yang diinisialisai nilainya `0`. Variabel `level` di sini berfungsi untuk menyimpan berapa banyak level dalam tree, sehingga dapat dikirimkan ke fungsi `printLevel`. 

Kemudian, dilakukan perulangan `while` selama `myQ` tidak kosong. Isi dari perulangan tersebut adalah mempush front dari `myQ` ke `nuQ`. Lalu, dilakukan traversal, yaitu `push` `left` dan `right` dari node yang disimpan dalam front `myQ`, dan levelnya ditambah 1 (`(myQ._front->depth)+1`), dan variabel `level` ditambah 1. Kemudian, `myQ` di`pop`.

Di dalam fungsi `printLevel`, yang dilakukan adalah membuat suatu array yang berukuran `level`. Array ini berfungsi untuk menyimpan nilai max/min dari masing-masing level tree. Lalu, array tersebut diinisialisasi dengan dua anggota pertama dari `queue` (karena root tidak memiliki "saudara" dan untuk mempermudah perbandingan untuk level 2). Kemudian, dijalankan perulangan selama `queue` tidak kosong. `if(depth == p->depth)` berfungsi untuk memeriksa apakah `front` dari `queue` satu `level` dengan anggota array sehingga perlu dibandingkan. 
- Jika iya, terdapat `if-else` yang berfungsi memeriksa apakah diambil nilai yang terkecil antara kedua nilai atau terbesar. Karena polanya adalah min-max-min dari level terbawah, maka jika level terbawah ganjil, tiap level ganjil akan mengambil nilai minimum, yang genap akan mengambil nilai maksimum dan sebaliknya. Hal ini diuji dengan `depth%2 == level%2`. Kemudian, di dalam `if-else` tersebut berisi `if` yang memeriksa apakah anggota array perlu diganti atau tidak.

- Jika tidak, tidak dibandingkan, dan pemeriksaan dilanjutkan pada level selanjutnya. Dalam kode ini ditunjukkan sebagai
```
depth++;
arr[depth-1] = p->data.key;
```
Kemudian, dilakukan `pop` pada `queue` sehingga dapat memeriksa anggota `queue` selanjutnya.
Terakhir, keluarkan nilai array dari index terbesar ke 0.

Dalam fungsi main, dilakukan penerimaan input dan pemanggilan fungsi yang telah dijabarkan. Pertama, akan diterima input banyak anggota tree yang disimpan dalam variabel `n`. Kedua, ada loop `for` untuk memasukkan anggota tree sesuai input. Ketika loop berakhir, tree `myTree` dikirimkan ke fungsi `bst_levelorder` yang akan mengirim nilai rootnya ke `__bst__levelorder`.

### Visualisasi Solusi
Untuk mempermudah visualisasi solusi dari permasalahan Penomoran Garasi Saha, digunakan sample input berikut:

```
7
500
250
750
125
375
625
875
```

Setelah nilai-nilai tersebut dimasukkan dalam tree, maka akan terjadi operasi seperti berikut.

![pgs1](/img/si_pgs1.gif)

Melakukan operasi `BFS` dengan bantuan queue `myQ`, dan menyimpan data dalam `nuQ`

![pgs2](/img/si_pgs2.JPG)

Membuat array sebesar `level`

![pgs3](/img/si_pgs3.JPG)

Mengisi index pertama dengan nilai `root`.

![pgs4a](/img/si_pgs4a.JPG)
![pgs4b](/img/si_pgs4b.JPG)

Mengisi index kedua dengan nilai `root->left`, kemudian membandingkannya dengan anggota tree yang se`level`. Karena banyak `level` 3, dan level yang dibandingkan adalah `level` 2, diambil nilai yang terbesar (`750`).

![pgs5a](/img/si_pgs5a.JPG)

Karena `front` dari `queue` sekarang berbeda `level` dengan anggota array pada index sekarang, index selanjutnya diisi dengan nilai `front` tersebut

![pgs5b](/img/si_pgs5b.gif)

Kemudian, anggota `queue` dibandingkan dengan anggota array pada index sekarang. Karena yang diminta pada `level` ini adalah nilai minimun, tidak terjadi perubahan pada array. 

![pgs6](/img/si_pgs6.JPG)

Keluarkan (`printf`) anggota `arr` dari index terbesar sampai 0.

Output:
```
125 750 500
```

## Roni Suka Merah
### Verdict
AC saat praktikum
#### Bukti
![verdict_rsm](/img/verdict_rsm.jpg)
### Penjelasan Soal
Diberikan beberapa bilangan yang lalu disusun dalam sebuah tree. Kemudian, diminta untuk mengeluarkan anggota tree sesuai dengan contoh output dan clue yang diberikan
### Penjelasan Solusi
Dari input-output yang dihasilkan pada soal (Input: `6 1 8 12 3 7`, Output: `1 3 6 7 8 12`) dan clue, disimpulkan bahwa output merupakan hasil traversal inorder dengan operasi print.

Dalam fungsi main:

Dibuat suatu `binary search tree` yang bernama `set`, berfungsi untuk menyimpan data dan variabel `N` untuk menyimpan berapa banyak anggota dari `tree` yang akan dibuat. Lalu, program meminta input untuk disimpan dalam N. Selanjutnya, dilakukan loop `for` sebanyak `N` kali. Dalam loop, nilai yang diinput dimasukkan ke dalam `tree` (`bst_insert`) dibantu oleh variabel `num`. Setelah loop selesai, `set` akan dikirim ke fungsi `bst_inorder`. Fungsi tersebut akan mengirim `root` dari `set` ke `__bst__inorder`. Fungsi ini akan melakukan operasi yang akan mengeluarkan nilai anggota-anggota dari sebelah kiri node yang dikirimkan ke fungsi, lalu nilai node itu sendiri, lalu nilai anggota-anggota yang berada di sebelah kanan node tersebut. Lebih jelasnya dapat dilihat di "Visualisasi Solusi".

### Visualisasi Solusi
Untuk mempermudah visualisasi solusi dari permasalahan Roni Suka Merah, digunakan sample input berikut:

```
6
6
1
8
12
3
7
```

![rsm1](/img/si_rsm1.JPG)

Membuat `tree` dari input-input yang diberikan.

![rsm2](/img/si_rsm2.JPG)

Memulai dari node `root`, yaitu 6. 

![rsm3](/img/si_rsm3.JPG)

Karena ada node disebelah kiri node 6, fungsi `inorder` untuk node kiri akan dijalankan terlebih dahulu.

![rsm4](/img/si_rsm4.JPG)

Tidak ada node di kiri node 1, sehingga keluarkan 1.

![rsm5](/img/si_rsm5.JPG)

`inorder` dilakukan pada node sebelah kanan dari node 1, yaitu node 3. Karena disebelah kiri dan kanan dari node 3 tidak ada, maka di `print` 3 saja.

![rsm6](/img/si_rsm6.JPG)

Karena operasi `inorder` pada node-node di sebelah kiri node 6 sudah selesai, print 6.

![rsm6](/img/si_rsm7.JPG)

Kemudian, lanjut ke node sebelah kanan dari node 6, yaitu node 8.

![rsm6](/img/si_rsm8.JPG)

Karena di kiri node 8 ada node 7, dan node 7 tidak memiliki `child`/`kiri`-`kanan`, print 7.

![rsm6](/img/si_rsm9.JPG)

Lalu, print 8.

![rsm6](/img/si_rsm10.JPG)

Lanjut ke kanan node 8,, yaitu node 12. Karena node 12 juga tidak ada kiri kanannya, print 12.

Traversal `inorder` selesai.

Output:
```
1 3 6 7 8 12 
```

## MALUR TERHUBUNG
### Verdict
AC saat revisi
#### Bukti
![verdict_mt](/img/verdict_mt.jpg)
### Penjelasan Soal
Diberikan beberapa bilangan yang dibentuk menjadi `Binary Search Tree`. Lalu, diberikan 2 bilangan. Kemudian, diminta mengeluarkan hasil penjumlahan dari subtree yang menghubungkan kedua bilangan tersebut.
### Penjelasan Solusi
Pertama, diinisialisasi sebuah `tree` bernama `set`. Lalu, didefinisikan variabel:
- `N` : untuk menyimpan banyak anggota `tree`.
- `Q` : untuk menyimpan banyak pasangan bilangan yang akan dicari penjumlahan subtree yang menghubungkan keduanya.
- `L` : untuk menyimpan bilangan pertama dari pasangan tersebut. 
- `R` : untuk menyimpan bilangan kedua dari pasangan tersebut.
- `A` : untuk menyimpan sementara nilai yang ingin dimasukkan ke dalam tree.
- `sum` : untuk menyimpan penjumlahan dari subtree.

Lalu, program meminta input yang akan disimpan ke variabel `N` dan `Q`. Kemudian, dijalankan loop `for` sebanyak `N` kali. Dengan bantuan variabel `A`, input dimasukkan ke dalam `tree` `set`. Selesai loop tersebut, program menjalankan lagi loop `for` sebanyak `Q` kali. Di dalam loop, program meminta input pasangan bilangan yang akan dicari penghubungnya, disimpan dalam `L` dan `R`. `sum` diinisialisasi dengan `0`. Kemudian, dilakukan `if-else`:
- Jika nilai `L` dan `R` ada di dalam `tree`, kirimkan alamat `set` `L`, `R`, dan alamat `sum` ke fungsi `findPath`. `findPath` kemudian mengirim `root` dari `set`, `L`, `R`, dan alamat `sum`. Di dalam fungsi `find_path` dilakukan beberapa evaluasi dalam `if-else`:
    - Jika nilai `L` dan `R` sama-sama lebih kecil dari node, panggil kembali fungsi `find_path`, tapi node diganti dengan `node->left`.
    - Jika sama-sama lebih besar dari nilai node, panggil kembali fungsi `find_path`, tapi node diganti dengan `node->left`.
    -Selain kondisi yang telah disebutkan, berarti node adalah __akar yang sama yang terdekat__ dari `L` dan `R`, sehingga lakukan fungsi `addInorder` dengan parameter node tersebut dan `sum`.
    Fungsi `addInorder` bekerja dengan menambahkan nilai dari node paling kiri sampai node paling kanan.
    Setelah fungsi tersebut dilaksanakan, print nilai `sum`.
- Jika tidak, keluarkan `-1`.

### Visualisasi Solusi
Untuk mempermudah visualisasi solusi dari permasalahan MALUR TERHUBUNG, digunakan sample input berikut:

```
8 2
8 3 10 1 6 14 4 7 
1 4  
4 9
```
__Untuk input `1 4`__
![mt1](/img/si_mt1.JPG)

Dibuat tree berdasarkan input.

![mt2](/img/si_mt2.JPG)

Cari 1 dan 4 dahulu. Karena ditemukan, lanjut ke fungsi `find_path`

![mt3](/img/si_mt3.JPG)

Mulai pointer p dari `root`, yaitu node dengan nilai 8. Karena nilainya lebih besar dari kedua nilai yang dicari, pindahkan pointer ke node kirinya (node 3).

![mt4](/img/si_mt4.JPG)

Karena 3 lebih dari 1 dan kurang dari 4, node 3 adalah `root` yang sama yang terdekat, sehingga node 3 dikirim ke fungsi `addInorder`.

![mt5](/img/si_mt5.JPG)

Fungsi `addInorder` akan menjumlahkan semua anggota subtree dibawah node 3, disimpan pada variabel `*sum` (dalam `find_path` dibuat variabel `sum` dan dikirimkan alamatnya ke `addInorder`)

![mt6](/img/si_mt6.JPG)

Kemudian, keluarkan nilai `sum`, yaitu `21`. 

__Untuk input `4 9`__

![mt7](/img/si_mt7.JPG)

9 tidak ditemukan dalam `tree`, sehingga keluarkan `-1`.

Output:
```
21
-1
```

## Nadut Gabut
### Verdict
AC saat revisi
#### Bukti
![verdict_ng](/img/verdict_ng.jpg)
### Penjelasan Soal
Diberikan beberapa bilangan yang disusun menjadi Binary Search Tree. Lalu, diberikan bilangan. Diminta untuk mencari apakah bilangan tersebut dapat dihasilkan dari penjumlahan 3 anggota tree yang bertetangga, dan mengeluarkan output seperti yang dicontohkan dalam soal.
### Penjelasan Solusi
Pertama, diinisialisasi sebuah tree bernama `set`, dan didefinisikan variabel `P` dan `N`. Lalu, untuk mengetahui banyak anggota tree, diambil input yang disimpan dalam variabel `P`. Setelahnya dilakukan loop `for` sebanyak `P` kali yang berfungsi untuk memasukkan nilai ke tree dengan bantuan variabel `Q`. Setelah loop selesai, diambil input bilangan query yang disimpan dalam `N`. Lalu, buat variabel `bool` `check` yang menerima kembalian dari fungsi `findSum` dengan parameter `&set` dan `N`. Jika `check` bernilai `true`, keluarkan `Penjumlahan angka di tree yang menghasilkan N ditemukan`. Jika tidak, keluarkan `Tidak ditemukan penjumlahan angka di tree yang menghasilkan N`.

#### __Penjelasan fungsi `findSum`__

Fungsi `findSum` akan mengirimkan parameter yang diterimanya ke `__findSum`, **kecuali** parameter tree diganti dengan root dari tree.

Kombinasi dari 3 angka pada `tree` ada 5 kemungkinan:
1. `root+left+right`
2. `root+left+left's left`
3. `root+left+left's right`
4. `root+right+right's left`
5. `root+right+right's right`

Semua variasi diperiksa dengan menggunakan fungsi-fungsi yang dipanggil pada fungsi `__findSum`.

Visualisasi dari fungsi-fungsi yang digunakan dalam `__findSum` adalah sebagai berikut:
1. `sumParent`

    ![bd_ng1](/img/bd_ng1.JPG)

2. `sumBranchRight`

    ![bd_ng2a](/img/bd_ng2a.JPG)
    ![bd_ng2b](/img/bd_ng2b.JPG)

3. `sumBranchLeft`

    ![bd_ng3a](/img/bd_ng3a.JPG)
    ![bd_ng3b](/img/bd_ng3b.JPG)

Proses yang terjadi dalam fungsi `__findSum`:

Pertama, sebuah variabel `bool` `check` diinisialisasi dengan nilai `false`. Variabel ini berfungsi untuk menyimpan kembalian dari fungsi `__findSum` yang dilakukan pada node sebelah kiri dan kanan dari node sekarang (`root`).
Lalu, jika `root->left` dan `root->right` ada isinya, lakukan fungsi `sumParent`. Jika nilainya sama dengan `query`, kembalikan `true`. Jika tidak, lanjut kode di bawahnya

Jika `root->right` ada nilainya,
- Jika `root->right->right` ada nilainya, lakukan fungsi `sumBranchRight` pada node `root->right`. Jika nilai kembaliannya sama dengan `query`, kembalikan nilai `true`
- Jika `root->right->left` ada nilainya, lakukan operasi yang sejenis dengan operasi di atas.

Jika belum juga ada yang mengembalikan nilai `true`, maka lakukan operasi yang sejenis dengan operasi `root->right` pada `root->left`.

Jika masih belum ditemukan nilai `true`, lanjutkan ke kode di line selanjutnya.

Jika `root->left` ada, buat `check` = nilai kembalian dari fungsi `__findSum` yang dimulai dari sub-node kiri dari node yang sekarang. Jika `check` nilainya `true`, langsung return `true`. Jika tidak, lakukan operasi yang sejenis pada `root->right`. Jika `check` tidak juga bernilai `true`, kembalikan nilai `false`.

### Visualisasi Solusi
Untuk mempermudah visualisasi solusi dari permasalahan Nadut Gabut, digunakan sample input berikut:

```
10
73 66 91 53 72 77 98 74 78 79
266
```

![ng1a](/img/si_ng1a.JPG)

Memeriksa `sumParent`. Karena belum `==query`, lanjut ke operasi selanjutnya.

![ng1b](/img/si_ng1b.JPG)

Memeriksa `sumBranchRight` dari subnode `right`. Karena belum `==query`, lanjut ke operasi selanjutnya.

![ng1c](/img/si_ng1c.JPG)
![ng1d](/img/si_ng1d.JPG)
![ng1e](/img/si_ng1e.JPG)

Begitupula dengan `sumBranchLeft` dari subnode right, `sumBranchRight` dari subnode `left`, dan `sumBranchLeft` dari subnode `left`, karena belum ==query, lanjut ke operasi selanjutnya.

![ng2](/img/si_ng2.JPG)

Memeriksa `sumParent` dari node 66. Karena belum `==query`, dan tidak ada node "cucu", maka lanjut ke node sebelah kanan dari `root`.

![ng3](/img/si_ng3.JPG)

Memeriksa `sumParent` dari node 91. Karena `==query`, `check=true`, dan pada akhirnya nilai yang dikembalikan adalah `true`. Oleh karena demikian, keluarkan "Penjumlahan angka di tree yang menghasilkan 266 ditemukan".

Output:
```
Penjumlahan angka di tree yang menghasilkan 266 ditemukan
```

## Genjil Ganap V2
### Verdict
AC saat revisi
#### Bukti
![verdict_ggv2](/img/verdict_ggv2.jpg)
### Penjelasan Soal
Diminta untuk membuat tree yang hanya berisi bilangan genap dari input yang diberikan, dengan ketentuan jika ada input bilangan ganjil, bilangan genap yang diinput sebelumnya akan dihapus dari tree.
### Penjelasan Solusi
Masalah Genjil Ganap V2 diselesaikan dengan struktur data `binary search tree` dan `array`. BST digunakan sebagai ADT utama untuk menyimpan data, sedangkan array digunakan untuk mengingat urutan masuknya data sehingga dapat digunakan sebagai referensi untuk menghapus bilangan genap yang terakhir dimasukkan.
Jika tree kosong (`bst_isEmpty(&set)`), dikeluarkan `Tree Kosong!`.Jika tidak, dikeluarkan anggota tree dengan traversal `inorder`.

__Implementasi:__

Diinisialisasi sebuah `binary search tree` bernama `set`, variabel `N`. Program menerima input yang disimpan dalam variabel `N`, yaitu banyak bilangan yang akan dicoba untuk dimasukkan ke `tree`. Lalu, dibuat array integer sebesar `N` yang bernama `N` dan integer `size` (diinisialisasi bernilai `0`). `size` berfungsi untuk mempermudah mengetahui banyak anggota tree sehingga untuk menginput dan mengakses data pada `arr` lebih mudah.
Setelah itu, dilakukan loop `for` sebanyak `N` kali. Isi dari loop adalah:

Pertama, input bilangan dan disimpan dalam variabel `num`.

Kedua, periksa apakah input genap (`if(num%2==0)`).
- Jika iya, nilai input dimasukkan ke `set`, simpan nilainya di `arr[size]`, size di*increment*.
- Jika tidak, jika `set` tidak kosong, hapus (`remove`) nilai yang terdapat pada `array[size-1]` dari `set`. Lalu, `size` di*decrement*.

Saat loop sudah selesai dijalankan, diperiksa apakah tree kosong atau tidak. Jika kosong, dikeluarkan `Tree Kosong!`. Jika tidak, keluarkan anggota `tree` secara traversal `inorder`.

### Visualisasi Solusi
Untuk mempermudah visualisasi solusi dari permasalahan Genjil Ganap V2, digunakan sample input berikut:

```
7
6
4
5
8
7
12
10
```

![ggv2_1](/img/si_ggv2_1.JPG)

Inisialisasi tree dan definisi array berukuran `7`.

![ggv2_2a](/img/si_ggv2_2a.JPG)
![ggv2_2b](/img/si_ggv2_2b.JPG)

Input 6 genap, masuk ke tree dan array, dan `size++`. Sama juga dengan input 4.

![ggv2_3](/img/si_ggv2_3.JPG)

Input 5 ganjil, nilai 4 di`remove` dari tree dan array. `size--`

![ggv2_4](/img/si_ggv2_4.JPG)

Input 8 genap, masuk ke tree dan array, dan `size++`.

![ggv2_5](/img/si_ggv2_5.JPG)

Input 7 ganjil, nilai 8 di`remove` dari tree dan array. `size--`

![ggv2_6a](/img/si_ggv2_6a.JPG)
![ggv2_6b](/img/si_ggv2_6b.JPG)

Input 12 genap, masuk ke tree dan array, dan `size++`
Input 10 genap, masuk ke tree dan array, dan `size++`

![ggv2_7](/img/si_ggv2_7.gif)

Agar anggota tree dikeluarkan secara terurut dari nilai terendah ke tertinggi, gunakan traversal `inorder` dengan fungsi `printf`.

Output:
```
6 10 12 
```