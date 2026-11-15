# <h1 align="center">Laporan Praktikum Modul 7 - Stack </h1>
<p align="center">Nesta Ramadana Andriyanto - 109082530031</p>

## Dasar Teori

Stack merupakan salah satu struktur data linear yang prinsip kerjanya seperti tumpukan. Elemen yang bisa diambil terlebih dahulu adalah elemen yang berada paling atas, yaitu elemen yang terakhir kali dimasukkan. Prinsip ini disebut LIFO (Last In First Out). Seluruh akses pada stack hanya dilakukan melalui satu pintu, yaitu elemen paling atas yang disebut `Top`.

### A. Stack<br/>
Komponen stack pada dasarnya sama dengan komponen pada Singly Linked List, hanya saja akses data hanya dapat dilakukan pada awal stack. Stack dapat direpresentasikan dengan dua cara, yaitu menggunakan pointer dan menggunakan tabel (array).
#### 1. Representasi Pointer
Pada representasi pointer, setiap elemen (`ElmStack`) terdiri dari `info` yang menyimpan data dan `next` yang menyimpan alamat elemen di bawahnya. Stack hanya memiliki satu field, yaitu `Top`, yang menunjuk ke elemen paling atas. Stack dinyatakan kosong apabila `Top` bernilai `Nil`. Karena memakai pointer, jumlah elemen tidak terbatas selama memori masih tersedia.
#### 2. Representasi Tabel
Pada representasi tabel, stack dibuat menggunakan array berindeks sehingga jumlah tumpukan terbatas sampai indeks maksimum (`Imax`). Posisi elemen teratas dicatat oleh `Top`. Pada modul ini stack dinyatakan kosong apabila `Top = 0`. Representasi tabel tidak memerlukan manajemen memori (alokasi dan dealokasi) seperti pada representasi pointer.
#### 3. Abstract Data Type (ADT)
Operasi-operasi pada Stack dikumpulkan dalam sebuah ADT yang terdiri dari file header (`*.h`) berisi deklarasi tipe dan prototype, file implementasi (`*.cpp`) berisi isi fungsi dan prosedur, serta file `main.cpp` untuk menguji ADT tersebut. 
### B. Operasi Dasar Stack<br/>
Stack memiliki dua operasi utama, yaitu `Push` dan `Pop`. Selain itu terdapat primitif-primitif pendukung seperti `createStack`, `isEmpty`, `alokasi`, `dealokasi`, dan fungsi pencarian. Primitif pada stack lebih sedikit dibanding list lain karena operasi hanya dilakukan pada elemen paling atas.
#### 1. Push
`Push` adalah operasi menyisipkan elemen ke dalam tumpukan. Pada representasi pointer, `Push` sama dengan `insert First` pada list, yaitu `P->next = S.Top` kemudian `S.Top = P`. Pada representasi tabel, `Push` dilakukan dengan menggeser `Top` ke indeks berikutnya (`Top = Top + 1`) lalu mengisi data pada indeks tersebut.
#### 2. Pop 
`Pop` adalah operasi mengambil elemen paling atas dari tumpukan. Pada representasi pointer, `Pop` sama dengan `delete First` pada list, yaitu `P = S.Top` kemudian `S.Top = (S.Top)->next`. Pada representasi tabel, data pada indeks `Top` disimpan terlebih dahulu `(Temp = S.Info[Top])` lalu `Top` bergeser ke indeks sebelumnya `(Top = Top - 1)`. 
#### 3. Primitif Lainnya
`createStack` membentuk stack kosong, `isEmpty` memeriksa apakah stack kosong, dan `viewStack` atau `printInfo` menampilkan seluruh isi stack. Pada representasi pointer ditambahkan `alokasi` dan `dealokasi` untuk mengelola memori node.

## Guided
### 1.Stack.h
```c++
#ifndef STACK_H_INCLUDED
#define STACK_H_INCLUDED

#include <iostream>
using namespace std;

struct infotype {
    char nim[20];
    char nama[20];
};
struct Stack {
    infotype info[10];
    int Top;
};

/* prototype */
/******** pengecekan apakah Stack kosong *********/
int isEmpty(Stack S);
/* mengembalikan nilai 0 jika stack kosong */

/******** pembuatan Stack ********/
void createStack(Stack &S);
/* I.S. sembarang
   F.S. terbentuk stack dengan Top=0 */

/******** penambahan elemen pada Stack ********/
void push(Stack &S, infotype X);
/* I.S. Stack mungkin kosong
   F.S. menambahkan elemen pada stack dengan nilai X, TOP=TOP+1 */

/******** penghapusan elemen pada Stack ********/
void pop(Stack &S, infotype &X);
/* I.S. stack tidak kosong
   F.S. nilai info pada indeks TOP disimpan pada X, kemudian TOP=TOP-1 */

/******** proses semua elemen Stack *********/
void viewStack(Stack S);
/* I.S. stack mungkin kosong
   F.S. jika stack tidak kosong menampilkan semua info yang ada pada stack
*/
#endif

```
Penjelasan Guided 1
stack.h adalah header ADT Stack dengan representasi tabel untuk data mahasiswa (NIM dan nama). File ini mendefinisikan `infotype`, struct `Stack` yang berisi array `info` berukuran `Imax` (10) dan `Top`, serta prototype operasi `isEmpty`, `isFull`, `createStack`, `Push`, `Pop`, dan `viewStack`. `Stack` kosong ditandai dengan `Top = 0`.

### 2.Stack.cpp
```c++
#include "stack.h"

int isEmpty(Stack S) {
    /* Top = 0 berarti kosong, sesuai komentar di stack.h dikembalikan 0 */
    if (S.Top == 0) {
        return 0;
    } else {
        return 1;
    }
}

void createStack(Stack &S) {
    S.Top = 0;
}

void push(Stack &S, infotype X) {
    if (S.Top < 10) {
        S.Top = S.Top + 1;
        S.info[S.Top - 1] = X;   /* Top 1..10 dipetakan ke indeks array 0..9 */
    } else {
        cout << "Stack penuh, data tidak bisa ditambahkan" << endl;
    }
}

void pop(Stack &S, infotype &X) {
    if (isEmpty(S) != 0) {
        X = S.info[S.Top - 1];
        S.Top = S.Top - 1;
    } else {
        cout << "Stack kosong, tidak ada data yang bisa diambil" << endl;
    }
}

void viewStack(Stack S) {
    if (isEmpty(S) == 0) {
        cout << "Stack kosong" << endl;
    } else {
        cout << "Isi stack (dari TOP):" << endl;
        for (int i = S.Top; i >= 1; i--) {
            cout << i << ". " << S.info[i - 1].nim << " - "
                 << S.info[i - 1].nama << endl;
        }
    }
}
```
Penjelasan guided 2
stack.cpp adalah implementasi ADT Stack representasi tabel. `createStack` mengatur `Top = 0`, `isEmpty` dan `isFull` memeriksa kondisi stack, `Push` menambah `Top` lalu menyimpan data pada indeks teratas, dan `Pop` menyimpan data teratas ke variabel `X` lalu mengurangi `Top`. `viewStack` menampilkan isi stack dari elemen paling atas sampai paling bawah sesuai prinsip LIFO. Karena `Top` menyatakan jumlah elemen, elemen teratas berada di `info[Top - 1]`.

### 3.main.cpp
```c++
#include <iostream>
#include <cstring>
#include "stack.h"
using namespace std;

int main() {
    Stack S;
    infotype data, hasil;

    createStack(S);
    cout << "Stack baru dibuat, Top = " << S.Top << endl;
    viewStack(S);

    cout << endl << "--- Push 3 data ---" << endl;
    strcpy(data.nim, "103112400001");
    strcpy(data.nama, "Andi");
    push(S, data);

    strcpy(data.nim, "103112400002");
    strcpy(data.nama, "Budi");
    push(S, data);

    strcpy(data.nim, "103112400003");
    strcpy(data.nama, "Citra");
    push(S, data);
    viewStack(S);

    cout << endl << "--- Pop 1 data ---" << endl;
    pop(S, hasil);
    cout << "Data yang diambil: " << hasil.nim << " - " << hasil.nama << endl;
    viewStack(S);

    cout << endl << "--- Pop sampai kosong ---" << endl;
    pop(S, hasil);
    pop(S, hasil);
    pop(S, hasil);
    viewStack(S);

    return 0;
}

  ```
  Penjelasan guided 3
main.cpp menguji ADT Stack dengan membuat stack kosong, memasukkan tiga data mahasiswa menggunakan `Push`, lalu menampilkannya dengan `viewStack`. Setelah itu program melakukan `Pop` sehingga data yang terakhir masuk (Sari) yang keluar lebih dulu, kemudian menampilkan isi stack setelah `Pop`.

## Unguided

### 1. Buatlah ADT Stack menggunakan ARRAY pada file "stack.h", implementasinya pada file "stack.cpp", lalu cobalah pada file "main.cpp".

**stack.h**
```C++
#ifndef STACK_H
#define STACK_H

#include <iostream>

typedef int infotype;

struct Stack {
    infotype info[20];
    int top;
};

void createStack(Stack &S);
int isEmpty(Stack S);
int isFull(Stack S);
void push(Stack &S, infotype x);
infotype pop(Stack &S);
void printInfo(Stack S);
void balikStack(Stack &S);

#endif
```

**stack.cpp**
```C++
#include "stack.h"

using namespace std;

void createStack(Stack &S) {
    S.top = -1;
}

int isEmpty(Stack S) {
    return S.top == -1;
}

int isFull(Stack S) {
    return S.top == 19;
}

void push(Stack &S, infotype x) {
    if (isFull(S)) {
        cout << "Stack penuh" << endl;
    } else {
        S.top++;
        S.info[S.top] = x;
    }
}

infotype pop(Stack &S) {
    infotype x = -1;
    if (isEmpty(S)) {
        cout << "Stack kosong" << endl;
    } else {
        x = S.info[S.top];
        S.top--;
    }
    return x;
}

void printInfo(Stack S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    int i = 0;
    int j = S.top;
    infotype temp;

    while (i < j) {
        temp = S.info[i];
        S.info[i] = S.info[j];
        S.info[j] = temp;
        i++;
        j--;
    }
}
```

**main.cpp**
```C++
#include "stack.h"

using namespace std;

void createStack(Stack &S) {
    S.top = -1;
}

int isEmpty(Stack S) {
    return S.top == -1;
}

int isFull(Stack S) {
    return S.top == 19;
}

void push(Stack &S, infotype x) {
    if (isFull(S)) {
        cout << "Stack penuh" << endl;
    } else {
        S.top++;
        S.info[S.top] = x;
    }
}

infotype pop(Stack &S) {
    infotype x = -1;
    if (isEmpty(S)) {
        cout << "Stack kosong" << endl;
    } else {
        x = S.info[S.top];
        S.top--;
    }
    return x;
}

void printInfo(Stack S) {
    cout << "[TOP] ";
    for (int i = S.top; i >= 0; i--) {
        cout << S.info[i] << " ";
    }
    cout << endl;
}

void balikStack(Stack &S) {
    int i = 0;
    int j = S.top;
    infotype temp;

    while (i < j) {
        temp = S.info[i];
        S.info[i] = S.info[j];
        S.info[j] = temp;
        i++;
        j--;
    }
}
```
### Output Unguided 1 :

##### Output 1
<img width="718" height="104" alt="Screenshot 2026-11-15 202627" src="https://github.com/user-attachments/assets/2846fba7-c168-41d3-b146-241b14535f2e" />


Program membuat stack kosong dengan `createStack`, lalu menjalankan urutan `Push` dan `Pop`. Setelah `Push` 3, 4, 8 dan `Pop` (8 keluar), dilanjutkan `Push` 2, 3, kemudian `Pop` (3 keluar) dan `Push` 9, sehingga isi stack dari bawah ke atas adalah 3, 4, 2, 9. `printInfo` menampilkan isi dari `Top` ke bawah sehingga hasilnya [TOP] 9 2 4 3. Prosedur `balikStack` membalik urutan isi stack dengan menukar elemen dari ujung bawah dan ujung atas hingga bertemu di tengah, sehingga hasil cetak berikutnya adalah [TOP] 3 4 2 9.

### 2. Tambahkan prosedur pushAscending( in/out S : Stack, in x : integer

Tambahan prototype pada **stack.h**
```C++
void pushAscending(Stack &S, infotype x);
```

Tambahan implementasi pada **stack.cpp**
```C++
void pushAscending(Stack &S, infotype x) {
    Stack temp;
    createStack(temp);

    while (!isEmpty(S) && S.info[S.top] > x) {
        push(temp, pop(S));
    }

    push(S, x);

    while (!isEmpty(temp)) {
        push(S, pop(temp));
    }
}
```

Tambahan pada **main.cpp** (setelah `printInfo(L);`)
```C++
#include <iostream>
#include "stack.h"

using namespace std;

int main() {
    cout << "Hello world!" << endl;

    Stack S;
    createStack(S);

    pushAscending(S, 3);
    pushAscending(S, 4);
    pushAscending(S, 8);
    pushAscending(S, 2);
    pushAscending(S, 3);
    pushAscending(S, 9);

    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);

    return 0;
}
```
### Output Unguided 2 :

##### Output 2
<img width="717" height="103" alt="Screenshot 2026-11-15 203850" src="https://github.com/user-attachments/assets/4ee4ee21-eb26-42d3-ad63-f917026d5efd" />

Prosedur pushAscending memasukkan data sehingga isi stack selalu terurut naik dari bawah ke atas. Elemen di atas yang lebih besar dari x dipindahkan sementara ke stack bantu (temp) menggunakan pop dan push, kemudian x dimasukkan ke stack utama, lalu elemen pada temp dikembalikan. Dengan data 3, 4, 8, 2, 3, 9, isi stack dari bawah ke atas menjadi 2, 3, 3, 4, 8, 9 sehingga printInfo menampilkan [TOP] 9 8 4 3 3 2, dan setelah balikStack menjadi [TOP] 2 3 3 4 8 9.

### 3. Tambahkan prosedur getInputStream( in/out S : Stack ). Prosedur akan terus membaca dan menerima input user dan memasukkan setiap input ke dalam stack hingga user menekan tombol enter.

Tambahan prototype pada **stack.h**
```C++
void getInputStream(Stack &S);
```

Tambahan implementasi pada **stack.cpp**
```C++
void getInputStream(Stack &S) {
    char c = cin.get();

    while (c != '\n') {
        if (c >= '0' && c <= '9') {
            push(S, c - '0');
        }
        c = cin.get();
    }
}
```

Tambahan pada **main.cpp** 
```C++
#include <iostream>
#include "stack.h"

using namespace std;

int main() {
    cout << "Hello world!" << endl;

    Stack S;
    createStack(S);

    getInputStream(S);
    printInfo(S);
    cout << "balik stack" << endl;
    balikStack(S);
    printInfo(S);

    return 0;
}
```
### Output Unguided 3 :

##### Output 3
<img width="575" height="107" alt="Screenshot 2026-11-15 204250" src="https://github.com/user-attachments/assets/5b47e172-ddec-4389-b07f-457edb288fa5" />


Prosedur getInputStream membaca input satu karakter demi satu menggunakan cin.get() dan berhenti ketika user menekan tombol enter ('\n'). Setiap karakter angka diubah menjadi bilangan dengan c - '0' lalu dimasukkan ke stack menggunakan push. Pada input 4729601, angka pertama (4) berada di dasar stack dan angka terakhir (1) berada di Top, sehingga printInfo menampilkan [TOP] 1 0 6 9 2 7 4. Setelah balikStack, urutannya menjadi [TOP] 4 7 2 9 6 0 1.

## Kesimpulan
Berdasarkan praktikum Modul 7, Stack merupakan struktur data yang menerapkan prinsip LIFO (Last In First Out), di mana penambahan dan pengambilan data hanya dilakukan pada elemen paling atas (Top). Operasi utama pada stack adalah Push untuk menambah data dan Pop untuk mengambil data. Stack dapat direpresentasikan menggunakan pointer maupun tabel (array). Representasi tabel lebih sederhana karena tidak memerlukan alokasi memori, tetapi jumlah datanya terbatas. Melalui praktikum ini, stack berhasil diimplementasikan dengan array, ditambah prosedur balikStack, pushAscending, dan getInputStream untuk mengolah isi stack dengan lebih fleksibel.

## Referensi
[1] Tim Dosen Struktur Data. (t.t.). Modul 07 Struktur Data: Stack. Modul Praktikum Struktur Data, hlm. 59-65.
