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

![Screenshot from 2024-04-21 14-10-24](https://github.com/Merody/LBM/assets/88936922/8305da37-ac9f-49c9-ae88-132824b9a78a)
![Screenshot from 2024-04-21 14-11-43](https://github.com/Merody/LBM/assets/88936922/760de79c-4906-4f07-86bc-f3024476eb3f)

## 2. Install File

Unzip file ln_solver :

    tar xvf ln_solver2.3.tar.gz

Kemudian copy file

- ln_solver2.2.ncepdata.tar.gz
- ln_solver2.2.ecmdata.tar.gz
- sysdep_linux.tar.gz

ke dalam file ln_solver

![Screenshot from 2024-04-21 14-15-30](https://github.com/Merody/LBM/assets/88936922/8b6483c8-2a4a-4c52-9123-a06023fe41a6)

Di dalam file ln_solver, unzip semua file :

    tar xvf sysdep_linux.tar.gz

    tar xvf ln_solver2.2.ncepdata.tar.gz

    tar xvf ln_solver2.2.ecmdata.tar.gz

setelah itu masuk ke bashrc untuk mengedit PATH $LNHOME :

    nano ~/.bashrc

![Screenshot from 2024-04-21 14-08-49](https://github.com/Merody/LBM/assets/88936922/230b1ddc-1b0c-4310-aa71-2db77ac5b2ed)

exit bashrc kemudian

    source ~/.bashrc

pastikan path $LNHOME bisa di akses menggunakan

    cd $LNHOME

## 3. Pengaturan MLBM

### 3.1. Edit file

Buka File pada $LNHOME/Lmake.inc

![Screenshot from 2024-04-21 14-17-02](https://github.com/Merody/LBM/assets/88936922/83e0e0b0-1e80-442f-8965-7c250e67640b)
![Screenshot from 2024-04-21 14-17-18](https://github.com/Merody/LBM/assets/88936922/e29c05ea-8c62-4723-8b66-85cc5dd6724b)
![Screenshot from 2024-04-21 14-17-42](https://github.com/Merody/LBM/assets/88936922/8ed098c5-032e-49d4-bd70-f5661a6e2820)
![Screenshot from 2024-04-21 14-18-04](https://github.com/Merody/LBM/assets/88936922/9d274a2f-704a-4250-b849-0026bd90e863)

setelah itu masuk ke $LNHOME/model/src/sysdep/Makedef.linux

dan edit compilernya menjadi icc dan ifort

![Screenshot from 2024-04-22 00-37-56](https://github.com/Merody/LBM/assets/88936922/bf862e57-3f3f-4ccd-a598-96bd53aa5497)

### 3.2. Membuat file lib dan bin linux

membuat directory lib dan bin untuk linux
![Screenshot from 2024-04-21 14-19-11](https://github.com/Merody/LBM/assets/88936922/be42ac22-e6e6-4a9a-8dbf-31af086056ab)
![Screenshot from 2024-04-21 14-19-05](https://github.com/Merody/LBM/assets/88936922/d351dc3a-9a65-47a6-a693-eb829bf896a9)

running make lib pada $LNHOME/model/src

    make lib

File liblbm2t21ml20c.a akan muncul pada file lib linux yang telah dibuat $LNHOME/model/lib/linux

![Screenshot from 2024-04-21 14-21-58](https://github.com/Merody/LBM/assets/88936922/64ead930-01f1-45eb-9aa3-a9e734e0f481)

Setelah itu run

    make.clean special

dan

    make lbm

![Screenshot from 2024-04-21 14-23-09](https://github.com/Merody/LBM/assets/88936922/b2c9e484-0fe1-4fea-a9a3-6e5fe8b80ee0)
![Screenshot from 2024-04-21 14-23-44](https://github.com/Merody/LBM/assets/88936922/84041772-e1e0-43ea-8e8e-1f2dc544d2ac)

file lbm2.t21ml20ctingr akan muncul pada file bin linux yang telah dibuat $LNHOME/model/bin/linux

![Screenshot from 2024-04-21 14-24-40](https://github.com/Merody/LBM/assets/88936922/3c648391-229e-4113-8acb-f6fc33b10d97)

### 3.3. Install dan Pengaturan File Ekstensi Lapack

#### 3.3.1. Edit file $LNHOME/solver/include/make.inc.linux

![Screenshot from 2024-04-21 14-26-44](https://github.com/Merody/LBM/assets/88936922/14500b2e-b3a0-44a3-b713-6579f23b1500)
![Screenshot from 2024-04-21 14-26-24](https://github.com/Merody/LBM/assets/88936922/aee63236-00c9-4ab1-9fe7-6c1524f95ae6)

#### 3.3.2. Unzip Lapack

    tar xvf lapack-3.9.0.tar.gz

membuat folder linux di $LNHOME/solver/lib

kemudian pindahkan file lapack-3.9.0 ke $LNHOME/solver/lib/linux

    mv lapack-3.9.0/ $LNHOME/solver/lib/linux

#### 3.3.3. Edit File $LNHOME/solver/lib/linux/make.inc.example (Setelah edit ubah namanya jadi make.inc)

![Screenshot from 2024-04-21 14-28-04](https://github.com/Merody/LBM/assets/88936922/6b098122-3573-4ee5-9c1a-17ad049ca2d8)
![Screenshot from 2024-04-21 14-28-14](https://github.com/Merody/LBM/assets/88936922/07e4ee84-9b9f-48d9-bf32-8ea887930123)
![Screenshot from 2024-04-21 14-28-28](https://github.com/Merody/LBM/assets/88936922/3693d647-2687-41b8-a905-c5f9db373ccd)

ubah juga $LNHOME/solver/lib/linux/Makefile

![Screenshot from 2024-04-21 14-29-22](https://github.com/Merody/LBM/assets/88936922/1db24fb0-c50d-4a93-9052-31b65504d955)

kemudian run make pada $LNHOME/solver/lib/linux

    make

maka akan muncul 3 file tersebut

![Screenshot from 2024-04-21 14-30-06](https://github.com/Merody/LBM/assets/88936922/30ae1ace-2290-4221-b651-0c24e009f642)

Akses file $LNHOME/solver/util kemudian ketik code

    make bs

maka akan muncul seperti ini

![Screenshot from 2024-04-21 14-30-44](https://github.com/Merody/LBM/assets/88936922/4b23ccc7-21e3-4c13-9795-4be072fd1ed9)

## 4. Persiapan Running Model

### 4.1. Membuat basic states atmosfer

Pengaturan untuk basic states terdapat pada file SETPAR pada file $LNHOME/solver/util yaitu:

- &nmncp
- &nmbs

pengaturan untuk basic states dapat dilakukan seperti ini

![Screenshot from 2024-04-21 14-32-17(1)](https://github.com/Merody/LBM/assets/88936922/89d34bda-ab33-450a-a2f9-630641db3278)

untuk musim dingin(djf) pengaturannya berupa kmo=12, navg=3
yang berarti bulan 12 dan selama 3 bulan dari bulan tersebut 12,1, dan 2

Jika pengaturan basic states sudah dilakukan, selanjutnya adalah running code

    ./ncepsbs

jika muncul tulisan seperti forrtl: severe (24) maka terjadi error

yang harusnya terjadi adalah seperti ini

![Screenshot from 2024-04-21 14-31-39](https://github.com/Merody/LBM/assets/88936922/b7012c88-6853-4880-b756-8fd745b8f5b0)

### 4.2. Membuat basic states SST dan WG (Untuk MLBM)

membuatnya sama seperti basic states atmosfer yaitu di file SETPAR seperti ini

![Screenshot from 2024-04-21 17-44-40](https://github.com/Merody/LBM/assets/88936922/ad8cb549-1f5e-4ffe-8cc6-ad9619e5c03a)
![Screenshot from 2024-04-21 17-45-12](https://github.com/Merody/LBM/assets/88936922/33920907-7716-45aa-b967-fe975f0fa47a)

kemudian run code

    make clean
    make
    ./ncep1vbs

### 4.3. Membuat Forcing MLBM

Contoh forcing terdapat pada file $LNHOME/sample. Untuk LBM yaitu frc.t21l20.classic.grd dan untuk MLBM yaitu frcsst.t21.grd

edit file SETPAR untuk konfigurasi forcing yang dibutuhkan

![Screenshot from 2024-04-21 17-46-16](https://github.com/Merody/LBM/assets/88936922/9ca4f04f-c825-441e-8734-98cc35735ae4)
![Screenshot from 2024-04-21 14-32-32(1)](https://github.com/Merody/LBM/assets/88936922/a1e4e7f6-d80e-4176-95c4-102748e6277c)

membuat file input forcing dan output yaitu $LNHOME/data/Forcing dan $LNHOME/data/Output untuk memudahkan running model

    cd $LNHOME/solver/util

    make clean

    make

![Screenshot from 2024-04-21 14-33-46](https://github.com/Merody/LBM/assets/88936922/00d023af-cf9f-44f8-9b62-70559775a1b7)
kemudian untuk forcing MLBM

    ./mkfrcsst

![Screenshot from 2024-04-21 17-38-40](https://github.com/Merody/LBM/assets/88936922/4fd3b015-cf22-4401-aa90-4e7263ef0703)

## 5. Running Model

File yang diperlukan untuk running model adalah input forcing yang telah dibuat

$LNHOME/model/sh/tingr/linear-run.csh merupakan file untuk running model MLBM. Copy file tersebut menjadi linear-run.moist-test.csh dan edit

    cd $LNHOME/model/sh/tingr
    cp linear-run.csh linear-run.moist-test.csh
    chmod u+x linear-run.moist-test.csh

edit file linear_run.moist-test.csh menjadi seperti berikut

![Screenshot from 2024-04-22 00-29-39](https://github.com/Merody/LBM/assets/88936922/cd86895c-f411-40c6-98af-a9e47281f922)
![Screenshot from 2024-04-22 00-29-51](https://github.com/Merody/LBM/assets/88936922/f88a2e6d-fae9-430b-83cb-7e6c14b4b33e)
![Screenshot from 2024-04-22 00-30-24](https://github.com/Merody/LBM/assets/88936922/167b0a27-32d5-4ebd-84d1-ff96dd10d8a7)

kemudian run
cd $LNHOME/model/sh/tingr
./linear-run.moist-test.csh

dan terdapat beberapa file pada $LNHOME/data/Output

![Screenshot from 2024-04-21 14-39-06](https://github.com/Merody/LBM/assets/88936922/0d78c911-be26-4f4d-87e7-87898565c9c4)

## 6. Post Proses dan Visualisasi

Output yang telah dihasilkan perlu digabungkan. Penggabungan file-file tersebut dilakukan menggunakan gt2gr yang terdapat pada $LNHOME/solver/util dan diedit pada SETPAR

SETPAR untuk post Proses adalah sebagai berikut

![Screenshot from 2024-04-21 14-32-17(3)](https://github.com/Merody/LBM/assets/88936922/e4afd265-3b97-4d94-9b7f-d68718cb2088)
![Screenshot from 2024-04-21 14-32-32(1)](https://github.com/Merody/LBM/assets/88936922/f959afb4-c03a-4517-9d1e-968d72ce9f21)

kemudian untuk menyatukan file menggunakan kode:

    cd $LNHOME/solver/util
    ./gt2gr

dengan log message setelahnya adalah seperti berikut

![Screenshot from 2024-04-21 14-39-53](https://github.com/Merody/LBM/assets/88936922/1a6d4c64-fd8f-4bff-b589-eeb310d726a5)
![Screenshot from 2024-04-21 14-39-59](https://github.com/Merody/LBM/assets/88936922/811f2bab-a50b-4620-9cbe-fcfb73737e97)
![Screenshot from 2024-04-21 14-40-03](https://github.com/Merody/LBM/assets/88936922/0f03ad77-fac6-4199-b86a-e714a5e88d31)

File hasil MLBM telah berhasil dibuat dalam bentuk binary data .grd. Untuk memudahkan pengolahan dan visualisasi binary data dapat di konversi ke netCDF menggunakan CDO maupun GraDS.

![Screenshot from 2024-04-21 14-40-24](https://github.com/Merody/LBM/assets/88936922/db772da5-bb5d-409c-9404-fc476df14cc6)

Konversi binary data memerlukan file catalog (.ctl) yang sudah disediakan pada $LNHOME/sample. Untuk LBM filenya adalah linear.t21l20.classic.ctl dan MLBM adalah linear.t21l20.ctl

dalam file .ctl perlu ditambahkan OPTION big_endian

![Screenshot from 2024-04-21 14-40-41](https://github.com/Merody/LBM/assets/88936922/e56a3158-0d1d-45cb-93e7-cba8de880753)

kemudian

    cdo -f nc import binary linearmoist.ctl linearmoist.nc

![Screenshot from 2024-04-21 14-41-59](https://github.com/Merody/LBM/assets/88936922/44c9338d-4716-4395-ae34-db7ea6b0531d)

berikut ini salah satu contoh visualisasi hasil running MLBM

![hgtz500elnino](https://github.com/Merody/LBM/assets/88936922/96e83538-fd08-4454-a830-051a9f89c6ee)
