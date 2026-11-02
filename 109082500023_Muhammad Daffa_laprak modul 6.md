# <h1 align="center">Laporan Praktikum Modul 6 - Double Linked List (bagian pertama)</h1>
<p align="center">Muhammad Daffa Sha'ib Nandana - 109082500023</p>

## Dasar Teori
Struktur data linear dinamis yang terdiri dari sekumpulan simpul (node), di mana setiap simpul menyimpan data (info) serta dua buah penunjuk (pointer), yaitu next untuk menunjuk ke simpul setelahnya dan prev untuk menunjuk ke simpul sebelumnya. Keberadaan dua penunjuk ini memungkinkan penelusuran data dilakukan secara dua arah (maju dan mundur), yang dikontrol melalui penunjuk utama First sebagai penanda awal senarai dan Last sebagai penanda akhir senarai.



## Unguided 

### 1. Struktur data buku pada buku.h

#ifndef BUKU_H_INCLUDED
#define BUKU_H_INCLUDED

#include <string>
using namespace std;

struct buku {
    string kodeBuku;
    string judul;
    int thnTerbit;
};

typedef buku infotype;
typedef struct ElmList *address;

struct ElmList {
    infotype info;
    address next;
    address prev;
};

struct List {
    address First;
    address Last;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(const List &L);
void insertLast(List &L, address P);
address findElm(const List &L, const string &kode);
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(address Prec, address &P);
void deleteList(List &L);

#endif

##### Output 1
![Screenshot Output Unguided 1_1] https://github.com/daffaeca-a11y/Strukdat-Modul-6/blob/main/output%20modul%206.jpeg

struct buku yang menyimpan informasi kodeBuku, judul, dan thnTerbit, lalu diberi nama alias infotype. Data tersebut dibungkus ke dalam node struct ElmList yang dilengkapi pointer next dan prev untuk menghubungkan ke elemen sebelum dan sesudahnya, dengan penunjuk alamat memorinya dinamai address. Keseluruhan rangkaian elemen ini diatur oleh struct List yang memegang pointer First dan Last sebagai penanda ujung awal dan akhir senarai.


### 2. Implementasi fungsi pada buku.cpp

#include <iostream>
#include "Buku.h"
using namespace std;

void CreateList(List &L) {
    L.First = nullptr;
    L.Last = nullptr;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = nullptr;
    P->prev = nullptr;
    return P;
}

void dealokasi(address &P) {
    if (P != nullptr) {
        delete P;
        P = nullptr;
    }
}

void printInfo(const List &L) {
    address P = L.Last;
    while (P != nullptr) {
        cout << "Kode Buku    : " << P->info.kodeBuku << endl;
        cout << "Judul        : " << P->info.judul << endl;
        cout << "Tahun Terbit : " << P->info.thnTerbit << endl;
        P = P->prev;
    }
}

void insertLast(List &L, address P) {
    if (P == nullptr) return;

    if (L.First == nullptr) {
        L.First = P;
        L.Last = P;
    } else {
        P->prev = L.Last;
        L.Last->next = P;
        L.Last = P;
    }
}

address findElm(const List &L, const string &kode) {
    address P = L.First;
    while (P != nullptr) {
        if (P->info.kodeBuku == kode) {
            return P;
        }
        P = P->next;
    }
    return nullptr;
}

void deleteFirst(List &L, address &P) {
    if (L.First == nullptr) {
        P = nullptr;
        return;
    }

    P = L.First;
    L.First = L.First->next;

    if (L.First != nullptr) {
        L.First->prev = nullptr;
    } else {
        L.Last = nullptr;
    }

    P->next = nullptr;
    P->prev = nullptr;
}

void deleteLast(List &L, address &P) {
    if (L.Last == nullptr) {
        P = nullptr;
        return;
    }

    P = L.Last;
    L.Last = L.Last->prev;

    if (L.Last != nullptr) {
        L.Last->next = nullptr;
    } else {
        L.First = nullptr;
    }

    P->next = nullptr;
    P->prev = nullptr;
}

void deleteAfter(address Prec, address &P) {
    if (Prec == nullptr || Prec->next == nullptr) {
        P = nullptr;
        return;
    }

    P = Prec->next;
    Prec->next = P->next;

    if (P->next != nullptr) {
        P->next->prev = Prec;
    }

    P->next = nullptr;
    P->prev = nullptr;
}

void deleteList(List &L) {
    address P;
    while (L.First != nullptr) {
        deleteFirst(L, P);
        dealokasi(P);
    }
}

##### Output 2
![Screenshot Output Unguided 2_1] https://github.com/daffaeca-a11y/Strukdat-Modul-6/blob/main/output%20modul%206.jpeg

prosedur CreateList yang mengeset penunjuk First dan Last ke nullptr untuk menandai bahwa senarai dalam keadaan kosong. Manajemen memori dinamis diatur oleh fungsi alokasi untuk memesan elemen baru, menyalin data buku ke dalamnya, serta menginisialisasi penunjuk next dan prev menjadi nullptr. Sebaliknya, prosedur dealokasi bertugas menghapus elemen dari memori menggunakan delete dan mengosongkan penunjuknya agar tidak meninggalkan penunjukan alamat liar (dangling pointer).

### 3. Main.cpp

#include <iostream>
#include <string>
#include "Buku.h"
using namespace std;

int main() {
    List L;
    CreateList(L);

    int jumlah;
    cout << "Masukkan jumlah data buku: ";
    cin >> jumlah;

    for (int i = 0; i < jumlah; i++) {
        buku data;
        cout << "\nMasukkan kode buku: ";
        cin >> data.kodeBuku;

        cout << "Masukkan judul buku: ";
        cin >> data.judul;

        cout << "Masukkan tahun terbit: ";
        cin >> data.thnTerbit;

        if (findElm(L, data.kodeBuku) != nullptr) {
            cout << "kode buku sudah terdaftar" << endl;
        } else {
            insertLast(L, alokasi(data));
        }
    }

    cout << "\nDATA LIST 1" << endl;
    printInfo(L);

    string kodeCari;
    cout << "\nMasukkan Kode Buku yang dicari : ";
    cin >> kodeCari;

    address hasil = findElm(L, kodeCari);
    if (hasil != nullptr) {
        cout << "\nKode Buku    : " << hasil->info.kodeBuku << endl;
        cout << "Judul        : " << hasil->info.judul << endl;
        cout << "Tahun Terbit : " << hasil->info.thnTerbit << endl;
    } else {
        cout << "Data buku tidak ditemukan." << endl;
    }

    string kodeHapus;
    cout << "\nMasukkan Kode Buku yang akan dihapus : ";
    cin >> kodeHapus;

    address target = findElm(L, kodeHapus);
    if (target == nullptr) {
        cout << "Data dengan kode buku " << kodeHapus
             << " tidak ditemukan." << endl;
    } else {
        address Hapus = nullptr;

        if (target == L.First) {
            deleteFirst(L, Hapus);
        } else if (target == L.Last) {
            deleteLast(L, Hapus);
        } else {
            deleteAfter(target->prev, Hapus);
        }

        dealokasi(Hapus);
        cout << "Data dengan kode buku " << kodeHapus
             << " berhasil dihapus." << endl;
    }

    cout << "\nDATA LIST 1" << endl;
    printInfo(L);

    deleteList(L);
    return 0;
}

##### Output 3
![Screenshot Output Unguided 3_1] https://github.com/daffaeca-a11y/Strukdat-Modul-6/blob/main/output%20modul%206.jpeg

Program diawali dengan penginisialisasian senarai kosong L menggunakan CreateList, dilanjutkan dengan meminta masukan dari pengguna mengenai jumlah buku yang ingin ditambahkan. Dalam perulangan, pengguna mengisikan kode buku, judul, dan tahun terbit. Sebelum dimasukkan ke senarai, program melakukan validasi melalui findElm untuk memastikan kode buku bersifat unik; jika kode belum terdaftar, data akan dialokasikan dan ditambahkan ke posisi paling belakang senarai dengan insertLast. Setelah proses penginputan selesai, seluruh isi senarai ditampilkan ke layar melalui prosedur printInfo.

## Kesimpulan
Kesimpulan dari modul kali ini adalah, program ini terbagi secara rapi ke dalam tiga berkas, yaitu berkas header (Buku.h) yang berfungsi sebagai cetak biru struktur data dan rancangan fungsi, berkas implementasi (Buku.cpp) yang menangani logika manipulasi pointer, serta berkas utama (main.cpp) sebagai antarmuka interaktif yang menjalankan seluruh fitur program.

## Referensi
[1] Triase. (2020). Diktat Edisi Revisi : STRUKTUR DATA. Medan: UNIVERSTAS ISLAM NEGERI SUMATERA UTARA MEDAN. 
<br>[2] Indahyati, Uce., Rahmawati Yunianita. (2020). "BUKU AJAR ALGORITMA DAN PEMROGRAMAN DALAM BAHASA C++". Sidoarjo: Umsida Press. Diakses pada 10 Maret 2024 melalui https://doi.org/10.21070/2020/978-623-6833-67-4.

