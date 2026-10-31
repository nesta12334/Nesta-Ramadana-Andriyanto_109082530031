# <h1 align="center">Laporan Praktikum Modul 6 - Doubly Linked List (Bagian Pertama)</h1>
<p align="center">Nesta Ramadana Andriyanto - 109082530031</p>

## Dasar Teori

Doubly Linked List merupakan salah satu struktur data linear yang setiap elemennya memiliki dua pointer, yaitu `next` dan `prev`. Pointer `next` digunakan untuk menunjuk elemen berikutnya, sedangkan pointer `prev` digunakan untuk menunjuk elemen sebelumnya. Struktur ini memungkinkan proses penelusuran data dilakukan secara dua arah, baik dari depan ke belakang maupun sebaliknya.

### A. Doubly Linked List<br/>
Doubly Linked List adalah jenis linked list yang setiap node-nya memiliki dua pointer, yaitu `next` dan `prev`. Pointer next menunjuk ke node berikutnya, sedangkan `prev` menunjuk ke node sebelumnya. Dengan demikian, penelusuran data dapat dilakukan dua arah, yaitu dari awal ke akhir maupun sebaliknya.
#### 1. Node (ElmList)
Setiap elemen (node) terdiri dari tiga bagian, yaitu info yang menyimpan data, next yang menyimpan alamat node selanjutnya, dan `prev`prev yang menyimpan alamat node sebelumnya. Pada modul ini, info bertipe infotype, sedangkan address merupakan pointer ke `ElmList`.
#### 2. List dan Pointer First & Last
List direpresentasikan menggunakan sebuah struct List yang memiliki dua field, yaitu first dan last. Pointer first menunjuk ke node pertama, sedangkan last menunjuk ke node terakhir. List dinyatakan kosong apabila first bernilai Nil (NULL). Node pertama dan terakhir dihubungkan menggunakan pointer next dan prev.
#### 3. Abstract Data Type (ADT)
Operasi-operasi pada Doubly Linked List dikumpulkan dalam sebuah ADT yang terdiri dari file header (`*.h`) berisi deklarasi tipe dan prototype, file implementasi (`*.cpp`) berisi isi fungsi dan prosedur, serta file `main.cpp` untuk menguji ADT tersebut. Struktur ini memisahkan definisi dari penggunaannya sehingga kode lebih rapi dan mudah digunakan kembali.  
### B. Operasi Dasar Singly Linked List<br/>
Operasi dasar pada Doubly Linked List meliputi pembuatan list, alokasi dan dealokasi node, penambahan elemen, penghapusan elemen, pencarian, update, serta penelusuran seluruh elemen. Operasi tersebut digunakan untuk mengelola data dengan memanfaatkan dua pointer, yaitu next dan prev.
#### 1. Alokasi dan Dealokasi
Fungsi `alokasi` membuat node baru dengan `new`, mengisi `info`, dan mengatur `next` ke `Nil`. Prosedur `Dealokasi`  digunakan untuk menghapus node dari memori setelah tidak digunakan lagi.
#### 2. Insert (Penambahan elemen)
`insert` merupakan proses menambahkan elemen baru ke dalam Doubly Linked List.Operasi insert terdiri dari insertFirst untuk menambahkan elemen di awal list, insertLast untuk menambahkan elemen di akhir list, insertAfter untuk menambahkan elemen setelah posisi tertentu, dan insertBefore untuk menambahkan elemen sebelum posisi tertentu. Setiap penambahan harus mengatur pointer next dan prev agar hubungan antar-node tetap terhubung.
#### 3. Delete (penghapus elemen)
`delete` merupakan proses menghapus elemen dari Doubly Linked List. Operasi ini terdiri dari `deleteFirst` untuk menghapus elemen pertama, `deleteLast` untuk menghapus elemen terakhir, `deleteAfter` untuk menghapus elemen setelah posisi tertentu, dan `deleteBefore` untuk menghapus elemen sebelum posisi tertentu. Setelah penghapusan, pointer harus diperbarui agar struktur list tetap terhubung.
#### 4. Searching, Update, dan View
`Searching` merupakan proses mencari elemen berdasarkan informasi tertentu. `Update` digunakan untuk mengubah data pada elemen, sedangkan `View` digunakan untuk menampilkan data yang tersimpan dalam list. Pada Doubly Linked List, proses akses elemen lebih mudah karena penelusuran dapat dilakukan dua arah menggunakan pointer `next` dan `prev`.

## Guided
### 1.Doublylist.h
```c++
#ifndef DOUBLYLIST_H
#define DOUBLYLIST_H

#include <iostream>
#include <string>

#define Nil NULL

// Latihan 1
struct infotype {
    std::string nopol; 
    std::string warna; 
    int thnBuat;      
};
typedef struct elmlist *address;
struct elmlist {
    infotype info;  
    address next;   
    address prev;   
};
struct List {
    address First; 
    address Last;  
};


// Latihan 1
void CreateList(List &L);
address alokasi(infotype x);
void dealokasi(address &P);
void printInfo(List L);
void insertLast(List &L, address P);

// Latihan 2

address findElm(List L, std::string nopol);

// Latihan 3
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(List &L, address Prec, address &P);
void deleteElm(List &L, std::string nopol, bool &status);

#endif

```
Penjelasan Guided 1
Penjelasan Guided 1 Doublylist.h adalah header ADT Doubly Linked List untuk data kendaraan (nomor polisi, warna, dan tahun pembuatan) yang mendefinisikan struktur data kendaraan, node dengan pointer `next` dan `prev`, serta list dengan pointer `First` dan `Last`. File ini juga berisi prototype operasi pembuatan list, alokasi dan dealokasi node, penambahan elemen di akhir list, penampilan data, pencarian kendaraan berdasarkan nomor polisi, serta penghapusan elemen pertama, terakhir, setelah posisi tertentu, dan berdasarkan nomor polisi.

### 2.Doublylist.cpp
```c++
#include "Doublylist.h"

// Latihan 1
void CreateList(List &L) {
    L.First = Nil;
    L.Last = Nil;
}

address alokasi(infotype x) {
    address P = new elmlist;
    P->info = x;
    P->next = Nil;
    P->prev = Nil; 
    return P;
}

void dealokasi(address &P) {
    delete P;
}

void printInfo(List L) {
    address P = L.Last;
    if (P == Nil) {
        std::cout << "List kosong." << std::endl;
        return;
    }
    while (P != Nil) {
        std::cout << "no polisi : " << P->info.nopol << std::endl;   
        std::cout << "warna     : " << P->info.warna << std::endl;   
        std::cout << "tahun     : " << P->info.thnBuat << std::endl; 
        P = P->prev;
    }
}

void insertLast(List &L, address P) {
    if (L.First == Nil) { 
        L.First = P;
        L.Last = P;
    } else {
        P->prev = L.Last;         
        (L.Last)->next = P;     
        L.Last = P;               
    }
}

// Latihan 2
address findElm(List L, std::string nopol) {
    address P = L.First;
    while (P != Nil) {
        if (P->info.nopol == nopol) {
            return P;
        }
        P = P->next;
    }
    return Nil;
}

// Latihan 3
void deleteFirst(List &L, address &P) {
    if (L.First == Nil) return;

    P = L.First; 
    if (L.First == L.Last) { 
        L.First = Nil;
        L.Last = Nil;
    } else {
        L.First = P->next;     
        (L.First)->prev = Nil; 
        P->next = Nil;         
    }
}

void deleteLast(List &L, address &P) {
    if (L.First == Nil) return;

    P = L.Last; 
    if (L.First == L.Last) { 
        L.First = Nil;
        L.Last = Nil;
    } else {
        L.Last = P->prev;      
        (L.Last)->next = Nil; 
        P->prev = Nil;         
    }
}

void deleteAfter(List &L, address Prec, address &P) {
    if (Prec == Nil || Prec->next == Nil) return;

    P = Prec->next;
    if (P == L.Last) {
        deleteLast(L, P);
    } else {
        Prec->next = P->next;     
        (P->next)->prev = Prec; 
        P->next = Nil;            
        P->prev = Nil;           
    }
}

void deleteElm(List &L, std::string nopol, bool &status) {
    address P = findElm(L, nopol);
    status = false;

    if (P != Nil) {
        if (P == L.First) {
            deleteFirst(L, P);
        } else if (P == L.Last) {
            deleteLast(L, P);
        } else {
            address Prec = P->prev;
            deleteAfter(L, Prec, P);
        }
        dealokasi(P); 
        status = true;
    }
}
```
Penjelasan guided 2
Penjelasan Guided 1 Doublylist.cpp adalah file implementasi ADT Doubly Linked List untuk mengelola data kendaraan (nomor polisi, warna, dan tahun pembuatan). File ini berisi fungsi `CreateList` untuk membuat list kosong, `alokasi` dan `dealokasi` untuk membuat serta menghapus node, `insertLast` untuk menambahkan data di akhir list, dan `printInfo` untuk menampilkan data kendaraan dari belakang ke depan menggunakan pointer `prev`. Selain itu, terdapat fungsi `findElm` untuk mencari kendaraan berdasarkan nomor polisi, serta prosedur `deleteFirst`, `deleteLast`, `deleteAfter`, dan `deleteElm` untuk menghapus data kendaraan berdasarkan posisi maupun nomor polisi.

### 3.main.cpp
```c++
#include <iostream>
#include "Doublylist.h"
using namespace std;

void inputData(List &L) {
    infotype data;
    address P;

    cout << "masukkan nomor polisi: "; cin >> data.nopol;
    cout << "masukkan warna kendaraan: "; cin >> data.warna;
    cout << "masukkan tahun kendaraan: "; cin >> data.thnBuat;
    if (findElm(L, data.nopol) == Nil) {
        P = alokasi(data); 
        insertLast(L, P);  
    } else {
        cout << "nomor polisi sudah terdaftar" << endl; 
    }
}

int main() {
    List L;
    CreateList(L); 
    address P;

    // Latihan 1
    inputData(L); 
    inputData(L); 
    inputData(L); 
    inputData(L); 

    cout << "\nDATA LIST 1" << endl; 
    printInfo(L); 

    // Latihan 2
    string nopolCari;
    cout << "\nMasukkan Nomor Polisi yang dicari : "; 
    cin >> nopolCari; 
    
    P = findElm(L, nopolCari); 
    if (P != Nil) {
        cout << "Nomor Polisi : " << P->info.nopol << endl; 
        cout << "Warna        : " << P->info.warna << endl; 
        cout << "Tahun        : " << P->info.thnBuat << endl; 
    } else {
        cout << "Data dengan nomor polisi " << nopolCari << " tidak ditemukan." << endl;
    }

    // Latihan 3
    string nopolHapus;
    bool hapusBerhasil;
    cout << "\nMasukkan Nomor Polisi yang akan dihapus : ";
    cin >> nopolHapus;

    deleteElm(L, nopolHapus, hapusBerhasil); 

    if (hapusBerhasil) {
        cout << "Data dengan nomor polisi " << nopolHapus << " berhasil dihapus." << endl; 
    } else {
        cout << "Data dengan nomor polisi " << nopolHapus << " tidak ditemukan." << endl;
    }
    cout << "\nDATA LIST 1" << endl; 
    printInfo(L); 

    return 0;
}
  ```
  Penjelasan guided 3
  Penjelasan Guided 1 main.cpp adalah file utama untuk menjalankan dan menguji implementasi ADT Doubly Linked List pada data kendaraan (nomor polisi, warna, dan tahun pembuatan). File ini memiliki fungsi `inputData` untuk memasukkan data kendaraan serta memeriksa agar nomor polisi tidak terdaftar lebih dari satu kali. Pada fungsi `main`, program membuat list kosong menggunakan `CreateList`, kemudian memasukkan empat data kendaraan dan menampilkannya menggunakan `printInfo`. Selanjutnya, program menjalankan fungsi `findElm` untuk mencari kendaraan berdasarkan nomor polisi dan `deleteElm` untuk menghapus data kendaraan berdasarkan nomor polisi. Setelah proses penghapusan, program menampilkan kembali data kendaraan untuk melihat hasil perubahan list.

## Unguided

### 1. Buatlah ADT Doubly Linked List pada file "Doublylist.h", implementasinya pada file "Doublylist.cpp", lalu cobalah pada file "main.cpp" dengan data kendaraan (nomor polisi, warna, dan tahun pembuatan).

**buku.h**
```C++
#ifndef BUKU_H
#define BUKU_H

#include <iostream>
#include <string>

#define Nil NULL

struct infotype {
    std::string kodeBuku;
    std::string judul;
    int thnTerbit;
};

typedef struct elmlist *address;

struct elmlist {
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
void printInfo(List L);
void insertLast(List &L, address P);

#endif
```

**buku.cpp**
```C++
#include "Singlylist.h"

void CreateList(List &L) {
    L.first = Nil;
}

address alokasi(infotype x) {
    address P = new ElmList;
    P->info = x;
    P->next = Nil;
    return P;
}

void dealokasi(address &P) {
    delete P;
    P = Nil;
}

void printInfo(List L) {
    address P = L.first;
    while (P != Nil) {
        cout << P->info << " ";
        P = P->next;
    }
    cout << endl;
}

void insertFirst(List &L, address P) {
    P->next = L.first;
    L.first = P;
}
```

**main.cpp**
```C++
#include <iostream>
#include "Buku.h"

using namespace std;

void inputData(List &L) {
    infotype data;
    address P, Q;

    cout << "masukkan kode buku: ";
    cin >> data.kodeBuku;

    cout << "masukkan judul buku: ";
    cin >> data.judul;

    cout << "masukkan tahun terbit: ";
    cin >> data.thnTerbit;

    Q = L.First;

    while (Q != Nil &&
           Q->info.kodeBuku != data.kodeBuku) {
        Q = Q->next;
    }

    if (Q == Nil) {
        P = alokasi(data);
        insertLast(L, P);
    } else {
        cout << "kode buku sudah terdaftar" << endl;
    }
}

int main() {
    List L;
    CreateList(L);

    inputData(L);
    inputData(L);
    inputData(L);
    inputData(L);

    cout << "\nDATA LIST 1" << endl;
    printInfo(L);

    return 0;
}
```
### Output Unguided 1 :

##### Output 1
<img width="577" height="435" alt="Screenshot 2026-10-31 115109" src="https://github.com/user-attachments/assets/d9a5e3a5-21d9-40ec-9809-b6fbb4d191d2" />


Program membuat list kosong dengan `CreateList`, lalu memasukkan empat data buku menggunakan `inputData` dan `insertLast`. Program melakukan pengecekan kode buku agar tidak terjadi duplikasi. Jika kode buku sudah terdaftar, data tidak dimasukkan dan menampilkan pesan "kode buku sudah terdaftar". Fungsi `printInfo` menelusuri list dari `Last` menggunakan pointer `prev` sehingga data ditampilkan dari belakang ke depan.

### 2. Carilah elemen berdasarkan kode B001 dengan membuat fungsi baru findElm.

Tambahan prototype pada **buku.h**
```C++
address findElm(List L, infotype x);
```

Tambahan implementasi pada **buku.cpp**
```C++
address findElm(List L, infotype x) {
    address P = L.First;

    while (P != Nil) {
        if (P->info.kodeBuku == x.kodeBuku) {
            return P;
        }
        P = P->next;
    }

    return Nil;
}
```

Tambahan pada **main.cpp** (setelah `printInfo(L);`)
```C++
infotype cari;

cout << "\nMasukkan Kode Buku yang dicari : ";
cin >> cari.kodeBuku;

address P = findElm(L, cari);

if (P != Nil) {
    cout << "Kode Buku    : " << P->info.kodeBuku << endl;
    cout << "Judul        : " << P->info.judul << endl;
    cout << "Tahun Terbit : " << P->info.thnTerbit << endl;
} else {
    cout << "Data tidak ditemukan." << endl;
}
```
### Output Unguided 2 :

##### Output 2
<img width="579" height="539" alt="Screenshot 2026-10-31 115239" src="https://github.com/user-attachments/assets/53291664-609e-4a46-826b-bfa6a4b3e8da" />


Program menggunakan fungsi `findElm` untuk mencari buku berdasarkan kode buku yang dimasukkan. Pencarian dilakukan dengan menelusuri list dari `First` menggunakan pointer `next` hingga kode buku ditemukan atau mencapai `Nil`. Jika ditemukan, program menampilkan kode buku, judul, dan tahun terbit sesuai data yang dicari.

### 3. Hapus elemen buku dengan kode buku B003 menggunakan prosedur deleteFirst, deleteLast, dan deleteAfter.

Tambahan prototype pada **buku.h**
```C++
void deleteFirst(List &L, address &P);
void deleteLast(List &L, address &P);
void deleteAfter(address Prec, address &P);
void deleteElm(List &L, std::string kodeBuku, bool &status);

```

Tambahan implementasi pada **buku.cpp**
```C++
// Latihan 3
void deleteFirst(List &L, address &P) {
    if (L.First == Nil) {
        P = Nil;
        return;
    }

    P = L.First;

    if (L.First == L.Last) {
        L.First = Nil;
        L.Last = Nil;
    } else {
        L.First = P->next;
        L.First->prev = Nil;
        P->next = Nil;
    }
}

void deleteLast(List &L, address &P) {
    if (L.Last == Nil) {
        P = Nil;
        return;
    }

    P = L.Last;

    if (L.First == L.Last) {
        L.First = Nil;
        L.Last = Nil;
    } else {
        L.Last = P->prev;
        L.Last->next = Nil;
        P->prev = Nil;
    }
}

void deleteAfter(address Prec, address &P) {
    if (Prec == Nil || Prec->next == Nil) {
        P = Nil;
        return;
    }

    P = Prec->next;
    Prec->next = P->next;

    if (P->next != Nil) {
        P->next->prev = Prec;
    }

    P->next = Nil;
    P->prev = Nil;
}

void deleteElm(List &L, std::string kodeBuku, bool &status) {
    address P = findElm(L, infotype{kodeBuku, "", 0});
    status = false;

    if (P != Nil) {
        if (P == L.First) {
            deleteFirst(L, P);
        } else if (P == L.Last) {
            deleteLast(L, P);
        } else {
            address Prec = P->prev;
            deleteAfter(Prec, P);
        }

        dealokasi(P);
        status = true;
    }
}
```

Tambahan pada **main.cpp** (setelah proses pencarian)
```C++
string kodeHapus;
bool hapusBerhasil;

cout << "\nMasukkan Kode Buku yang akan dihapus : ";
cin >> kodeHapus;

deleteElm(L, kodeHapus, hapusBerhasil);

if (hapusBerhasil) {
    cout << "Data dengan kode buku "
         << kodeHapus << " berhasil dihapus." << endl;
} else {
    cout << "Data dengan kode buku "
         << kodeHapus << " tidak ditemukan." << endl;
}

cout << "\nDATA LIST 1" << endl;
printInfo(L);
```
### Output Unguided 3 :

##### Output 3
<img width="573" height="717" alt="Screenshot 2026-10-31 115327" src="https://github.com/user-attachments/assets/f5fca336-8834-4b18-bd09-368e5dac65c3" />


Program menggunakan fungsi `deleteElm` untuk menghapus data buku berdasarkan kode buku yang dimasukkan. Proses penghapusan dilakukan dengan `deleteFirst` jika data berada di awal, `deleteLast` jika berada di akhir, dan `deleteAfter` jika berada di tengah list. Setelah data dihapus, fungsi `dealokasi` membebaskan memori. Program kemudian menampilkan pesan keberhasilan dan menampilkan kembali data buku yang tersisa.

## Kesimpulan
Berdasarkan praktikum Modul 6, Doubly Linked List merupakan struktur data yang setiap elemennya memiliki pointer next dan prev untuk menghubungkan data sebelumnya dan sesudahnya. Melalui program ini, kita dapat membuat list, menambahkan data buku, mencari buku berdasarkan kode, serta menghapus data dari awal, akhir, maupun tengah list. Pengecekan kode buku duplikat juga dilakukan agar tidak ada data dengan kode yang sama. Dengan demikian, Doubly Linked List memudahkan proses pengelolaan data secara lebih fleksibel.

## Referensi
[1] BINUS University. (2017). “Doubly Linked List”. School of Computer Science. https://socs.binus.ac.id/2017/03/15/doubly-linked-list/

[2] Wijoyo, A., Prayudi, L. A., Fiqih, M., Santoso, R. D., Putra, R. D. T. S., Arifin, T., & Farhan, A. (2024). “Penggunaan Algoritma Doubly Linked List Untuk Insertion dan Deletion”. JRIIN: Jurnal Riset Informatika dan Inovasi, 1(12). https://garuda.kemdiktisaintek.go.id/documents/detail/4287537

[3] Tim Dosen Struktur Data. (t.t.). Modul 06 Struktur Data: Doubly Linked List (Bagian Pertama). Modul Praktikum Struktur Data, hlm. 51–58.

[4] Northern Illinois University. (t.t.). “Doubly-Linked List Insertion and Deletion”. Department of Computer Science. https://faculty.cs.niu.edu/~mcmahon/CS241/Notes/Data_Structures/doubly_linked.html
