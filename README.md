# Tutorial Install Moist Linear Baroclinic Model di Linux

## 1. Download File

File yang diperlukan dapat di download di [website ccsr](https://ccsr.aori.u-tokyo.ac.jp/~lbm/sub/lbm_4.html) yang memerlukan izin akses melalui email hayashi.michiya@nies.go.jp. File-file yang digunakan yaitu:

- ln_solver2.3.tar.gz
- ln_solver2.2.ncepdata.tar.gz
- ln_solver2.2.ecmdata.tar.gz
- model/src/sysdep/ and solver/include/ files for the Linux architecture

Dibutuhkan juga file ekstensi Lapack yang bisa di download pada [link berikut](https://codeload.github.com/Reference-LAPACK/lapack/tar.gz/v3.9.0)

- Lapack-3.9.0.tar.gz

---

Compiler yang digunakan berupa

- Intel Fortran Compiler (Beta)
- Intel Fortran Compiler Classic Linux (ifort)
- Intel OneApi DPC++/C++ Compiler
- Intel C++ Compiler Classic Linux (icc).

Pastikan icc dan ifort terdapat dalam sistem seperti ini:

gambar 1 (icc -v)

## 2. Install File

Unzip file ln_solver :

    tar xvf ln_solver2.3.tar.gz

Kemudian copy file

- ln_solver2.2.ncepdata.tar.gz
- ln_solver2.2.ecmdata.tar.gz
- sysdep_linux.tar.gz

ke dalam file ln_solver

gambar 2 (file ln_solver)

Di dalam file ln_solver, unzip semua file :

    tar xvf sysdep_linux.tar.gz

    tar xvf ln_solver2.2.ncepdata.tar.gz

    tar xvf ln_solver2.2.ecmdata.tar.gz

setelah itu masuk ke bashrc untuk mengedit PATH $LNHOME :

    nano ~/.bashrc

gambar 6
tambah gambar export big_endian

exit bashrc kemudian

    source ~/.bashrc

pastikan path $LNHOME bisa di akses menggunakan

    cd $LNHOME

## 3. Pengaturan MLBM

### 3.1. Edit file

Buka File pada $LNHOME/Lmake.inc

gambar 7

setelah itu masuk ke $LNHOME/model/src/sysdep/Makedef.linux

dan edit compilernya menjadi icc dan ifort

gambar 8

### 3.2. Membuat file lib dan bin linux

membuat directory lib dan bin untuk linux

gambar 9

running make lib pada $LNHOME/model/src

    make lib

File liblbm2t21ml20c.a akan muncul pada file lib linux yang telah dibuat $LNHOME/model/lib/linux

gambar 10

Setelah itu run

    make.clean special

dan

    make lbm

gambar 11

file lbm2.t21ml20ctingr akan muncul pada file bin linux yang telah dibuat $LNHOME/model/bin/linux

gambar 12

### 3.3. Install dan Pengaturan File Ekstensi Lapack

#### 3.3.1. Edit file $LNHOME/solver/include/make.inc.linux

gambar 13

kemudian

gambar 14

#### 3.3.2. Unzip Lapack

    tar xvf lapack-3.9.0.tar.gz

membuat folder linux di $LNHOME/solver/lib

kemudian pindahkan file lapack-3.9.0 ke $LNHOME/solver/lib/linux

    mv lapack-3.9.0/ $LNHOME/solver/lib/linux

#### 3.3.3. Edit File $LNHOME/solver/lib/linux/make.inc.example (Setelah edit ubah namanya jadi make.inc)

gambar 15

ubah juga $LNHOME/solver/lib/linux/Makefile

kemudian run make pada $LNHOME/solver/lib/linux

    make

maka akan muncul 3 file tersebut

gambar 16

Akses file $LNHOME/solver/util kemudian ketik code

    make bs

maka akan muncul seperti ini

gambar 17

## 4. Persiapan Running Model

### 4.1. Membuat basic states atmosfer

Pengaturan untuk basic states terdapat pada file SETPAR pada file $LNHOME/solver/util yaitu:

- &nmncp
- &nmbs

pengaturan untuk basic states dapat dilakukan seperti ini

gambar 18

untuk musim dingin(djf) pengaturannya berupa kmo=12, navg=3
yang berarti bulan 12 dan selama 3 bulan dari bulan tersebut 12,1, dan 2

Jika pengaturan basic states sudah dilakukan, selanjutnya adalah running code

    ./ncepsbs

jika muncul tulisan seperti forrtl: severe (24) maka terjadi error

yang harusnya terjadi adalah seperti ini

gambar 19

### 4.2. Membuat basic states SST dan WG (Untuk MLBM)

membuatnya sama seperti basic states atmosfer yaitu di file SETPAR seperti ini

gambar 20

kemudian run code

    make clean
    make
    ./ncep1vbs

### 4.3. Membuat Forcing

Contoh forcing terdapat pada file $LNHOME/sample. Untuk LBM yaitu frc.t21l20.classic.grd dan untuk MLBM yaitu frcsst.t21.grd

edit file SETPAR untuk konfigurasi forcing yang dibutuhkan

gambar 21

membuat file input forcing dan output yaitu $LNHOME/data/Forcing dan $LNHOME/data/Output untuk memudahkan running model

    cd $LNHOME/solver/util

    make clean

    make

kemudian untuk forcing MLBM

    ./mkfrcsst

gambar 22

## 5. Running Model

File yang diperlukan untuk running model adalah input forcing yang telah dibuat

$LNHOME/model/sh/tingr/linear-run.csh merupakan file untuk running model MLBM. Copy file tersebut menjadi linear-run.moist-test.csh dan edit

    cd $LNHOME/model/sh/tingr
    cp linear-run.csh linear-run.moist-test.csh
    chmod u+x linear-run.moist-test.csh

edit file linear_run.moist-test.csh menjadi seperti berikut

gambar 23

kemudian run
cd $LNHOME/model/sh/tingr
./linear-run.moist-test.csh

akan muncul log message seperti ini

gambar 24

dan terdapat beberapa file pada $LNHOME/data/Output

gambar 25

## 6. Post Proses dan Visualisasi

Output yang telah dihasilkan perlu digabungkan. Penggabungan file-file tersebut dilakukan menggunakan gt2gr yang terdapat pada $LNHOME/solver/util dan diedit pada SETPAR

SETPAR untuk post Proses adalah sebagai berikut

gambar 26

kemudian

    cd $LNHOME/solver/util
    ./gt2gr

dengan log message setelahnya adalah seperti berikut

gambar 27

File hasil MLBM telah berhasil dibuat dalam bentuk binary data .grd. Untuk memudahkan pengolahan dan visualisasi binary data dapat di konversi ke netCDF menggunakan CDO maupun GraDS.

Konversi binary data memerlukan file catalog (.ctl) yang sudah disediakan pada $LNHOME/sample. Untuk LBM filenya adalah linear.t21l20.classic.ctl dan MLBM adalah linear.t21l20.ctl

dalam file .ctl perlu ditambahkan OPTION big_endian

gambar 28

kemudian

    cdo -f nc import binary linearmoist.ctl linearmoist.nc

berikut ini salah satu contoh visualisasi hasil running MLBM

gambar 29
