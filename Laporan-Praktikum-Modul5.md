# <h1 align="center">Laporan Praktikum Modul 5 - SINGLY LINKED LIST (BAGIAN KEDUA)</h1>
<p align="center">muh rifqi irwanto - 109082500016</p>

## Dasar Teori
Singly Linked List adalah sebuah model ADT Linked List yang hanya memiliki pointer satu arah. Setiap elemen (node) terdiri atas dua komponen: info (data yang disimpan) dan next (penunjuk yang mengarah ke elemen selanjutnya). Linked list memiliki sifat dinamis karena dapat berkembang dan menyusut sesuai kebutuhan, berbeda dengan array yang bersifat tetap [1]. Istilah kunci dalam Singly Linked List: first/head: penunjuk yang mengarah ke alamat elemen pertama daftar next: penunjuk pada elemen yang berfungsi sebagai penerus Nil/NULL: nilai kosong, tidak menunjuk ke mana pun Node/simpul: lokasi penyimpanan data dalam memori Operasi dasar ADT Linked List mencakup: CreateList, alokasi, dealokasi, insertFirst, insertLast, insertAfter, deleteFirst, deleteLast, deleteAfter, printInfo, dan nbList [1].

## Guided 

### 1.Program ADT Singly Linked List
```C++
// File: listBuah.h
#ifndef LISTBUAH_H
#define LISTBUAH_H
#define Nil NULL

#include<iostream>
using namespace std;

struct buah{
    string nama;
    int jumlah; 
    float harga;
};

typedef buah dataBuah; 

typedef struct node *address; 

struct node{ 
    dataBuah isidata;
    address next;
};

struct linkedlist{ 
    address first;
};

bool isEmpty(linkedlist List);
void createList(linkedlist &List);
address alokasi(string nama, int jumlah, float harga);
void dealokasi(address &node);
void printList(linkedlist List);
void insertFirst(linkedlist &List,address nodeBaru);
void insertAfter(linkedlist &List, address nodeBaru, address prev);
void insertLast(linkedlist &List, address nodeBaru);
void delFirst(linkedlist &List);
void delLast(linkedlist &List);
void delAfter(linkedlist &List, address nodeHapus, address nodePrev);
int nbList(linkedlist List);
void deleteList(linkedlist &List);

void updateFirst(linkedlist List);
void updateLast(linkedlist List);
void updateAfter(linkedlist List, address prev);

void FindNodeByData(linkedlist list, string data);
void FindNodeByAddress(linkedlist list, address node);
void FindNodeByRange(linkedlist list, float hargaAwal, float HargaAkhir);

#endif

// File: listBuah.cpp
#include "listBuah.h"
#include <iostream>
using namespace std;

//fungsi untuk cek apakah list kosong atau tidak
bool isEmpty(linkedlist List) {
    if(List.first == Nil){
        return true; 
    } else {
        return false;
    }
}

//pembuatan linked list kosong
void createList(linkedlist &List) {
    List.first = Nil;
}

//pembuatan node baru dengan menerapkan manajemen memori
address alokasi(string nama, int jumlah, float harga) { 
    address nodeBaru = new node; 
    nodeBaru->isidata.nama = nama;
    nodeBaru->isidata.jumlah = jumlah; 
    nodeBaru->isidata.harga = harga;
    nodeBaru->next = Nil;
    return nodeBaru;
}

//penghapusan node dengan menerapkan manajemen memori
void dealokasi(address &node) {
    node->next = Nil;
    delete node;
}

//prosedur-prosedur untuk insert / menambahkan node baru kedalam list
void insertFirst(linkedlist &List, address nodeBaru) {
    nodeBaru->next = List.first; 
    List.first = nodeBaru;
}

void insertAfter(linkedlist &List, address nodeBaru, address Prev) {
    if (Prev != Nil) {
        nodeBaru->next = Prev->next;
        Prev->next = nodeBaru;
    } else {
        cout << "Node sebelumnya tidak valid!" << endl;
    }
}

void insertLast(linkedlist &List, address nodeBaru) {
    if (isEmpty(List)) {
        List.first = nodeBaru;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        nodeBantu->next = nodeBaru;
    }
}

//prosedur-prosedur untuk delete / menghapus node yang ada didalam list
void delFirst(linkedlist &List){
    address nodeHapus;
    if (isEmpty(List) == false) {
        nodeHapus = List.first;
        List.first = List.first->next;
        nodeHapus->next = Nil;
        dealokasi(nodeHapus);
        cout << "Node pertama berhasil terhapus!" << endl;
    } else {
        cout << "List kosong!" << endl;
    }
}

void delLast(linkedlist &List){
    address nodeHapus, nodePrev;
    if(isEmpty(List) == false){
        nodeHapus = List.first;
        if(nodeHapus->next == Nil){
            List.first = Nil;
            dealokasi(nodeHapus);
        } else { 
            while(nodeHapus->next != Nil){
                nodePrev = nodeHapus; 
                nodeHapus = nodeHapus->next;
            }
            nodePrev->next = Nil; 
            dealokasi(nodeHapus);
        }
        cout << "Node terakhir berhasil terhapus!" << endl;
    } else {
        cout << "list kosong" << endl;
    }
}

void delAfter(linkedlist &List, address nodeHapus, address nodePrev){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else {
        if (nodePrev != Nil && nodePrev->next != Nil) { 
            nodeHapus = nodePrev->next;       
            nodePrev->next = nodeHapus->next;  
            nodeHapus->next = Nil;         
            dealokasi(nodeHapus);
            cout << "Node setelah node " << nodePrev->isidata.nama << " berhasil terhapus!" << endl;
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

//prosedur untuk menampilkan isi list
void printList(linkedlist List) {
    if (isEmpty(List)) {
        cout << "List kosong." << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu != Nil) { 
            cout << "Nama Buah : " << nodeBantu->isidata.nama 
                 << ", Jumlah : " << nodeBantu->isidata.jumlah 
                 << ", Harga : " << nodeBantu->isidata.harga << endl;
            nodeBantu = nodeBantu->next;
        }
    }
}

//function untuk menampilkan jumlah node didalam list
int nbList(linkedlist List) {
    int count = 0;
    address nodeBantu = List.first;
    while (nodeBantu != Nil) {
        count++;
        nodeBantu = nodeBantu->next; 
    }
    return count;
}

//prosedur untuk menghapus list (menghapus semua node didalam list)
void deleteList(linkedlist &List){
    address nodeBantu, nodeHapus;
    nodeBantu = List.first;
    while(nodeBantu != Nil){
        nodeHapus = nodeBantu;
        nodeBantu = nodeBantu->next;
        dealokasi(nodeHapus); 
    }
    List.first = Nil; 
    cout << "List berhasil terhapus!" << endl;
}

/*----- MATERI PERTEMUAN 5 - SINGLY LINKED LIST (BAGIAN KEDUA) - UPDATE -----*/
void updateFirst(linkedlist List){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else {
        cout << "Masukkan update data node pertama : " << endl;
        cout << "Nama buah : ";
        cin >> List.first->isidata.nama;
        cout << "Jumlah : ";
        cin >> List.first->isidata.jumlah;
        cout << "Harga : ";
        cin >> List.first->isidata.harga;
        cout << "Data Berhasil Diupdate!" << endl;
        cout << endl;
    }
}

void updateLast(linkedlist List){
    if (isEmpty(List) == true) {
        cout << "List Kosong!" << endl;
    } else {
        address nodeBantu = List.first;
        while (nodeBantu->next != Nil) {
            nodeBantu = nodeBantu->next;
        }
        cout << "masukkan update data node terakhir : " << endl;
        cout << "Nama buah : ";
        cin >> nodeBantu->isidata.nama;
        cout << "Jumlah : ";
        cin >> nodeBantu->isidata.jumlah;
        cout << "Harga : ";
        cin >> nodeBantu->isidata.harga;
        cout << "Data Berhasil Diupdate!" << endl;
        cout << endl;
    }
}

void updateAfter(linkedlist List, address nodePrev){
    if(isEmpty(List) == true){
        cout << "List kosong!" << endl;
    } else {
        if (nodePrev != Nil && nodePrev->next != Nil){
            address nodeBantu = nodePrev->next;
            cout << "masukkan update data node setelah node " << nodePrev->isidata.nama << " : " << endl;
            cout << "Nama buah : ";
            cin >> nodeBantu->isidata.nama;
            cout << "Jumlah : ";
            cin >> nodeBantu->isidata.jumlah;
            cout << "Harga : ";
            cin >> nodeBantu->isidata.harga;
            cout << "Data Berhasil Diupdate!" << endl;
            cout << endl;
        } else {
            cout << "Node sebelumnya (prev) tidak valid!" << endl;
        }
    }
}

/*----- MATERI PERTEMUAN 5 - SINGLY LINKED LIST (BAGIAN KEDUA) - SEARCHING -----*/
void FindNodeByData(linkedlist list, string data){
    if(isEmpty(list) == true){
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        while(nodeBantu != Nil){
            posisi++;
            if(nodeBantu->isidata.nama == data){
                cout << "Data " << data << " ditemukan pada posisi ke-" << posisi << "!" << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama 
                     << ", Jumlah : " << nodeBantu->isidata.jumlah 
                     << ", Harga : " << nodeBantu->isidata.harga << endl;
                found = true;
                break;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false){
            cout << "Node dengan data " << data << " tidak ditemukan!" << endl;
        }
    }
    cout << endl;
}

void FindNodeByAddress(linkedlist list, address node) {
    if(isEmpty(list) == true) {
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        while (nodeBantu != Nil) {
            posisi++;
            if(nodeBantu == node) {
                cout << "Node ditemukan pada posisi ke-" << posisi << "!" << endl;
                cout << "Alamat node : " << nodeBantu << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama 
                     << ", Jumlah : " << nodeBantu->isidata.jumlah 
                     << ", Harga : " << nodeBantu->isidata.harga << endl;
                found = true;
                break;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false) {
            cout << "Node dengan alamat " << node << " tidak ditemukan dalam list!" << endl;
        }
    }
    cout << endl;
}

void FindNodeByRange(linkedlist list, float hargaAwal, float hargaAkhir) {
    if(isEmpty(list) == true) {
        cout << "List kosong!" << endl;
    } else {
        address nodeBantu = list.first;
        int posisi = 0;
        bool found = false;
        cout << "--- Buah dalam range harga " << hargaAwal << " - " << hargaAkhir << " ---" << endl;
        cout << "-------------------------------------------" << endl;
        while (nodeBantu != Nil) {
            posisi++;
            float harga = nodeBantu->isidata.harga;
            if(harga >= hargaAwal && harga <= hargaAkhir) {
                cout << "Data ditemukan pada posisi ke-" << posisi << " :" << endl;
                cout << "Nama Buah : " << nodeBantu->isidata.nama 
                     << ", Jumlah : " << nodeBantu->isidata.jumlah 
                     << ", Harga : " << nodeBantu->isidata.harga << endl;
                cout << "-------------------------------------------" << endl;
                found = true;
            }
            nodeBantu = nodeBantu->next;
        }
        if(found == false) {
            cout << "Tidak ada data buah dalam range harga tersebut!" << endl;
            cout << "-------------------------------------------" << endl;
        }
    }
    cout << endl;
}

// File: main.cpp
#include "listBuah.h"
#include <iostream>
using namespace std;

int main() {
    linkedlist List;
    createList(List);

    address apel   = alokasi("Apel", 10, 5000);
    address jeruk  = alokasi("Jeruk", 20, 3000);
    address mangga = alokasi("Mangga", 15, 7000);
    address anggur = alokasi("Anggur", 5, 12000);

    insertFirst(List, apel);
    insertLast(List, jeruk);
    insertAfter(List, mangga, apel);
    insertLast(List, anggur);

    cout << "--- ISI LIST AWAL ---" << endl;
    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;
    cout << endl;

    cout << "--- SEARCHING ---" << endl;
    FindNodeByData(List, "Mangga");
    FindNodeByAddress(List, mangga);
    FindNodeByRange(List, 4000, 8000);

    // Update dapat diaktifkan sesuai kebutuhan praktikum
    // updateFirst(List);
    // updateLast(List);
    // updateAfter(List, apel);

    cout << "--- DELETE ---" << endl;
    delFirst(List);
    delLast(List);

    address nodeHapus;
    delAfter(List, nodeHapus, mangga);

    cout << "--- ISI LIST SETELAH DELETE ---" << endl;
    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;
    cout << endl;

    deleteList(List);

    cout << "--- ISI LIST SETELAH DELETE LIST ---" << endl;
    printList(List);
    cout << "Jumlah node : " << nbList(List) << endl;

    return 0;
}
```

## Unguided 

### 1. Program ADT Singly Linked List - Insert First

```C++

File: Singlylist.h
#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
using namespace std;

#define Nil NULL

typedef int infotype;
typedef struct Elmlist *address;

struct Elmlist {
    infotype info;
    address next;
};

struct List {
    address First;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);

#endif

File: Singlylist.cpp
#include "Singlylist.h"

void CreateList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new Elmlist;

    P->info = x;
    P->next = Nil;

    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;

    while (P != Nil) {
        cout << P->info;

        if (P->next != Nil) {
            cout << " ";
        }

        P = P->next;
    }

    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}

File: main.cpp
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5;

    CreateList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    cout << "Isi Singly Linked List: ";
    printInfo(L);

    return 0;
}

```

Program ini dimulai dengan menciptakan sebuah daftar kosong melalui prosedur `CreateList`, yang bertujuan untuk menginisialisasi pointer `First` agar tidak mengacu pada node apa pun. Setelah itu, program memberikan alokasi kepada lima node dengan nilai berurutan, yaitu 2, 0, 8, 12, dan 9. Setiap node selanjutnya dimasukkan ke dalam daftar menggunakan prosedur `insertFirst`, yang menempatkan node baru di posisi terdepan dan menghubungkannya dengan node yang telah ada sebelumnya. Dengan demikian, setiap kali sebuah node baru ditambahkan, letak node itu akan berada di depan node yang sudah ada sebelumnya. Akibatnya, susunan elemen dalam daftar berubah menjadi terbalik dari urutan ketika elemen dimasukkan, sehingga angka 9 yang ditambahkan terakhir menjadi elemen pertama, diikuti oleh 12, 8, 0, dan 2 sebagai elemen terakhir. Selanjutnya, fungsi `printInfo` berfungsi untuk menampilkan semua isi daftar dengan mengunjungi setiap node dari `First` hingga mencapai `Nil`. Berdasarkan proses itu, program menghasilkan keluaran `9 12 8 0 2`. Hasil ini mengindikasikan bahwa prosedur `insertFirst` sukses menambahkan node di awal daftar dan mengaitkan setiap node menggunakan pointer `next`, seperti yang diperlihatkan pada ilustrasi Singly Linked List dalam soal.

### 2.Pencarian Elemen dengan Info 8
```C++
File: Singlylist.h

#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
using namespace std;

#define Nil NULL

typedef int infotype;
typedef struct Elmlist *address;

struct Elmlist {
    infotype info;
    address next;
};

struct List {
    address First;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);
address findElm(List L, infotype x);

#endif

File: Singlylist.cpp

#include "Singlylist.h"

void CreateList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new Elmlist;

    P->info = x;
    P->next = Nil;

    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;

    while (P != Nil) {
        cout << P->info;

        if (P->next != Nil) {
            cout << " ";
        }

        P = P->next;
    }

    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}

address findElm(List L, infotype x) {
    address P = L.First;

    while (P != Nil) {
        if (P->info == x) {
            return P;
        }

        P = P->next;
    }

    return Nil;
}
File: main.cpp
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5;
    address hasil;

    CreateList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    cout << "Isi Singly Linked List: ";
    printInfo(L);

    hasil = findElm(L, 8);

    if (hasil != Nil) {
        cout << "8 ditemukan dalam list" << endl;
    } else {
        cout << "8 tidak ditemukan dalam list" << endl;
    }

    return 0;
}
```
Fungsi `findElm` berfungsi untuk mencari elemen spesifik dalam Singly Linked List dengan mengeksplorasi setiap node mulai dari elemen pertama hingga mencapai `Nil`. Di setiap node, program melakukan perbandingan antara nilai `info` dan nilai yang dicari. Apabila nilai yang dicari sepadan dengan nilai `info` pada node itu, fungsi akan mengeluarkan alamat node yang tepat. Akan tetapi, jika nilai tersebut tidak ada setelah semua node diperiksa, fungsi akan mengembalikan `Nil`. Dalam program ini, nilai 8 dicari dalam daftar yang terdiri dari elemen 9, 12, 8, 0, dan 2. Proses pencarian dimulai dari nilai 9, lalu berlanjut ke 12, hingga akhirnya menemukan nilai 8 di node ketiga. Setelah nilai itu ditemukan, program menunjukkan pesan `8 ditemukan dalam daftar`. Dengan demikian, fungsi `findElm` mampu melakukan pencarian elemen sesuai dengan hasil yang diharapkan.

### 3. Menghitung Total Info Seluruh Elemen
```C++
File: Singlylist.h

#ifndef SINGLYLIST_H_INCLUDED
#define SINGLYLIST_H_INCLUDED

#include <iostream>
using namespace std;

#define Nil NULL

typedef int infotype;
typedef struct Elmlist *address;

struct Elmlist {
    infotype info;
    address next;
};

struct List {
    address First;
};

void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertFirst(List &L, address P);
address findElm(List L, infotype x);
int sumInfo(List L);

#endif

File: Singlylist.cpp

#include "Singlylist.h"

void CreateList(List &L) {
    L.First = Nil;
}

address alokasi(infotype x) {
    address P = new Elmlist;

    P->info = x;
    P->next = Nil;

    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.First;

    while (P != Nil) {
        cout << P->info;

        if (P->next != Nil) {
            cout << " ";
        }

        P = P->next;
    }

    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.First;
    L.First = P;
}

address findElm(List L, infotype x) {
    address P = L.First;

    while (P != Nil) {
        if (P->info == x) {
            return P;
        }

        P = P->next;
    }

    return Nil;
}

int sumInfo(List L) {
    int total = 0;
    address P = L.First;

    while (P != Nil) {
        total += P->info;
        P = P->next;
    }

    return total;
}
File: main.cpp
#include "Singlylist.h"

int main() {
    List L;
    address P1, P2, P3, P4, P5;
    address hasil;
    int total;

    CreateList(L);

    P1 = alokasi(2);
    insertFirst(L, P1);

    P2 = alokasi(0);
    insertFirst(L, P2);

    P3 = alokasi(8);
    insertFirst(L, P3);

    P4 = alokasi(12);
    insertFirst(L, P4);

    P5 = alokasi(9);
    insertFirst(L, P5);

    cout << "Isi Singly Linked List: ";
    printInfo(L);

    hasil = findElm(L, 8);

    if (hasil != Nil) {
        cout << "8 ditemukan dalam list" << endl;
    } else {
        cout << "8 tidak ditemukan dalam list" << endl;
    }

    total = sumInfo(L);

    cout << "Total info dari kelima elemen adalah "
         << total << endl;

    return 0;
}
```
Fungsi `sumInfo` berfungsi untuk menghitung total semua nilai yang ada di dalam node Singly Linked List. Perhitungan dimulai dengan mengatur variabel `total` menjadi 0 dan menggunakan variabel bantu `P` untuk merujuk pada node pertama melalui `First`. Selanjutnya, program menjelajahi setiap node sampai mencapai `Nil`. Setiap kali sebuah node dikunjungi, nilai `info` akan diakumulasikan ke dalam variabel `total`, dan kemudian pointer `P` akan bergerak ke node selanjutnya dengan menggunakan `P = P->next`. Proses ini akan dilanjutkan hingga semua elemen dalam list telah diperiksa. Dalam program ini, angka-angka yang ada pada daftar adalah 9, 12, 8, 0, dan 2. Setelah menjumlahkan semua nilai, didapatkan hasil 9 + 12 + 8 + 0 + 2 = 31. Hasil penjumlahan itu selanjutnya dikembalikan oleh fungsi `sumInfo` dan ditampilkan oleh program melalui pesan `Total info dari lima elemen adalah 31`. Dengan demikian, fungsi `sumInfo` berhasil menghitung jumlah total dari semua node yang ada dalam list sesuai dengan hasil yang diharapkan pada masalah.

## Kesimpulan
Berdasarkan praktik Modul 5, dapat disimpulkan bahwa Singly Linked List adalah struktur data dinamis yang terdiri atas beberapa node yang saling terhubung dengan pointer `next`. Pada penerapannya menggunakan C++, program dirancang dengan konsep Abstract Data Type (ADT) melalui pemisahan file `Singlylist.h` untuk deklarasi tipe data dan prototipe fungsi, `Singlylist.cpp` untuk implementasi fungsi-fungsi yang telah dideklarasikan, serta `main.cpp` sebagai program utama untuk menjalankan dan menguji operasi yang dibuat. Dalam praktikum ini, 
