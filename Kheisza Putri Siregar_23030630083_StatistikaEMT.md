﻿# Kheisza Putri Siregar_23030630083_StatistikaEMT
Nama: Kheisza Putri Siregar


NIM: 23030630083


Kelas: Matematika B


# EMT untuk Statistika

Dalam buku catatan ini, kami mendemonstrasikan plot statistik utama,
tes, dan distribusi dalam Euler.


Mari kita mulai dengan beberapa statistik deskriptif. Ini bukanlah
sebuah pengantar statistik. Jadi, Anda mungkin memerlukan latar
belakang untuk memahami detailnya.


Asumsikan pengukuran berikut ini. Kita ingin menghitung nilai
rata-rata dan deviasi standar yang diukur.


\>M=[1000,1004,998,997,1002,1001,998,1004,998,997]; ...  
\>   median(M), mean(M), dev(M),


    999
    999.9
    2.72641400622

We can plot the box-and-whiskers plot for the data. In our case there
are no outliers.


\>aspect(1.75); boxplot(M):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-001.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-001.png)

Kami menghitung probabilitas bahwa suatu nilai lebih besar dari 1005,
dengan mengasumsikan nilai yang diukur dari distribusi normal.


Semua fungsi untuk distribusi dalam Euler diakhiri dengan ...dis dan
menghitung distribusi probabilitas kumulatif (CPF).


$$\text{normaldis(x,m,d)}=\int_{-\infty}^x \frac{1}{d\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{t-m}{d})^2}\ dt.$$Kami mencetak hasilnya dalam % dengan akurasi 2 digit menggunakan
fungsi cetak.


\>print((1-normaldis(1005,mean(M),dev(M)))\*100,2,unit=" %")


          3.07 %

Untuk contoh berikutnya, kami mengasumsikan jumlah pria berikut ini
dalam rentang ukuran tertentu.


\>r=155.5:4:187.5; v=[22,71,136,169,139,71,32,8];


Berikut ini adalah plot distribusinya.


\>plot2d(r,v,a=150,b=200,c=0,d=190,bar=1,style="\\/"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-003.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-003.png)

Kita dapat memasukkan data mentah tersebut ke dalam tabel.


Tabel adalah sebuah metode untuk menyimpan data statistik. Tabel kita
harus berisi tiga kolom: Awal rentang, akhir rentang, jumlah orang
dalam rentang.


Tabel dapat dicetak dengan header. Kami menggunakan vektor string
untuk mengatur header.


\>T:=r[1:8]' | r[2:9]' | v'; writetable(T,labc=["BB","BA","Frek"])


            BB        BA      Frek
         155.5     159.5        22
         159.5     163.5        71
         163.5     167.5       136
         167.5     171.5       169
         171.5     175.5       139
         175.5     179.5        71
         179.5     183.5        32
         183.5     187.5         8

Jika kita membutuhkan nilai rata-rata dan statistik lain dari ukuran,
kita perlu menghitung titik tengah rentang. Kita dapat menggunakan dua
kolom pertama dari tabel kita untuk hal ini.


Sumbol “|” digunakan untuk memisahkan kolom, fungsi “writetable”
digunakan untuk menulis tabel, dengan opsi “labc” untuk menentukan
judul kolom.


\>(T[,1]+T[,2])/2 // the midpoint of each interval


            157.5 
            161.5 
            165.5 
            169.5 
            173.5 
            177.5 
            181.5 
            185.5 

Tetapi akan lebih mudah, untuk melipat rentang dengan vektor
[1/2,1/2].


\>M=fold(r,[0.5,0.5])


    [157.5,  161.5,  165.5,  169.5,  173.5,  177.5,  181.5,  185.5]

Sekarang kita dapat menghitung rata-rata dan deviasi sampel dengan
frekuensi yang diberikan.


\>{m,d}=meandev(M,v); m, d,


    169.901234568
    5.98912964449

Mari kita tambahkan distribusi normal dari nilai-nilai tersebut ke
dalam diagram batang di atas. Rumus untuk distribusi normal dengan
rata-rata m dan deviasi standar d adalah:


$$y=\frac{1}{d\sqrt{2\pi}}e^{\frac{-(x-m)^2}{2d^2}}.$$Karena nilainya antara 0 dan 1, untuk memplotnya pada plot batang,
nilai tersebut harus dikalikan dengan 4 kali jumlah data.


\>plot2d("qnormal(x,m,d)\*sum(v)\*4", ...  
\>     xmin=min(r),xmax=max(r),thickness=3,add=1):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-005.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-005.png)

# Tabel

Dalam direktori buku catatan ini, Anda akan menemukan file dengan
tabel. Data tersebut mewakili hasil survei. Berikut adalah empat baris
pertama dari file tersebut. Data berasal dari buku online berbahasa
Jerman “Einführung in die Statistik mit R” oleh A. Handl.


\>printfile("table.dat",4);


    Person Sex Age Titanic Evaluation Tip Problem
    1 m 30 n . 1.80 n
    2 f 23 y g 1.80 n
    3 f 26 y g 1.80 y

Tabel berisi 7 kolom angka atau token (string). Kita ingin membaca
tabel tersebut dari file. Pertama, kita menggunakan terjemahan kita
sendiri untuk token-token tersebut.


Untuk itu, kita mendefinisikan set token. Fungsi strtokens()
mendapatkan vektor string token dari string yang diberikan.


\>mf:=["m","f"]; yn:=["y","n"]; ev:=strtokens("g vg m b vb");


Sekarang kita membaca tabel dengan terjemahan ini.


Argumen tok2, tok4, dan lain-lain adalah terjemahan dari kolom-kolom
tabel. Argumen-argumen ini tidak ada dalam daftar parameter
readtable(), jadi Anda perlu memberikannya dengan “:=”.


\>{MT,hd}=readtable("table.dat",tok2:=mf,tok4:=yn,tok5:=ev,tok7:=yn);

\>load over statistics;


Untuk mencetak, kita perlu menentukan set token yang sama. Kami
mencetak empat baris pertama saja.


\>writetable(MT[1:10],labc=hd,wc=5,tok2:=mf,tok4:=yn,tok5:=ev,tok7:=yn);


     Person  Sex  Age Titanic Evaluation  Tip Problem
          1    m   30       n          .  1.8       n
          2    f   23       y          g  1.8       n
          3    f   26       y          g  1.8       y
          4    m   33       n          .  2.8       n
          5    m   37       n          .  1.8       n
          6    m   28       y          g  2.8       y
          7    f   31       y         vg  2.8       n
          8    m   23       n          .  0.8       n
          9    f   24       y         vg  1.8       y
         10    m   26       n          .  1.8       n

Tanda titik “.” mewakili nilai yang tidak tersedia.


Jika kita tidak ingin menentukan token untuk terjemahan sebelumnya,
kita hanya perlu menentukan kolom mana yang berisi token dan bukan
angka.


\>ctok=[2,4,5,7]; {MT,hd,tok}=readtable("table.dat",ctok=ctok);


Fungsi readtable() sekarang mengembalikan satu set token.


\>tok


    m
    n
    f
    y
    g
    vg

Tabel berisi entri dari file dengan token yang diterjemahkan ke angka.


String khusus NA=“.” ditafsirkan sebagai “Tidak Tersedia”, dan
mendapatkan NAN (bukan angka) dalam tabel. Terjemahan ini dapat diubah
dengan parameter NA, dan NAval.


\>MT[1]


    [1,  1,  30,  2,  NAN,  1.8,  2]

Berikut ini adalah isi tabel dengan angka yang tidak diterjemahkan.


\>writetable(MT,wc=5)


        1    1   30    2    .  1.8    2
        2    3   23    4    5  1.8    2
        3    3   26    4    5  1.8    4
        4    1   33    2    .  2.8    2
        5    1   37    2    .  1.8    2
        6    1   28    4    5  2.8    4
        7    3   31    4    6  2.8    2
        8    1   23    2    .  0.8    2
        9    3   24    4    6  1.8    4
       10    1   26    2    .  1.8    2
       11    3   23    4    6  1.8    4
       12    1   32    4    5  1.8    2
       13    1   29    4    6  1.8    4
       14    3   25    4    5  1.8    4
       15    3   31    4    5  0.8    2
       16    1   26    4    5  2.8    2
       17    1   37    2    .  3.8    2
       18    1   38    4    5    .    2
       19    3   29    2    .  3.8    2
       20    3   28    4    6  1.8    2
       21    3   28    4    1  2.8    4
       22    3   28    4    6  1.8    4
       23    3   38    4    5  2.8    2
       24    3   27    4    1  1.8    4
       25    1   27    2    .  2.8    4

Untuk kenyamanan, Anda dapat menaruh output dari readtable() ke dalam
sebuah daftar.


\>Table={{readtable("table.dat",ctok=ctok)}};


Dengan menggunakan kolom token yang sama dan token yang dibaca dari
file, kita dapat mencetak tabel. Kita dapat menentukan ctok, tok, dll.
atau menggunakan daftar Tabel.


\>writetable(Table,ctok=ctok,wc=5);


     Person  Sex  Age Titanic Evaluation  Tip Problem
          1    m   30       n          .  1.8       n
          2    f   23       y          g  1.8       n
          3    f   26       y          g  1.8       y
          4    m   33       n          .  2.8       n
          5    m   37       n          .  1.8       n
          6    m   28       y          g  2.8       y
          7    f   31       y         vg  2.8       n
          8    m   23       n          .  0.8       n
          9    f   24       y         vg  1.8       y
         10    m   26       n          .  1.8       n
         11    f   23       y         vg  1.8       y
         12    m   32       y          g  1.8       n
         13    m   29       y         vg  1.8       y
         14    f   25       y          g  1.8       y
         15    f   31       y          g  0.8       n
         16    m   26       y          g  2.8       n
         17    m   37       n          .  3.8       n
         18    m   38       y          g    .       n
         19    f   29       n          .  3.8       n
         20    f   28       y         vg  1.8       n
         21    f   28       y          m  2.8       y
         22    f   28       y         vg  1.8       y
         23    f   38       y          g  2.8       n
         24    f   27       y          m  1.8       y
         25    m   27       n          .  2.8       y

Fungsi tablecol() mengembalikan nilai kolom dari tabel, melewatkan
setiap baris dengan nilai NAN (“.” dalam file), dan indeks kolom, yang
berisi nilai-nilai ini.


\>{c,i}=tablecol(MT,[5,6]);


Kita dapat menggunakan ini untuk mengekstrak kolom dari tabel untuk
tabel baru.


\>j=[1,5,6]; writetable(MT[i,j],labc=hd[j],ctok=[2],tok=tok)


        Person Evaluation       Tip
             2          g       1.8
             3          g       1.8
             6          g       2.8
             7         vg       2.8
             9         vg       1.8
            11         vg       1.8
            12          g       1.8
            13         vg       1.8
            14          g       1.8
            15          g       0.8
            16          g       2.8
            20         vg       1.8
            21          m       2.8
            22         vg       1.8
            23          g       2.8
            24          m       1.8

Tentu saja, kita perlu mengekstrak tabel itu sendiri dari daftar Tabel
dalam kasus ini.


\>MT=Table[1];


Tentu saja, kita juga dapat menggunakannya untuk menentukan nilai
rata-rata kolom atau nilai statistik lainnya.


\>mean(tablecol(MT,6))


    2.175

The getstatistics() function returns the elements in a vector, and their counts. We apply it
to the "m" and "f" values in the second column of our table.


\>{xu,count}=getstatistics(tablecol(MT,2)); xu, count,


    [1,  3]
    [12,  13]

We can print the result in a new table.


\>writetable(count',labr=tok[xu])


             m        12
             f        13

The function selecttable() returns a new table with the values in one column selected from a
vector of indices. First we look up the indices of two of our values in the token table.


\>v:=indexof(tok,["g","vg"])


    [5,  6]

Now we can select the rows of the table, which have any of the values in v in their 5-th row.


\>MT1:=MT[selectrows(MT,5,v)]; i:=sortedrows(MT1,5);


Now we can print the table, with extracted and sorted values in the 5-th column.


\>writetable(MT1[i],labc=hd,ctok=ctok,tok=tok,wc=7);


    Index 3 in string vector out of bounds!
    Try "trace errors" to inspect local variables after errors.
    writetable:
        wc[j]=max(wc[j],strlen(""+labc[j])+1);

For the next statistic, we want to relate two columns of the table. So we extract column 2 and
4 and sort the table.


\>i=sortedrows(MT,[2,4]);  ...  
\>     writetable(tablecol(MT[i],[2,4])',ctok=[1,2],tok=tok)


             m         n
             m         n
             m         n
             m         n
             m         n
             m         n
             m         n
             m         y
             m         y
             m         y
             m         y
             m         y
             f         n
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y
             f         y

With getstatistics(), we can also relate the counts in two columns of the table to each other.


\>MT24=tablecol(MT,[2,4]); ...  
\>   {xu1,xu2,count}=getstatistics(MT24[1],MT24[2]); ...  
\>   writetable(count,labr=tok[xu1],labc=tok[xu2])


                       n         y
             m         7         5
             f         1        12

A table can be written to a file.


\>filename="test.dat"; ...  
\>   writetable(count,labr=tok[xu1],labc=tok[xu2],file=filename);


Then we can read the table from the file.


\>{MT2,hd,tok2,hdr}=readtable(filename,\>clabs,\>rlabs); ...  
\>   writetable(MT2,labr=hdr,labc=hd)


                       n         y
             m         7         5
             f         1        12

And delete the file.


\>fileremove(filename);


# Distribusi

Dengan plot2d, ada metode yang sangat mudah untuk memplot distribusi
data eksperimen.


\>p=normal(1,1000); //1000 random normal-distributed sample p

\>plot2d(p,distribution=20,style="\\/"); // plot the random sample p

\>plot2d("qnormal(x,0,1)",add=1): // add the standard normal distribution plot


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-006.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-006.png)

Perhatikan perbedaan antara plot batang (sampel) dan kurva normal
(distribusi sesungguhnya). Masukkan kembali ketiga perintah tersebut
untuk melihat hasil pengambilan sampel yang lain.


Berikut ini adalah perbandingan 10 simulasi dari 1000 nilai
terdistribusi normal dengan menggunakan apa yang disebut plot kotak.
Plot ini menunjukkan median, kuartil 25% dan 75%, nilai minimal dan
maksimal, serta pencilan.


\>p=normal(10,1000); boxplot(p):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-007.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-007.png)

Untuk menghasilkan bilangan bulat acak, Euler memiliki intrandom. Mari
kita simulasikan pelemparan dadu dan memplot distribusinya.


Kita menggunakan fungsi getmultiplicities(v,x), yang menghitung
seberapa sering elemen-elemen dari v muncul di dalam x. Kemudian kita
memplot hasilnya menggunakan columnsplot().


\>k=intrandom(1,6000,6);  ...  
\>   columnsplot(getmultiplicities(1:6,k));  ...  
\>   ygrid(1000,color=red):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-008.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-008.png)

Meskipun intrandom(n,m,k) menghasilkan bilangan bulat yang
terdistribusi secara seragam dari 1 sampai k, adalah mungkin untuk
menggunakan distribusi bilangan bulat yang lain dengan randpint().


Pada contoh berikut, probabilitas untuk 1,2,3 adalah 0.4, 0.1, 0.5
secara berurutan.


\>randpint(1,1000,[0.4,0.1,0.5]); getmultiplicities(1:3,%)


    [378,  102,  520]

Euler dapat menghasilkan nilai acak dari lebih banyak distribusi.
Lihatlah ke dalam referensi.


Misalnya, kita mencoba distribusi eksponensial. Sebuah variabel acak
kontinu X dikatakan memiliki distribusi eksponensial, jika PDF-nya
diberikan oleh


$$f_X(x)=\lambda e^{-\lambda x},\quad x>0,\quad \lambda>0,$$

dengan parameter


$$\lambda=\frac{1}{\mu},\quad \mu \text{ is the mean, and denoted by } X \sim \text{Exponential}(\lambda).$$\>plot2d(randexponential(1,1000,2),\>distribution):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-011.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-011.png)

Untuk banyak distribusi, Euler dapat menghitung fungsi distribusi dan
kebalikannya.


\>plot2d("normaldis",-4,4): 


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-012.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-012.png)

Berikut ini adalah salah satu cara untuk memplot kuantil.


\>plot2d("qnormal(x,1,1.5)",-4,6);  ...  
\>   plot2d("qnormal(x,1,1.5)",a=2,b=5,\>add,\>filled):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-013.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-013.png)

$$\text{normaldis(x,m,d)}=\int_{-\infty}^x \frac{1}{d\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{t-m}{d})^2}\ dt.$$

Probabilitas untuk berada di area hijau adalah sebagai berikut.


\>normaldis(5,1,1.5)-normaldis(2,1,1.5)


    0.248662156979

Hal ini dapat dihitung secara numerik dengan integral berikut ini.


$$\int_2^5 \frac{1}{1.5\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{x-1}{1.5})^2}\ dx.$$\>gauss("qnormal(x,1,1.5)",2,5)


    0.248662156979

Mari kita bandingkan distribusi binomial dengan distribusi normal
dengan rata-rata dan deviasi yang sama. Fungsi invbindis()
menyelesaikan interpolasi linier antara nilai bilangan bulat.


\>invbindis(0.95,1000,0.5), invnormaldis(0.95,500,0.5\*sqrt(1000))


    525.516721219
    526.007419394

Fungsi qdis() adalah densitas dari distribusi chi-square. Seperti
biasa, Euler memetakan vektor ke fungsi ini. Dengan demikian kita
mendapatkan plot semua distribusi chi-kuadrat dengan derajat 5 hingga
30 dengan mudah dengan cara berikut.


\>plot2d("qchidis(x,(5:5:50)')",0,50):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-016.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-016.png)

Euler memiliki fungsi-fungsi yang akurat untuk mengevaluasi
distribusi-distribusi. Mari kita periksa chidis() dengan sebuah
integral.


Penamaannya diusahakan untuk konsisten. 


Sebagai contoh,


* 
distribusi chi-kuadrat adalah chidis(),


 fungsi kebalikannya adalah invchidis(),  

 densitasnya adalah qchidis().  

Pelengkap dari distribusi (ekor atas) adalah chicdis().


\>chidis(1.5,2), integrate("qchidis(x,2)",0,1.5)


    0.527633447259
    0.527633447259

# Distribusi Diskrit

Untuk menentukan distribusi diskrit Anda sendiri, Anda dapat
menggunakan metode berikut.


Pertama, kita tetapkan fungsi distribusinya.


\>wd = 0|((1:6)+[-0.01,0.01,0,0,0,0])/6


    [0,  0.165,  0.335,  0.5,  0.666667,  0.833333,  1]

Artinya, dengan probabilitas wd[i+1]-wd[i] kita menghasilkan nilai
acak i.


Ini hampir merupakan distribusi yang seragam. Mari kita definisikan
sebuah generator bilangan acak untuk ini. Fungsi find(v,x) menemukan
nilai x dalam vektor v. Fungsi ini juga dapat digunakan untuk vektor
x.


\>function wrongdice (n,m) := find(wd,random(n,m))


Kesalahan ini sangat halus sehingga kita hanya bisa melihatnya setelah
melakukan iterasi yang sangat banyak.


\>columnsplot(getmultiplicities(1:6,wrongdice(1,1000000))):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-017.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-017.png)

Berikut ini adalah fungsi sederhana untuk memeriksa distribusi seragam
dari nilai 1... K dalam v. Kami menerima hasilnya, jika untuk semua
frekuensi


$$\left|f_i-\frac{1}{K}\right| < \frac{\delta}{\sqrt{n}}.$$\>function checkrandom (v, delta=1) ...


      K=max(v); n=cols(v);
      fr=getfrequencies(v,1:K);
      return max(fr/n-1/K)<delta/sqrt(n);
      endfunction
</pre>
Memang fungsi ini menolak distribusi seragam.


\>checkrandom(wrongdice(1,1000000))


    0

Dan ini menerima generator acak bawaan.


\>checkrandom(intrandom(1,1000000,6))


    1

Kita dapat menghitung distribusi binomial. Pertama, ada binomialsum(),
yang mengembalikan probabilitas i atau kurang dari n percobaan.


\>bindis(410,1000,0.4)


    0.751401349654

Fungsi Beta invers digunakan untuk menghitung interval kepercayaan
Clopper-Pearson untuk parameter p. Tingkat defaultnya adalah alpha.


Arti dari interval ini adalah jika p berada di luar interval, hasil
yang diamati sebesar 410 dalam 1000 jarang terjadi.


\>clopperpearson(410,1000)


    [0.37932,  0.441212]

Perintah berikut ini adalah cara langsung untuk mendapatkan hasil di
atas. Tetapi untuk n yang besar, penjumlahan langsung tidak akurat dan
lambat.


\>p=0.4; i=0:410; n=1000; sum(bin(n,i)\*p^i\*(1-p)^(n-i))


    0.751401349655

Omong-omong, invbinsum() menghitung kebalikan dari binomialsum().


\>invbindis(0.75,1000,0.4)


    409.932733047

Dalam Bridge, kita mengasumsikan 5 kartu yang terbuka (dari 52 kartu)
di dua tangan (26 kartu). Mari kita hitung probabilitas distribusi
yang lebih buruk dari 3:2 (misalnya 0:5, 1:4, 4:1, atau 5:0).


\>2\*hypergeomsum(1,5,13,26)


    0.321739130435

Ada juga simulasi distribusi multinomial.


\>randmultinomial(10,1000,[0.4,0.1,0.5])


              381           100           519 
              376            91           533 
              417            80           503 
              440            94           466 
              406           112           482 
              408            94           498 
              395           107           498 
              399            96           505 
              428            87           485 
              400            99           501 

# Memplot Data

Untuk memplot data, kami mencoba hasil pemilihan umum Jerman sejak
tahun 1990, yang diukur dalam kursi.


\>BW := [ ...  
\>   1990,662,319,239,79,8,17; ...  
\>   1994,672,294,252,47,49,30; ...  
\>   1998,669,245,298,43,47,36; ...  
\>   2002,603,248,251,47,55,2; ...  
\>   2005,614,226,222,61,51,54; ...  
\>   2009,622,239,146,93,68,76; ...  
\>   2013,631,311,193,0,63,64];


Untuk pesta, kami menggunakan serangkaian nama.


\>P:=["CDU/CSU","SPD","FDP","Gr","Li"];


Mari kita cetak persentasenya dengan baik.


Pertama kita ekstrak kolom-kolom yang diperlukan. Kolom 3 sampai 7
adalah kursi masing-masing partai, dan kolom 2 adalah jumlah total
kursi. kolom adalah tahun pemilihan.


\>BT:=BW[,3:7]; BT:=BT/sum(BT); YT:=BW[,1]';


Kemudian kita mencetak statistik dalam bentuk tabel. Kita menggunakan
nama sebagai judul kolom, dan tahun sebagai judul baris. Lebar default
untuk kolom adalah wc = 10, tetapi kami lebih suka output yang lebih
padat. Kolom-kolom akan diperluas untuk label-label kolom, jika perlu.


\>writetable(BT\*100,wc=6,dc=0,\>fixed,labc=P,labr=YT)


           CDU/CSU   SPD   FDP    Gr    Li
      1990      48    36    12     1     3
      1994      44    38     7     7     4
      1998      37    45     6     7     5
      2002      41    42     8     9     0
      2005      37    36    10     8     9
      2009      38    23    15    11    12
      2013      49    31     0    10    10

Perkalian matriks berikut ini mengekstrak jumlah persentase dua partai
besar yang menunjukkan bahwa partai-partai kecil telah memperoleh
suara di parlemen hingga tahun 2009.


\>BT1:=(BT.[1;1;0;0;0])'\*100


    [84.29,  81.25,  81.1659,  82.7529,  72.9642,  61.8971,  79.8732]

Ada juga plot statistik sederhana. Kita menggunakannya untuk
menampilkan garis dan titik secara bersamaan. Alternatif lainnya
adalah memanggil plot2d dua kali dengan &gt;add.


\>statplot(YT,BT1,"b"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-019.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-019.png)

Tentukan beberapa warna untuk masing-masing pihak.


\>CP:=[rgb(0.5,0.5,0.5),red,yellow,green,rgb(0.8,0,0)];


Sekarang kita dapat memplot hasil pemilu 2009 dan perubahannya ke
dalam satu plot menggunakan figure. Kita dapat menambahkan vektor
kolom pada setiap plot.


\>figure(2,1);  ...  
\>   figure(1); columnsplot(BW[6,3:7],P,color=CP); ...  
\>   figure(2); columnsplot(BW[6,3:7]-BW[5,3:7],P,color=CP);  ...  
\>   figure(0):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-020.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-020.png)

Plot data menggabungkan baris data statistik dalam satu plot.


\>J:=BW[,1]'; DP:=BW[,3:7]'; ...  
\>   dataplot(YT,BT',color=CP);  ...  
\>   labelbox(P,colors=CP,styles="[]",\>points,w=0.2,x=0.3,y=0.4):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-021.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-021.png)

Plot kolom 3D menunjukkan deretan data statistik dalam bentuk kolom.
Kami menyediakan label untuk baris dan kolom. angle adalah sudut
pandang.


\>columnsplot3d(BT,scols=P,srows=YT, ...  
\>     angle=30°,ccols=CP):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-022.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-022.png)

Representasi lainnya adalah plot mosaik. Perhatikan bahwa kolom-kolom
pada plot mewakili kolom-kolom pada matriks di sini. Karena panjangnya
label CDU/CSU, kita mengambil jendela yang lebih kecil dari biasanya.


\>shrinkwindow(\>smaller);  ...  
\>   mosaicplot(BT',srows=YT,scols=P,color=CP,style="#"); ...  
\>   shrinkwindow():


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-023.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-023.png)

We can also do a pie chart. Since black and yellow form a coalition, we reorder the elements.


\>i=[1,3,5,4,2]; piechart(BW[6,3:7][i],color=CP[i],lab=P[i]):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-024.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-024.png)

Berikut ini jenis plot yang lain.


\>starplot(normal(1,10)+4,lab=1:10,\>rays):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-025.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-025.png)

Beberapa plot di plot2d bagus untuk statika. Berikut ini adalah plot
impuls dari data acak, yang terdistribusi secara seragam dalam [0,1].


\>plot2d(makeimpulse(1:10,random(1,10)),\>bar):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-026.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-026.png)

Tetapi untuk data yang terdistribusi secara eksponensial, kita mungkin
memerlukan plot logaritmik.


\>logimpulseplot(1:10,-log(random(1,10))\*10):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-027.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-027.png)

Fungsi columnsplot() lebih mudah digunakan, karena hanya membutuhkan
sebuah vektor nilai. Selain itu, fungsi ini dapat mengatur labelnya
menjadi apa pun yang kita inginkan, kita telah mendemonstrasikan hal
ini dalam tutorial ini.


Berikut ini adalah aplikasi lain, di mana kita menghitung karakter
dalam sebuah kalimat dan memplot statistik.


\>v=strtochar("the quick brown fox jumps over the lazy dog"); ...  
\>   w=ascii("a"):ascii("z"); x=getmultiplicities(w,v); ...  
\>   cw=[]; for k=w; cw=cw|char(k); end; ...  
\>   columnsplot(x,lab=cw,width=0.05):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-028.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-028.png)

Anda juga dapat menetapkan sumbu secara manual.


\>n=10; p=0.4; i=0:n; x=bin(n,i)\*p^i\*(1-p)^(n-i); ...  
\>   columnsplot(x,lab=i,width=0.05,<frame,<grid); ...  
\>   yaxis(0,0:0.1:1,style="-\>",\>left); xaxis(0,style="."); ...  
\>   label("p",0,0.25), label("i",11,0); ...  
\>   textbox(["Binomial distribution","with p=0.4"]):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-029.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-029.png)

Berikut ini adalah cara untuk memplot frekuensi angka dalam vektor.


Kami membuat vektor angka acak bilangan bulat 1 hingga 6.


\>v:=intrandom(1,10,10)


    [8,  5,  8,  8,  6,  8,  8,  3,  5,  5]

Then extract the unique numbers in v.


\>vu:=unique(v)


    [3,  5,  6,  8]

And plot the frequencies in a columns plot.


\>columnsplot(getmultiplicities(vu,v),lab=vu,style="/"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-030.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-030.png)

We want to demonstrate functions for the empirical distribution of values.


\>x=normal(1,20);


The function empdist(x,vs) needs a sorted array of values. So we have to sort x before we can
use it.


\>xs=sort(x);


Then we plot the empirical distribution and some density bars into one plot. Instead of a bar
plot for the distribution we use a sawtooth plot this time.


\>figure(2,1); ...  
\>   figure(1); plot2d("empdist",-4,4;xs); ...  
\>   figure(2); plot2d(histo(x,v=-4:0.2:4,<bar));  ...  
\>   figure(0):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-031.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-031.png)

A scatter plot is easy to do in Euler with the usual point plot. The
following graph shows that the X and X+Y are clearly positively
correlated.


\>x=normal(1,100); plot2d(x,x+rotright(x),\>points,style=".."):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-032.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-032.png)

Often, we wish to compare two samples of different distributions. This can be done with a
quantile-quantile-plot.


For a test, we try the student-t distribution and exponential distribution.


\>x=randt(1,1000,5); y=randnormal(1,1000,mean(x),dev(x)); ...  
\>   plot2d("x",r=6,style="--",yl="normal",xl="student-t",\>vertical); ...  
\>   plot2d(sort(x),sort(y),\>points,color=red,style="x",\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-033.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-033.png)

The plot clearly shows that the normal distributed values tend to be smaller at the extreme
ends.


If we have two distributions of different size, we can expand the smaller one or shrink the
larger one. The following function is good for both. It takes the median values with
percentages between 0 and 1.


\>function medianexpand (x,n) := median(x,p=linspace(0,1,n-1));


Let us compare two equal distributions.


\>x=random(1000); y=random(400); ...  
\>   plot2d("x",0,1,style="--"); ...  
\>   plot2d(sort(medianexpand(x,400)),sort(y),\>points,color=red,style="x",\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-034.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-034.png)

# Regresi dan Korelasi

Regresi linier dapat dilakukan dengan fungsi polyfit() atau berbagai
fungsi kecocokan.


Sebagai permulaan, kita mencari garis regresi untuk data univariat
dengan polyfit(x,y,1).


\>x=1:10; y=[2,3,1,5,6,3,7,8,9,8]; writetable(x'|y',labc=["x","y"])


             x         y
             1         2
             2         3
             3         1
             4         5
             5         6
             6         3
             7         7
             8         8
             9         9
            10         8

Kami ingin membandingkan kecocokan tanpa bobot dan dengan bobot.
Pertama, koefisien dari kecocokan linier.


\>p=polyfit(x,y,1)


    [0.733333,  0.812121]

Now the coefficients with a weight that emphasizes the last values.


\>w &= "exp(-(x-10)^2/10)"; pw=polyfit(x,y,1,w=w(x))


    [4.71566,  0.38319]

We put everything into one plot for the points and the regression lines, and for the weights
used.


\>figure(2,1);  ...  
\>   figure(1); statplot(x,y,"b",xl="Regression"); ...  
\>     plot2d("evalpoly(x,p)",\>add,color=blue,style="--"); ...  
\>     plot2d("evalpoly(x,pw)",5,10,\>add,color=red,style="--"); ...  
\>   figure(2); plot2d(w,1,10,\>filled,style="/",fillcolor=red,xl=w); ...  
\>   figure(0):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-035.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-035.png)

ntuk contoh lain, kita membaca survei tentang siswa, usia mereka, usia
orang tua mereka, dan jumlah saudara kandung dari sebuah file.


Tabel ini berisi “m” dan “f” pada kolom kedua. Kita menggunakan
variabel tok2 untuk mengatur terjemahan yang tepat dan bukannya
membiarkan readtable() mengumpulkan terjemahan.


\>{MS,hd}:=readtable("table1.dat",tok2:=["m","f"]);  ...  
\>   writetable(MS,labc=hd,tok2:=["m","f"]);


        Person       Sex       Age    Mother    Father  Siblings
             1         m        29        58        61         1
             2         f        26        53        54         2
             3         m        24        49        55         1
             4         f        25        56        63         3
             5         f        25        49        53         0
             6         f        23        55        55         2
             7         m        23        48        54         2
             8         m        27        56        58         1
             9         m        25        57        59         1
            10         m        24        50        54         1
            11         f        26        61        65         1
            12         m        24        50        52         1
            13         m        29        54        56         1
            14         m        28        48        51         2
            15         f        23        52        52         1
            16         m        24        45        57         1
            17         f        24        59        63         0
            18         f        23        52        55         1
            19         m        24        54        61         2
            20         f        23        54        55         1

How do the ages depend on each other? A first impression comes from a pairwise scatterplot.


\>scatterplots(tablecol(MS,3:5),hd[3:5]):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-036.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-036.png)

Jelas bahwa usia ayah dan ibu saling bergantung satu sama lain. Mari
kita tentukan dan plot garis regresinya.


\>cs:=MS[,4:5]'; ps:=polyfit(cs[1],cs[2],1)


    [17.3789,  0.740964]

Ini jelas merupakan model yang salah. Garis regresinya adalah s = 17 +
0,74t, di mana t adalah usia ibu dan s adalah usia ayah. Perbedaan
usia mungkin sedikit bergantung pada usia, tetapi tidak terlalu
banyak.


Sebaliknya, kita menduga fungsi seperti s = a + t. Kemudian a adalah
rata-rata dari s-t. Ini adalah perbedaan usia rata-rata antara ayah
dan ibu.


\>da:=mean(cs[2]-cs[1])


    3.65

Mari kita plotkan ini ke dalam satu scatter plot.


\>plot2d(cs[1],cs[2],\>points);  ...  
\>   plot2d("evalpoly(x,ps)",color=red,style=".",\>add);  ...  
\>   plot2d("x+da",color=blue,\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-037.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-037.png)

Berikut ini adalah plot kotak dari kedua usia tersebut. Ini hanya
menunjukkan, bahwa usia keduanya berbeda.


\>boxplot(cs,["mothers","fathers"]):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-038.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-038.png)

Sangat menarik bahwa perbedaan dalam median tidak sebesar perbedaan
dalam mean.


\>median(cs[2])-median(cs[1])


    1.5

Koefisien korelasi menunjukkan korelasi positif.


\>correl(cs[1],cs[2])


    0.7588307236

Korelasi peringkat adalah ukuran untuk urutan yang sama dalam kedua
vektor. Korelasi ini juga cukup positif.


\>rankcorrel(cs[1],cs[2])


    0.758925292358

# Membuat Fungsi baru

Tentu saja, bahasa EMT dapat digunakan untuk memprogram fungsi baru.
Misalnya, kita mendefinisikan fungsi kemiringan.


$$\text{sk}(x) = \dfrac{\sqrt{n} \sum_i (x_i-m)^3}{\left(\sum_i (x_i-m)^2\right)^{3/2}}$$di mana m adalah rata-rata dari x.


\>function skew (x:vector) ...


    m=mean(x);
    return sqrt(cols(x))*sum((x-m)^3)/(sum((x-m)^2))^(3/2);
    endfunction
</pre>
Seperti yang Anda lihat, kita dapat dengan mudah menggunakan bahasa
matriks untuk mendapatkan implementasi yang sangat singkat dan
efisien. Mari kita coba fungsi ini.


\>data=normal(20); skew(normal(10))


    -0.198710316203

Berikut ini adalah fungsi lain, yang disebut koefisien kemencengan
Pearson.


\>function skew1 (x) := 3\*(mean(x)-median(x))/dev(x)

\>skew1(data)


    -0.0801873249135

# Simulasi Monte Carlo

Euler dapat digunakan untuk mensimulasikan kejadian acak. Kita telah
melihat contoh sederhana di atas. Berikut ini adalah contoh lainnya,
yang mensimulasikan 1000 kali pelemparan 3 dadu, dan menanyakan
distribusi dari jumlah tersebut.


\>ds:=sum(intrandom(1000,3,6))';  fs=getmultiplicities(3:18,ds)


    [5,  17,  35,  44,  75,  97,  114,  116,  143,  116,  104,  53,  40,
    22,  13,  6]

We can plot this now.


\>columnsplot(fs,lab=3:18):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-040.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-040.png)

Untuk menentukan distribusi yang diharapkan tidaklah mudah. Kami
menggunakan rekursi tingkat lanjut untuk hal ini.


Fungsi berikut ini menghitung jumlah cara angka k dapat
direpresentasikan sebagai jumlah n angka dalam rentang 1 hingga m.
Fungsi ini bekerja secara rekursif dengan cara yang jelas.


\>function map countways (k; n, m) ...


      if n==1 then return k>=1 && k<=m
      else
        sum=0; 
        loop 1 to m; sum=sum+countways(k-#,n-1,m); end;
        return sum;
      end;
    endfunction
</pre>
Berikut ini adalah hasil dari tiga lemparan dadu.


\>countways(5:25,5,5)


    [1,  5,  15,  35,  70,  121,  185,  255,  320,  365,  381,  365,  320,
    255,  185,  121,  70,  35,  15,  5,  1]

\>cw=countways(3:18,3,6)


    [1,  3,  6,  10,  15,  21,  25,  27,  27,  25,  21,  15,  10,  6,  3,
    1]

We add the expected values to the plot.


\>plot2d(cw/6^3\*1000,\>add); plot2d(cw/6^3\*1000,\>points,\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-041.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-041.png)

Untuk simulasi lainnya, deviasi nilai rata-rata dari n variabel acak
berdistribusi normal 0-1 adalah 1/sqrt(n).


\>longformat; 1/sqrt(10)


    0.316227766017

Mari kita periksa hal ini dengan sebuah simulasi. Kami menghasilkan
10.000 kali 10 vektor acak.


\>M=normal(10000,10); dev(mean(M)')


    0.319493614817

\>plot2d(mean(M)',\>distribution):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-042.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-042.png)

The median of 10 0-1-normal distributed random numbers has a larger deviation.


\>dev(median(M)')


    0.374460271535

Karena kita dapat dengan mudah menghasilkan jalan acak, kita dapat
mensimulasikan proses Wiener. Kami mengambil 1000 langkah dari 1000
proses. Kami kemudian memplot deviasi standar dan rata-rata dari
langkah ke-n dari proses-proses ini bersama dengan nilai yang
diharapkan dalam warna merah.


\>n=1000; m=1000; M=cumsum(normal(n,m)/sqrt(m)); ...  
\>   t=(1:n)/n; figure(2,1); ...  
\>   figure(1); plot2d(t,mean(M')'); plot2d(t,0,color=red,\>add); ...  
\>   figure(2); plot2d(t,dev(M')'); plot2d(t,sqrt(t),color=red,\>add); ...  
\>   figure(0):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-043.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-043.png)

# Tests

Tests adalah alat yang penting dalam statistik. Dalam Euler, banyak
tes yang diterapkan. Semua tes ini mengembalikan kesalahan yang kita
terima jika kita menolak hipotesis nol.


Sebagai contoh, kita menguji lemparan dadu untuk distribusi yang
seragam. Pada 600 lemparan, kita mendapatkan nilai berikut, yang kita
masukkan ke dalam uji chi-kuadrat.


\>chitest([90,103,114,101,103,89],dup(100,6)')


    0.498830517952

Uji chi-square juga memiliki mode, yang menggunakan simulasi Monte
Carlo untuk menguji statistik. Hasilnya seharusnya hampir sama.
Parameter &gt;p menginterpretasikan vektor y sebagai vektor probabilitas.


\>chitest([90,103,114,101,103,89],dup(1/6,6)',\>p,\>montecarlo)


    0.52

Kesalahan ini terlalu besar. Jadi kita tidak bisa menolak distribusi
seragam. Ini tidak membuktikan bahwa dadu kita adil. Tetapi kita tidak
dapat menolak hipotesis kita.


Selanjutnya kita buat 1000 lemparan dadu dengan menggunakan generator
bilangan acak, dan lakukan pengujian yang sama.


\>n=1000; t=random([1,n\*6]); chitest(count(t\*6,6),dup(n,6)')


    0.160135893909

Let us test for the mean value 100 with the t-test.


\>s=200+normal([1,100])\*10; ...  
\>   ttest(mean(s),dev(s),100,200)


    0.0793042036132

Fungsi ttest() membutuhkan nilai rata-rata, deviasi, jumlah data, dan
nilai rata-rata untuk diuji.


Sekarang mari kita periksa dua pengukuran untuk mean yang sama. Kita
tolak hipotesis bahwa kedua pengukuran tersebut memiliki nilai
rata-rata yang sama, jika hasilnya &lt; 0,05.


\>tcomparedata(normal(1,10),normal(1,10))


    0.361546001568

If we add a bias to one distribution, we get more rejections. Repeat this simulation several
times to see the effect.


\>tcomparedata(normal(1,10),normal(1,10)+2)


    0.00114271563128

Pada contoh berikut, kita membuat 20 lemparan dadu secara acak
sebanyak 100 kali dan menghitung jumlah dadu yang muncul. Rata-rata
harus ada 20/6 = 3,3 mata dadu.


\>R=random(100,20); R=sum(R\*6<=1)'; mean(R)


    3.3

Sekarang kita bandingkan jumlah satu dengan distribusi binomial.
Pertama, kita memplot distribusi angka satu.


\>plot2d(R,distribution=max(R)+1,even=1,style="\\/"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-044.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-044.png)

\>t=count(R,21);


Kemudian kami menghitung nilai yang diharapkan.


\>n=0:20; b=bin(20,n)\*(1/6)^n\*(5/6)^(20-n)\*100;


Kami harus mengumpulkan beberapa angka untuk mendapatkan kategori yang
cukup besar.


\>t1=sum(t[1:2])|t[3:7]|sum(t[8:21]); ...  
\>   b1=sum(b[1:2])|b[3:7]|sum(b[8:21]);


Uji chi-square menolak hipotesis bahwa distribusi kita adalah
distribusi binomial, jika hasilnya &lt;0,05.


\>chitest(t1,b1)


    0.746082959869

Contoh berikut ini berisi hasil dari dua kelompok orang (laki-laki dan
perempuan, katakanlah) yang memberikan suara untuk satu dari enam
partai.


\>A=[23,37,43,52,64,74;27,39,41,49,63,76];  ...  
\>     writetable(A,wc=6,labr=["m","f"],labc=1:6)


               1     2     3     4     5     6
         m    23    37    43    52    64    74
         f    27    39    41    49    63    76

Kami ingin menguji independensi suara dari jenis kelamin. Uji tabel
chi^2 melakukan hal ini. Hasilnya terlalu besar untuk menolak
independensi. Jadi kita tidak dapat mengatakan, jika pemungutan suara
tergantung pada jenis kelamin dari data ini.


\>tabletest(A)


    0.990701632326

Berikut ini adalah tabel yang diharapkan, jika kita mengasumsikan
frekuensi pemungutan suara yang diamati.


\>writetable(expectedtable(A),wc=6,dc=1,labr=["m","f"],labc=1:6)


               1     2     3     4     5     6
         m  24.9  37.9  41.9  50.3  63.3  74.7
         f  25.1  38.1  42.1  50.7  63.7  75.3

We can compute the corrected contingency coefficient. Since it very close to 0, we conclude
that the voting does not depend on the sex.


\>contingency(A)


    0.0427225484717

# Beberapa Tes Lainnya

Selanjutnya kita menggunakan analisis varians (uji F) untuk menguji
tiga sampel data yang terdistribusi secara normal dengan nilai
rata-rata yang sama. Metode ini disebut ANOVA (analisis varians).
Dalam Euler, fungsi varanalysis() digunakan.


\>x1=[109,111,98,119,91,118,109,99,115,109,94]; mean(x1),


    106.545454545

\>x2=[120,124,115,139,114,110,113,120,117]; mean(x2),


    119.111111111

\>x3=[120,112,115,110,105,134,105,130,121,111]; mean(x3)


    116.3

\>varanalysis(x1,x2,x3)


    0.0138048221371

Ini berarti, kami menolak hipotesis nilai rata-rata yang sama. Kami
melakukan ini dengan probabilitas kesalahan sebesar 1,3%.


Ada juga uji median, yang menolak sampel data dengan distribusi
rata-rata yang berbeda dengan menguji median dari sampel gabungan.


\>a=[56,66,68,49,61,53,45,58,54];

\>b=[72,81,51,73,69,78,59,67,65,71,68,71];

\>mediantest(a,b)


    0.0241724220052

Another test on equality is the rank test. It is much sharper than the median test.


\>ranktest(a,b)


    0.00199969612469

In the following example, both distributions have the same mean.


\>ranktest(random(1,100),random(1,50)\*3-1)


    0.0498282994473

Sekarang mari kita coba mensimulasikan dua perawatan a dan b yang
diterapkan pada orang yang berbeda.


\>a=[8.0,7.4,5.9,9.4,8.6,8.2,7.6,8.1,6.2,8.9];

\>b=[6.8,7.1,6.8,8.3,7.9,7.2,7.4,6.8,6.8,8.1];


Uji signum memutuskan, apakah a lebih baik daripada b.


\>signtest(a,b)


    0.0546875

Ini adalah kesalahan yang terlalu besar. Kita tidak dapat menolak
bahwa a sama baiknya dengan b.


Uji Wilcoxon lebih tajam daripada uji ini, tetapi bergantung pada
nilai kuantitatif dari perbedaan.


\>wilcoxon(a,b)


    0.0296680599405

Mari kita coba dua pengujian lagi dengan menggunakan rangkaian yang
dihasilkan.


\>wilcoxon(normal(1,20),normal(1,20)-1)


    0.00286699547515

\>wilcoxon(normal(1,20),normal(1,20))


    0.396919522771

# Bilangan Acak

Berikut ini adalah tes untuk generator bilangan acak. Euler
menggunakan generator yang sangat bagus, jadi kita tidak perlu
mengharapkan adanya masalah.


Pertama, kita akan membangkitkan sepuluh juta bilangan acak dalam
[0,1].


\>n:=10000000; r:=random(1,n);


Selanjutnya, kami menghitung jarak antara dua angka yang kurang dari
0,05.


\>a:=0.05; d:=differences(nonzeros(r<a));


Terakhir, kami memplot berapa kali, setiap jarak yang terjadi, dan
membandingkannya dengan nilai yang diharapkan.


\>m=getmultiplicities(1:100,d); plot2d(m); ...  
\>     plot2d("n\*(1-a)^(x-1)\*a^2",color=red,\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-045.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-045.png)

Bersihkan data


\>remvalue n;


# Pengantar untuk Pengguna Proyek R

Jelas, EMT tidak bersaing dengan R sebagai sebuah paket statistik.
Namun, ada banyak prosedur dan fungsi statistik yang tersedia di EMT
juga. Jadi EMT dapat memenuhi kebutuhan dasar. Bagaimanapun, EMT hadir
dengan paket numerik dan sistem aljabar komputer.


Buku ini diperuntukkan bagi Anda yang sudah terbiasa dengan R, tetapi
perlu mengetahui perbedaan sintaks EMT dan R. Kami mencoba memberikan
gambaran umum mengenai hal-hal yang jelas dan kurang jelas yang perlu
Anda ketahui.


Selain itu, kami juga membahas cara-cara untuk bertukar data di antara
kedua sistem tersebut.


Perhatikan bahwa ini adalah pekerjaan yang sedang berlangsung.


# Sintaks Dasar

Hal pertama yang Anda pelajari dalam R adalah membuat sebuah vektor.
Dalam EMT, perbedaan utamanya adalah operator : dapat mengambil ukuran
langkah. Selain itu, operator ini memiliki daya ikat yang rendah.


\>n=10; 0:n/20:n-1


    [0,  0.5,  1,  1.5,  2,  2.5,  3,  3.5,  4,  4.5,  5,  5.5,  6,  6.5,
    7,  7.5,  8,  8.5,  9]

Fungsi c() tidak ada. Anda dapat menggunakan vektor untuk
menggabungkan beberapa hal.


Contoh berikut ini, seperti banyak contoh lainnya, berasal dari
“Interoduksi ke R” yang disertakan dengan proyek R. Jika Anda membaca
PDF ini, Anda akan menemukan bahwa saya mengikuti alurnya dalam
tutorial ini.


\>x=[10.4, 5.6, 3.1, 6.4, 21.7]; [x,0,x]


    [10.4,  5.6,  3.1,  6.4,  21.7,  0,  10.4,  5.6,  3.1,  6.4,  21.7]

Operator titik dua dengan ukuran langkah EMT digantikan oleh fungsi
seq() dalam R. Kita dapat menulis fungsi ini dalam EMT.


\>function seq(a,b,c) := a:b:c; ...  
\>   seq(0,-0.1,-1)


    [0,  -0.1,  -0.2,  -0.3,  -0.4,  -0.5,  -0.6,  -0.7,  -0.8,  -0.9,  -1]

Fungsi rep() dari R tidak ada dalam EMT. Untuk input vektor, dapat
dituliskan sebagai berikut.


\>function rep(x:vector,n:index) := flatten(dup(x,n)); ...  
\>   rep(x,2)


    [10.4,  5.6,  3.1,  6.4,  21.7,  10.4,  5.6,  3.1,  6.4,  21.7]

Perhatikan bahwa “=” atau “:=” digunakan untuk penugasan. Operator
“-&gt;” digunakan untuk unit dalam EMT.


\>125km -\> " miles"


    77.6713990297 miles

Operator “&lt;-” untuk penugasan menyesatkan, dan bukan ide yang baik
untuk R. Berikut ini akan membandingkan a dan -4 dalam EMT.


\>a=2; a<-4


    0

Dalam R, “a&lt;-4&lt;3” bisa digunakan, tetapi “a&lt;-4&lt;-3” tidak. Saya juga
mengalami ambiguitas yang sama di EMT, tetapi saya mencoba untuk
menghilangkannya.


EMT dan R memiliki vektor dengan tipe boolean. Tetapi dalam EMT, angka
0 dan 1 digunakan untuk merepresentasikan salah dan benar. Dalam R,
nilai benar dan salah tetap dapat digunakan dalam aritmatika biasa
seperti dalam EMT.


\>x<5, %\*x


    [0,  0,  1,  0,  0]
    [0,  0,  3.1,  0,  0]

EMT melempar kesalahan atau menghasilkan NAN tergantung pada flag
“errors”.


\>errors off; 0/0, isNAN(sqrt(-1)), errors on;


    NAN
    1

String sama saja dalam R dan EMT. Keduanya berada di lokal saat ini,
bukan di Unicode.


Dalam R ada paket-paket untuk Unicode. Dalam EMT, sebuah string dapat
berupa string Unicode. Sebuah string Unicode dapat diterjemahkan ke
pengkodean lokal dan sebaliknya. Selain itu, u“...” dapat berisi
entitas HTML.


\>u"&#169; Ren&eacut; Grothmann"


    © René Grothmann

Berikut ini mungkin atau mungkin tidak ditampilkan dengan benar pada
sistem Anda sebagai A dengan titik dan tanda hubung di atasnya. Hal
ini tergantung pada jenis huruf yang Anda gunakan.


\>chartoutf([480])


    Ǡ

Penggabungan string dilakukan dengan “+” atau “|”. Ini dapat
menyertakan angka, yang akan dicetak dalam format saat ini.


\>"pi = "+pi


    pi = 3.14159265359

# Pengindeksan

Sebagian besar waktu, ini akan bekerja seperti pada R.


Tetapi EMT akan menginterpretasikan indeks negatif dari bagian
belakang vektor, sementara R menginterpretasikan x[n] sebagai x tanpa
elemen ke-n.


\>x, x[1:3], x[-2]


    [10.4,  5.6,  3.1,  6.4,  21.7]
    [10.4,  5.6,  3.1]
    6.4

Perilaku R dapat dicapai dalam EMT dengan drop().


\>drop(x,2)


    [10.4,  3.1,  6.4,  21.7]

Vektor logika tidak diperlakukan secara berbeda dengan indeks di EMT,
berbeda dengan R. Anda harus mengekstrak elemen-elemen yang bukan nol
terlebih dahulu di EMT.


\>x, x\>5, x[nonzeros(x\>5)]


    [10.4,  5.6,  3.1,  6.4,  21.7]
    [1,  1,  0,  1,  1]
    [10.4,  5.6,  6.4,  21.7]

Just as in R, the index vector can contain repetitions.


\>x[[1,2,2,1]]


    [10.4,  5.6,  5.6,  10.4]

BNamun pemberian nama untuk indeks tidak dimungkinkan dalam EMT. Untuk
paket statistik, hal ini mungkin sering diperlukan untuk memudahkan
akses ke elemen-elemen vektor.


Untuk meniru perilaku ini, kita dapat mendefinisikan sebuah fungsi
sebagai berikut.


\>function sel (v,i,s) := v[indexof(s,i)]; ...  
\>   s=["first","second","third","fourth"]; sel(x,["first","third"],s)


    
    Trying to overwrite protected function sel!
    Error in:
    function sel (v,i,s) := v[indexof(s,i)]; ... ...
                 ^
    [10.4,  3.1]

# Tipe Data

EMT memiliki lebih banyak tipe data yang tetap dibandingkan R. Jelas,
dalam R terdapat vektor yang berkembang. Anda bisa mengatur sebuah
vektor numerik kosong v dan memberikan sebuah nilai pada elemen v[17].
Hal ini tidak mungkin dilakukan dalam EMT.


Hal berikut ini sedikit tidak efisien.


\>v=[]; for i=1 to 10000; v=v|i; end;


EMT sekarang akan membuat vektor dengan v dan i yang ditambahkan pada
tumpukan dan menyalin vektor tersebut kembali ke variabel global v.


Semakin efisien mendefinisikan vektor terlebih dahulu.


\>v=zeros(10000); for i=1 to 10000; v[i]=i; end;


Untuk mengubah jenis tanggal di EMT, Anda dapat menggunakan fungsi
seperti complex().


\>complex(1:4)


    [ 1+0i ,  2+0i ,  3+0i ,  4+0i  ]

Konversi ke string hanya dapat dilakukan untuk tipe data dasar. Format
saat ini digunakan untuk penggabungan string sederhana. Tetapi ada
fungsi-fungsi seperti print() atau frac().


Untuk vektor, Anda dapat dengan mudah menulis fungsi Anda sendiri.


\>function tostr (v) ...


    s="[";
    loop 1 to length(v);
       s=s+print(v[#],2,0);
       if #<length(v) then s=s+","; endif;
    end;
    return s+"]";
    endfunction
</pre>
\>tostr(linspace(0,1,10))


    [0.00,0.10,0.20,0.30,0.40,0.50,0.60,0.70,0.80,0.90,1.00]

Untuk komunikasi dengan Maxima, ada sebuah fungsi convertmxm(), yang
juga dapat digunakan untuk memformat vektor untuk output.


\>convertmxm(1:10)


    [1,2,3,4,5,6,7,8,9,10]

Untuk Latex, perintah tex dapat digunakan untuk mendapatkan perintah
Latex.


\>tex(&[1,2,3])


    \left[ 1 , 2 , 3 \right] 

# Faktor dan Tabel

Pada pengantar R terdapat sebuah contoh dengan apa yang disebut
faktor.


Berikut ini adalah daftar wilayah dari 30 negara bagian.


\>austates = ["tas", "sa", "qld", "nsw", "nsw", "nt", "wa", "wa", ...  
\>   "qld", "vic", "nsw", "vic", "qld", "qld", "sa", "tas", ...  
\>   "sa", "nt", "wa", "vic", "qld", "nsw", "nsw", "wa", ...  
\>   "sa", "act", "nsw", "vic", "vic", "act"];


Asumsikan, kita memiliki pendapatan yang sesuai di setiap negara
bagian.


\>incomes = [60, 49, 40, 61, 64, 60, 59, 54, 62, 69, 70, 42, 56, ...  
\>   61, 61, 61, 58, 51, 48, 65, 49, 49, 41, 48, 52, 46, ...  
\>   59, 46, 58, 43];


Sekarang, kita ingin menghitung rata-rata pendapatan di wilayah
tersebut. Sebagai sebuah program statistik, R memiliki fungsi factor()
dan tappy() untuk hal ini.


EMT dapat melakukan hal ini dengan mencari indeks dari wilayah-wilayah
di dalam daftar unik dari wilayah-wilayah tersebut.


\>auterr=sort(unique(austates)); f=indexofsorted(auterr,austates)


    [6,  5,  4,  2,  2,  3,  8,  8,  4,  7,  2,  7,  4,  4,  5,  6,  5,  3,
    8,  7,  4,  2,  2,  8,  5,  1,  2,  7,  7,  1]

Pada titik ini, kita dapat menulis fungsi perulangan kita sendiri
untuk melakukan berbagai hal untuk satu faktor saja.


Atau kita dapat meniru fungsi tapply() dengan cara berikut.


\>function map tappl (i; f$:call, cat, x) ...


    u=sort(unique(cat));
    f=indexof(u,cat);
    return f$(x[nonzeros(f==indexof(u,i))]);
    endfunction
</pre>
Ini sedikit tidak efisien, karena menghitung wilayah unik untuk setiap
i, tetapi berfungsi.


\>tappl(auterr,"mean",austates,incomes)


    [44.5,  57.333,  55.5,  53.6,  55,  60.5,  56,  52.25]

Note that it works for each vector of territories.


\>tappl(["act","nsw"],"mean",austates,incomes)


    [44.5,  57.333]

Sekarang, paket statistik EMT mendefinisikan tabel seperti halnya di
R. Fungsi readtable() dan writetable() dapat digunakan untuk input dan
output.


Jadi kita dapat mencetak rata-rata pendapatan negara di wilayah dengan
cara yang ramah.


\>writetable(tappl(auterr,"mean",austates,incomes),labc=auterr,wc=7)


        act    nsw     nt    qld     sa    tas    vic     wa
       44.5  57.33   55.5   53.6     55   60.5     56  52.25

Kita juga dapat mencoba meniru perilaku R sepenuhnya.


Faktor-faktor tersebut harus disimpan dengan jelas dalam sebuah
koleksi dengan jenis dan kategorinya (negara bagian dan wilayah dalam
contoh kita). Untuk EMT, kita menambahkan indeks yang telah dihitung
sebelumnya.


\>function makef (t) ...


    ## Factor data
    ## Returns a collection with data t, unique data, indices.
    ## See: tapply
    u=sort(unique(t));
    return {{t,u,indexofsorted(u,t)}};
    endfunction
</pre>
\>statef=makef(austates);


Sekarang elemen ketiga dari koleksi ini akan berisi indeks.


\>statef[3]


    [6,  5,  4,  2,  2,  3,  8,  8,  4,  7,  2,  7,  4,  4,  5,  6,  5,  3,
    8,  7,  4,  2,  2,  8,  5,  1,  2,  7,  7,  1]

Sekarang kita dapat meniru tapply() dengan cara berikut. Ini akan
mengembalikan sebuah tabel sebagai kumpulan data tabel dan judul
kolom.


\>function tapply (t:vector,tf,f$:call) ...


    ## Makes a table of data and factors
    ## tf : output of makef()
    ## See: makef
    uf=tf[2]; f=tf[3]; x=zeros(length(uf));
    for i=1 to length(uf);
       ind=nonzeros(f==i);
       if length(ind)==0 then x[i]=NAN;
       else x[i]=f$(t[ind]);
       endif;
    end;
    return {{x,uf}};
    endfunction
</pre>
Kami tidak menambahkan banyak pemeriksaan tipe di sini. Satu-satunya
tindakan pencegahan adalah kategori (faktor) yang tidak memiliki data.
Tetapi kita harus memeriksa panjang t yang benar dan kebenaran koleksi
tf.


Tabel ini bisa dicetak sebagai sebuah tabel dengan writetable().


\>writetable(tapply(incomes,statef,"mean"),wc=7)


        act    nsw     nt    qld     sa    tas    vic     wa
       44.5  57.33   55.5   53.6     55   60.5     56  52.25

# Array

EMT hanya memiliki dua dimensi untuk array. Tipe datanya disebut
matriks. Akan lebih mudah untuk menulis fungsi untuk dimensi yang
lebih tinggi atau sebuah pustaka C untuk ini.


R memiliki lebih dari dua dimensi. Dalam R, larik adalah sebuah vektor
dengan sebuah bidang dimensi.


Dalam EMT, sebuah vektor adalah sebuah matriks dengan satu baris. Ini
bisa dibuat menjadi sebuah matriks dengan redim().


\>shortformat; X=redim(1:20,4,5)


            1         2         3         4         5 
            6         7         8         9        10 
           11        12        13        14        15 
           16        17        18        19        20 

Ekstraksi baris dan kolom, atau sub-matriks, sama seperti di R.


\>X[,2:3]


            2         3 
            7         8 
           12        13 
           17        18 

Namun, dalam R dimungkinkan untuk mengatur daftar indeks tertentu dari
vektor ke suatu nilai. Hal yang sama juga dapat dilakukan dalam EMT
hanya dengan sebuah perulangan.


\>function setmatrixvalue (M, i, j, v) ...


    loop 1 to max(length(i),length(j),length(v))
       M[i{#},j{#}] = v{#};
    end;
    endfunction
</pre>
Kami mendemonstrasikan hal ini untuk menunjukkan bahwa matriks
dilewatkan dengan referensi di EMT. Jika Anda tidak ingin mengubah
matriks asli M, Anda perlu menyalinnya dalam fungsi.


\>setmatrixvalue(X,1:3,3:-1:1,0); X,


            1         2         0         4         5 
            6         0         8         9        10 
            0        12        13        14        15 
           16        17        18        19        20 

Hasil kali luar dalam EMT hanya dapat dilakukan di antara vektor. Hal
ini otomatis karena bahasa matriks. Satu vektor harus berupa vektor
kolom dan vektor baris.


\>(1:5)\*(1:5)'


            1         2         3         4         5 
            2         4         6         8        10 
            3         6         9        12        15 
            4         8        12        16        20 
            5        10        15        20        25 

Dalam pengantar PDF untuk R ada sebuah contoh, yang menghitung
distribusi ab-cd untuk a, b, c, d yang dipilih dari 0 sampai n secara
acak. Solusinya dalam R adalah membentuk sebuah matriks 4 dimensi dan
menjalankan table() di atasnya.


Tentu saja, ini bisa dicapai dengan sebuah perulangan. Tetapi
perulangan tidak efektif dalam EMT atau R. Dalam EMT, kita bisa
menulis perulangan dalam C dan itu adalah solusi tercepat.


Tetapi kita ingin meniru perilaku R. Untuk ini, kita perlu meratakan
perkalian ab dan membuat sebuah matriks ab-cd.


\>a=0:6; b=a'; p=flatten(a\*b); q=flatten(p-p'); ...  
\>   u=sort(unique(q)); f=getmultiplicities(u,q); ...  
\>   statplot(u,f,"h"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-046.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-046.png)

Selain kelipatan yang tepat, EMT dapat menghitung frekuensi dalam
vektor.


\>getfrequencies(q,-50:10:50)


    [0,  23,  132,  316,  602,  801,  333,  141,  53,  0]

Cara yang paling mudah untuk memplot ini sebagai distribusi adalah
sebagai berikut.


\>plot2d(q,distribution=11):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-047.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-047.png)

Tetapi juga memungkinkan untuk menghitung jumlah dalam interval yang
dipilih sebelumnya. Tentu saja, berikut ini menggunakan
getfrequencies() secara internal.


Karena fungsi histo() mengembalikan frekuensi, kita perlu
menskalakannya sehingga integral di bawah grafik batang adalah 1.


\>{x,y}=histo(q,v=-55:10:55); y=y/sum(y)/differences(x); ...  
\>   plot2d(x,y,\>bar,style="/"):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-048.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-048.png)

# Lists

EMT memiliki dua jenis daftar. Yang pertama adalah daftar global yang
dapat diubah, dan yang kedua adalah jenis daftar yang tidak dapat
diubah. Kita tidak akan membahas mengenai daftar global di sini.


Tipe daftar yang tidak dapat diubah disebut koleksi dalam EMT. Ia
berperilaku seperti struktur dalam C, tetapi elemen-elemennya hanya
diberi nomor dan tidak diberi nama.


\>L={{"Fred","Flintstone",40,[1990,1992]}}


    Fred
    Flintstone
    40
    [1990,  1992]

Saat ini elemen-elemen tersebut tidak memiliki nama, meskipun nama
dapat ditetapkan untuk tujuan khusus. Elemen-elemen tersebut diakses
dengan angka.


\>(L[4])[2]


    1992

# Masukan dan Keluaran File (Membaca dan Menulis Data)

Anda mungkin sering ingin mengimpor matriks data dari sumber lain ke
EMT. Tutorial ini memberitahu Anda tentang banyak cara untuk mencapai
hal ini. Fungsi yang sederhana adalah writematrix() dan readmatrix().


Mari kita tunjukkan bagaimana cara membaca dan menulis sebuah vektor
real ke sebuah file.


\>a=random(1,100); mean(a), dev(a),


    0.51451
    0.29368

Untuk menulis data ke sebuah berkas, kita menggunakan fungsi
writematrix().


Karena pengenalan ini kemungkinan besar berada di sebuah direktori, di
mana pengguna tidak memiliki akses tulis, kita menulis data ke
direktori home pengguna. Untuk notebook sendiri, hal ini tidak
diperlukan, karena file data akan ditulis ke dalam direktori yang
sama.


\>filename="test.dat";


Sekarang kita tuliskan vektor kolom a' ke dalam file. Hal ini akan
menghasilkan satu angka pada setiap baris file.


\>writematrix(a',filename);


Untuk membaca data, kita menggunakan readmatrix().


\>a=readmatrix(filename)';


Hapus filenya


\>fileremove(filename);

\>mean(a), dev(a),


    0.51451
    0.29368

Fungsi writematrix() atau writetable() dapat dikonfigurasi untuk
bahasa lain.


Sebagai contoh, jika Anda memiliki sistem bahasa Indonesia (titik
desimal dengan koma), Excel Anda membutuhkan nilai dengan koma desimal
yang dipisahkan oleh titik koma dalam file csv (defaultnya adalah
nilai yang dipisahkan dengan koma). File “test.csv” berikut ini akan
muncul di folder cuurent Anda.


\>filename="test.csv"; ...  
\>   writematrix(random(5,3),file=filename,separator=",");


You can now open this file with an Indonesian Excel directly.


\>fileremove(filename);


Terkadang kita memiliki string dengan token seperti berikut ini.


\>s1:="f m m f m m m f f f m m f";  ...  
\>   s2:="f f f m m f f";


Untuk menandai ini, kita mendefinisikan vektor token.


\>tok:=["f","m"]


    f
    m

Kemudian kita dapat menghitung berapa kali setiap token muncul dalam
string, dan memasukkan hasilnya ke dalam tabel.


\>M:=getmultiplicities(tok,strtokens(s1))\_ ...  
\>     getmultiplicities(tok,strtokens(s2));


Tulis tabel dengan tajuk token.


\>writetable(M,labc=tok,labr=1:2,wc=8)


                   f       m
           1       6       7
           2       5       2

Untuk statika, EMT dapat membaca dan menulis tabel.


\>file="test.dat"; open(file,"w"); ...  
\>   writeln("A,B,C"); writematrix(random(3,3)); ...  
\>   close();


File terlihat seperti ini.


\>printfile(file)


    A,B,C
    0.5494906682086674,0.4167953824752977,0.2970084493261381
    0.4952211725031024,0.6784877142270355,0.237954824336879
    0.3591665502307735,0.9364528230384411,0.6858407278994386
    

Fungsi readtable() dalam bentuknya yang paling sederhana dapat membaca
ini dan mengembalikan sebuah koleksi nilai dan baris judul.


\>L=readtable(file,\>list);


Koleksi ini dapat dicetak dengan writetable() ke buku catatan, atau ke
sebuah file.


\>writetable(L,wc=10,dc=5)


             A         B         C
       0.54949    0.4168   0.29701
       0.49522   0.67849   0.23795
       0.35917   0.93645   0.68584

Matriks nilai adalah elemen pertama dari L. Perhatikan bahwa mean()
dalam EMT menghitung nilai rata-rata dari baris-baris matriks.


\>mean(L[1])


       0.4211 
      0.47055 
      0.66049 

# File CSV

Pertama, mari kita tulis sebuah matriks ke dalam sebuah file. Untuk
keluarannya, kami membuat file di direktori kerja saat ini.


\>file="test.csv";  ...  
\>   M=random(3,3); writematrix(M,file);


Here is the content of this file.


\>printfile(file)


    0.3831674338175132,0.6450784461720231,0.2279854958778094
    0.7978112873686288,0.7659446308879617,0.9419559906545371
    0.9777802108374042,0.6893762706764894,0.1059938953302247
    

CVS ini dapat dibuka di sistem bahasa Inggris ke Excel dengan klik dua
kali. Jika Anda mendapatkan file seperti itu pada sistem Jerman, Anda
perlu mengimpor data ke Excel dengan memperhatikan titik desimal.


Namun, titik desimal juga merupakan format default untuk EMT. Anda
dapat membaca sebuah matriks dari sebuah file dengan readmatrix()..


\>readmatrix(file)


      0.38317         0   0.64508         0   0.22799 
      0.79781         0   0.76594         0   0.94196 
      0.97778         0   0.68938         0   0.10599 

Dimungkinkan untuk menulis beberapa matriks ke dalam satu file.
Perintah open() dapat membuka file untuk menulis dengan parameter “w”.
Standarnya adalah “r” untuk membaca.


\>open(file,"w"); writematrix(M); writematrix(M'); close();


Matriks-matriks tersebut dipisahkan oleh sebuah baris kosong. Untuk
membaca matriks, buka file dan panggil readmatrix() beberapa kali.


\>open(file); A=readmatrix(); B=readmatrix(); A==B, close();


            1         1         0         1         0 
            0         1         1         1         0 
            0         1         0         1         1 

Di Excel atau spreadsheet serupa, Anda dapat mengekspor matriks
sebagai CSV (nilai yang dipisahkan dengan koma). Pada Excel 2007,
gunakan “simpan sebagai” dan “format lain”, lalu pilih “CSV”.
Pastikan, tabel saat ini hanya berisi data yang ingin Anda ekspor.


Berikut ini adalah contohnya.


\>printfile("excel-data.csv")


    Could not open the file
    excel-data.csv
    for reading!
    Try "trace errors" to inspect local variables after errors.
    printfile:
        open(filename,"r");

Seperti yang Anda lihat, sistem Jerman saya menggunakan titik koma
sebagai pemisah dan koma desimal. Anda dapat mengubahnya di pengaturan
sistem atau di Excel, tetapi tidak perlu untuk membaca matriks ke
dalam EMT.


Cara termudah untuk membaca ini ke dalam Euler adalah readmatrix().
Semua koma digantikan oleh titik dengan parameter &gt;comma. Untuk CSV
bahasa Inggris, hilangkan saja parameter ini.


\>M=readmatrix("excel-data.csv",\>comma)


    Could not open the file
    excel-data.csv
    for reading!
    Try "trace errors" to inspect local variables after errors.
    readmatrix:
        if filename&lt;&gt;"" then open(filename,"r"); endif;

Mari kita plot


\>plot2d(M'[1],M'[2:3],\>points,color=[red,green]'):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-049.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-049.png)

Ada beberapa cara yang lebih mendasar untuk membaca data dari file.
Anda dapat membuka file dan membaca angka baris demi baris. Fungsi
getvectorline() akan membaca angka dari sebuah baris data. Secara
default, fungsi ini mengharapkan sebuah titik desimal. Tetapi fungsi
ini juga dapat menggunakan koma desimal, jika Anda memanggil
setdecimaldot(“,”) sebelum menggunakan fungsi ini.


Fungsi berikut ini adalah contohnya. Fungsi ini akan berhenti pada
akhir file atau baris kosong.


\>function myload (file) ...


    open(file);
    M=[];
    repeat
       until eof();
       v=getvectorline(3);
       if length(v)>0 then M=M_v; else break; endif;
    end;
    return M;
    close(file);
    endfunction
</pre>
\>myload(file)


      0.38317         0   0.64508         0   0.22799 
      0.79781         0   0.76594         0   0.94196 
      0.97778         0   0.68938         0   0.10599 

Anda juga dapat membaca semua angka dalam file tersebut dengan
getvector().


\>open(file); v=getvector(10000); close(); redim(v[1:9],3,3)


      0.38317         0   0.64508 
            0   0.22799   0.79781 
            0   0.76594         0 

Dengan demikian, sangat mudah untuk menyimpan vektor nilai, satu nilai
di setiap baris dan membaca kembali vektor ini.


\>v=random(1000); mean(v)


    0.4996

\>writematrix(v',file); mean(readmatrix(file)')


    0.4996

# Menggunakan Tabel

Tabel dapat digunakan untuk membaca atau menulis data numerik. Sebagai
contoh, kita menulis tabel dengan judul baris dan kolom ke file.


\>file="test.tab"; M=random(3,3);  ...  
\>   open(file,"w");  ...  
\>   writetable(M,separator=",",labc=["one","two","three"]);  ...  
\>   close(); ...  
\>   printfile(file)


    one,two,three
           0.8,      0.88,       0.3
          0.55,      0.34,      0.93
          0.29,      0.79,      0.08

File ini dapat diimpor ke Excel.


Untuk membaca file di EMT, kita menggunakan readtable().


\>{M,headings}=readtable(file,\>clabs); ...  
\>   writetable(M,labc=headings)


           one       two     three
           0.8      0.88       0.3
          0.55      0.34      0.93
          0.29      0.79      0.08

# Menganalisis Garis

Anda bahkan dapat mengevaluasi setiap baris dengan tangan.
Misalkan,kita memiliki baris dengan format berikut.


\>line="2020-11-03,Tue,1'114.05"


    2020-11-03,Tue,1'114.05

Pertama, kita dapat memberi tanda pada garis tersebut.


\>vt=strtokens(line)


    2020-11-03
    Tue
    1'114.05

Kemudian, kita dapat mengevaluasi setiap elemen garis dengan
menggunakan evaluasi yang sesuai.


\>day(vt[1]),  ...  
\>   indexof(["mon","tue","wed","thu","fri","sat","sun"],tolower(vt[2])),  ...  
\>   strrepl(vt[3],"'","")()


    7.3816e+05
    2
    1114

Dengan menggunakan ekspresi reguler, Anda dapat mengekstrak hampir
semua informasi dari sebuah baris data.


Anggaplah kita memiliki baris dokumen HTML berikut ini.


\>line="<tr\><td\>1145.45</td\><td\>5.6</td\><td\>-4.5</td\><tr\>"


    &lt;tr&gt;&lt;td&gt;1145.45&lt;/td&gt;&lt;td&gt;5.6&lt;/td&gt;&lt;td&gt;-4.5&lt;/td&gt;&lt;tr&gt;

Untuk mengekstrak ini, kita menggunakan ekspresi reguler, yang mencari


* 
tanda kurung tutup &gt;,


* 
string apa pun yang tidak mengandung tanda kurung dengan
* sub-pencocokan “(...)”,


* 
kurung pembuka dan kurung penutup menggunakan solusi terpendek,


* 
sekali lagi, semua string yang tidak mengandung tanda kurung,


* 
dan sebuah kurung pembuka &lt;.


Ekspresi reguler agak sulit untuk dipelajari tetapi sangat kuat.


\>{pos,s,vt}=strxfind(line,"\>([^<\>]+)<.+?\>([^<\>]+)<");


Hasilnya adalah posisi kecocokan, string yang cocok, dan vektor string
untuk sub-cocokan.


\>for k=1:length(vt); vt[k](), end;


    1145.5
    5.6

Berikut ini adalah fungsi yang membaca semua item numerik antara &lt;td&gt;
dan &lt;/td&gt;.


\>function readtd (line) ...


    v=[]; cp=0;
    repeat
       {pos,s,vt}=strxfind(line,"<td.*?>(.+?)</td>",cp);
       until pos==0;
       if length(vt)>0 then v=v|vt[1]; endif;
       cp=pos+strlen(s);
    end;
    return v;
    endfunction
</pre>
\>readtd(line+"<td\>non-numerical</td\>")


    1145.45
    5.6
    -4.5
    non-numerical

# Membaca dari Web

Situs web atau file dengan URL dapat dibuka di EMT dan dapat dibaca
baris demi baris.


Dalam contoh, kita membaca versi saat ini dari situs EMT. Kami
menggunakan ekspresi reguler untuk memindai “Versi ...” dalam judul.


\>function readversion () ...


    urlopen("http://www.euler-math-toolbox.de/Programs/Changes.html");
    repeat
      until urleof();
      s=urlgetline();
      k=strfind(s,"Version ",1);
      if k>0 then substring(s,k,strfind(s,"<",k)-1), break; endif;
    end;
    urlclose();
    endfunction
</pre>
\>readversion


    Version 2021-04-30

# Masukan dan Keluaran Variabel

Anda dapat menulis variabel dalam bentuk definisi Euler ke file atau
ke baris perintah.


\>writevar(pi,"mypi");


    mypi = 3.141592653589793;

Untuk pengujian, kami membuat file Euler di direktori kerja EMT.


\>file="test.e"; ...  
\>   writevar(random(2,2),"M",file); ...  
\>   printfile(file,3)


    M = [ ..
    0.175447553355732, 0.5538947610561991;
    0.9334632085377224, 0.02148969835910218];

Sekarang kita dapat memuat file tersebut. Ini akan mendefinisikan
matriks M.


\>load(file); show M,


    M = 
      0.17545   0.55389 
      0.93346   0.02149 

Sebagai catatan, jika writevar() digunakan pada sebuah variabel, maka
ia akan mencetak definisi variabel dengan nama variabel tersebut.


\>writevar(M); writevar(inch$)


    M = [ ..
    0.175447553355732, 0.5538947610561991;
    0.9334632085377224, 0.02148969835910218];
    inch$ = 0.0254;

Kita juga dapat membuka file baru atau menambahkan ke file yang sudah
ada. Dalam contoh ini, kami menambahkan ke file yang telah dibuat
sebelumnya.


\>open(file,"a"); ...  
\>   writevar(random(2,2),"M1"); ...  
\>   writevar(random(3,1),"M2"); ...  
\>   close();

\>load(file); show M1; show M2;


    M1 = 
      0.64938   0.52967 
      0.13322   0.72143 
    M2 = 
      0.18757 
      0.72351 
      0.11936 

To remove any files use fileremove().


\>fileremove(file);


Sebuah vektor baris dalam sebuah file tidak membutuhkan koma, jika
setiap angka berada dalam baris baru. Mari kita buat file seperti itu,
menulis setiap baris satu per satu dengan writeln().


\>open(file,"w"); writeln("M = ["); ...  
\>   for i=1 to 5; writeln(""+random()); end; ...  
\>   writeln("];"); close(); ...  
\>   printfile(file)


    M = [
    0.916670783384
    0.302475433196
    0.153850217851
    0.0612933450425
    0.453509632288
    ];

\>load(file); M


    [0.91667,  0.30248,  0.15385,  0.061293,  0.45351]

# Soal-Soal

1. Tentukan mean dan standar deviasi dari tinggi badan kelas B,
kemudian buatlah plotnya!


\>A= [160,168,163,158,170,156];

\>mean(A), dev(A)


    162.5
    5.57673739744

\>aspect(1.5); boxplot(A):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-050.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-050.png)

2.Buatlah plot dari 750 angka acak yang didistribusikan normal, lalu
visualisasikan menggunakan boxplot.


\>a=normal(1,750);

\>plot2d(a, distribution=3,style="/");

\>plot2d("qnormal(x,0,5)",add=5):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-051.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-051.png)

\>a=normal(1,750); boxplot(a):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-052.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-052.png)

3. Misalkan punya vektor x=[5,10,15,20,25]


a. buatkan vektor yang menggabungkan vektor x, angka 0 dan vektor x
lagi


b. tentukan apakah setiap elemen vektor x lebih besar dari 10 (hasil
logika 1 untuk benar dan 0 untuk salah)


\>x:=[5,10,15,20,25]; [x,0,x]


    [5,  10,  15,  20,  25,  0,  5,  10,  15,  20,  25]

\>x\>10, %\*x


    [0,  0,  1,  1,  1]
    [0,  0,  15,  20,  25]

4. a) Buatlah plot distribusi jarak antar angka acak yang lebih kecil
dari 0,02 dari 25 juta angka acak. Hitung frekuensi jarak antar angka
acak dalam simulasi Monte Carlo dan bandingkan hasilnya  menggunakan
fungsi


$$f(x)=(n(1-p)^{x-1})p^5$$\>n:=25000000; r:=random(1,n);

\>p:=0.02; d:=differences(nonzeros(r<p));

\>m=getmultiplicities(1:100,d); plot2d(m); ...  
\>    plot2d("n\*(1-p)^(x-1)\*p^5",color=green,\>add):


![images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-054.png](images/Kheisza%20Putri%20Siregar_23030630083_StatistikaEMT-054.png)

<pre class="udf">    
    
    
    
    
</pre>
