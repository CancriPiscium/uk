```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

struct barang {
    char nama_barang[50];
    int harga_barang;
    int jumlah_barang;
    double diskon_barang;
};

//Definisi Harga Barang
int harga_barang[] = {5000, 2000, 1000, 1000, 500};

//Variable Global
int index = 0;
struct barang pembelian[100];
int total_jumlah_barang = 0;
int total_harga_barang = 0;
int total_harga_final = 0;
double jumlah_diskon = 0;

//deklarasi fungsi
void pesanBarang(int kode_barang);
void hitungDiskon(struct barang *barang);
void hitungTotal();
void resetData();

//fungsi utama
int main() {
    //Tetapkan Seed Untuk Fungsi rand() menggunakan waktu saat ini
    srand(time(NULL));

    //tampilan awal
    printf("Selamat Datang di Toko SKENSA\n");
    printf("Silahkan Pilih Barang yang Ana Inginkan: \n");
    printf("+=================================+\n");
    printf("| No |     Barang     |   Harga   |\n");
    printf("+=================================+\n");
    printf("|  1 |Buku Tulis      |Rp. 5000   |\n");
    printf("|  2 |Pensil          |Rp. 2000   |\n");
    printf("|  3 |Penghapus       |Rp. 1000   |\n");
    printf("|  4 |Penggaris       |Rp. 1000   |\n");
    printf("|  5 |Bujur Sangkar   |Rp. 500    |\n");
    printf("+=================================+\n\n");
    printf("99. Struk Pembayaran\n");
    printf("55. Reset Pilihan\n");
    printf("00. Keluar\n");
    printf("+=================================+\n\n");

    int kode_barang;
    while(1){
        printf("\n\nInput Pilihan yang Anda Inginkan: ");
        scanf("%d", &kode_barang);

        //handle pilihan user
        if (kode_barang == 99){
            hitungTotal();
            break;
        }else if (kode_barang == 55) {
            system("cls");
            resetData();
            main();
            break;
        }else if (kode_barang == 00) {
            exit(EXIT_SUCCESS);
        }else if (kode_barang >= 1 && kode_barang <= 5){
            pesanBarang(kode_barang);
        }else {
            printf("Menu yang Anda Inputkan Tidak Ada di Dalam Daftar Barang!! \n", kode_barang);
            printf("Silahkan Input Ulang \n\n");
        }
    }

    return 0;
}
//fungsi reset data
void resetData() {
    index = 0;
    total_jumlah_barang = 0;
    total_harga_barang = 0;
    jumlah_diskon = 0;
    total_harga_final = 0;
}

//Fungsi Untuk Menukar 2 elemen
void swap(struct barang *a, struct barang *b){
    struct barang temp = *a;
    *a = *b;
    *b = temp;
}

// Fungsi untuk mengurutkan barang berdasarkan jumlah terbanyak menggunakan Bubble Sort
void sortirBarang(struct barang arr[], int n){
    int i, j;
    for(i = 0; i <n-1; i++){
        for(j = 0; j <n-i-1; j++){
            if (arr[j].jumlah_barang < arr[j+1].jumlah_barang){
                swap(&arr[j], &arr[j+1]);
            }
        }
    }
}

//fungsi untuk memesan barang
void pesanBarang(int kode_barang){
    printf("\nPemesanan: ");
    switch (kode_barang){
    case 1:
        printf("Buku Tulis");
        break;
    case 2:
        printf("Pensil");
        break;
    case 3:
        printf("Penghapus");
        break;
    case 4:
        printf("Penggaris");
        break;
    case 5:
        printf("Bujur Sangkar");
        break;
    }

    //input jumlah barang yang akan dibeli
    printf("\nJumlah Pesan: ");
    scanf("%d", &pembelian[index].jumlah_barang);
    //menyimpan nama barang berdasarkan kode barang
    strcpy(pembelian[index].nama_barang, (kode_barang == 1) ? "Buku Tulis" :
                                         (kode_barang == 2) ? "Pensil" :
                                         (kode_barang == 3) ? "Penghapus" :
                                         (kode_barang == 4) ? "Penggaris" :
                                                              "Bujur Sangkar");
    //mendapatkan harga barang dari array harga_barang
    pembelian[index].harga_barang = harga_barang[kode_barang - 1];
    // Menambahkan jumlah barang yang dibeli dan total harga barang
    total_jumlah_barang += pembelian[index].jumlah_barang;
    total_harga_barang += pembelian[index].jumlah_barang * pembelian[index].harga_barang;
    // Menghitung diskon untuk barang yang dibeli
    hitungDiskon(&pembelian[index]);
    index++;
}


//fungsi untuk menghitung diskon barang
void hitungDiskon(struct barang *barang){
    if(barang->jumlah_barang >=5){
        barang->diskon_barang = barang->harga_barang * barang->jumlah_barang * 0.15;//jika barang lebih dari atau sama dengan 5 akan mendapat diskon sebesar 15%
    }else if(barang->jumlah_barang >=3){
        barang->diskon_barang = barang->harga_barang * barang->jumlah_barang * 0.10;//jika barang lebih dari atau sama dengan 3 akan mendapat diskon sebesar 10%
    }else{
        barang->diskon_barang = 0;//jika barang berjumlah 1 dan 2 tidak akan mendapat diskon
    }

    jumlah_diskon += barang->diskon_barang;// Menambahkan jumlah diskon yang diberikan

}

//fungsi untuk menghirung total pembelian barang
void hitungTotal() {
    sortirBarang(pembelian, index);// Mengurutkan barang berdasarkan jumlah terbanyak

    //Menampilkan rekap pembelian
    printf("\nRekap Pesanan Barang\n");
    printf("+====+========+================+===========+=============+=============+\n");
    printf("| No | Jumlah |     Barang     |   Harga   | Total Harga |   Diskon    |\n");
    printf("+====+========+================+===========+=============+=============+\n");

    for (int i = 0; i < index; i++){
        printf("| %d  |%-6d  |%-16s|Rp. %-9d|Rp. %-7d|Rp. %-8.2lf |\n", i + 1, pembelian[i].jumlah_barang, pembelian[i].nama_barang, pembelian[i].harga_barang,
                                                                       pembelian[i].harga_barang * pembelian[i].jumlah_barang, pembelian[i].diskon_barang);
    }
    printf("+====+========+================+===========+=============+=============+\n\n");

    // Menampilkan total harga barang dan total diskon
    printf("Total Harga    : Rp. %d,-\n", total_harga_barang);
    printf("Total Diskon   : Rp. %.2lf,-\n", jumlah_diskon);

    //menghitung Harga Final setelah Diskon
    int total_harga_final = total_harga_barang - jumlah_diskon;

    //menampilkan Harga Final
    printf("Total Bayar    : Rp. %d,-\n", total_harga_final);
    printf("+======================================================================+\n");

    //input jumlah uang bayar
    int bayar;
    printf("Masukan Jumlah Uang Bayar: ");
    scanf("%d", &bayar);
    if (bayar < total_harga_final){//melakukan perulangan jika uang yang di bayar kurang
        printf("\nUang yang Anda Masukan Kurang, Silahkan Masukan Ulang: ");
        scanf("%d", &bayar);
    }
    int kembalian = bayar - total_harga_final;//menghitung kembalian
    printf("\nKembalian: %d,-\n\n", kembalian);//menampilkan kembalian

    // Menetapkan ID Struk menjadi lima angka dengan membatasi hasil dari rand()
    int id_struk = rand() % 100000; // Membatasi hasil menjadi 5 digit (0-99999)

    time_t t = time(NULL); // Mengambil waktu saat ini
    struct tm tm = *localtime(&t); // Konversi waktu ke dalam struktur tm

    // Membuat nama file dengan format "Struk_IDStruk.txt"
    char nama_file[20];
    sprintf(nama_file, "Struk_%07d.txt", id_struk); // Menggunakan format "%05d" untuk memastikan ada lima digit

    // Menyimpan struk pembayaran ke dalam file teks
    FILE *file = fopen(nama_file, "w");

    // Memeriksa apakah file berhasil dibuka
    if(file != NULL){
        // Menulis detail pembelian ke dalam file
        fprintf(file, "+=================================================================+\n");
        fprintf(file, "|                            TOKO SKENSA                          |\n");
        fprintf(file, "|              Jl. HOS Cokroaminoto No. 84, Denpasar              |\n");
        fprintf(file, "|                               Bali                              |\n");
        fprintf(file, "|                        Telp : 0816285791                        |\n");
        fprintf(file, "|ID Struk : %07d                                               |\n", id_struk); // Menggunakan format "%07d" untuk memastikan ada lima digit
        fprintf(file, "+======+====================+===========+===========+=============+\n");
        fprintf(file, "|Jumlah|     Nama Barang    |   Harga   |   Total   |    Diskon   |\n");
        fprintf(file, "+======+====================+===========+===========+=============+\n");
        for (int i = 0; i < index; i++) {
            fprintf(file, "|%-6d| %-19s| Rp. %-6d| Rp. %-6d| Rp. %-7.2lf |\n",pembelian[i].jumlah_barang, pembelian[i].nama_barang, pembelian[i].harga_barang * pembelian[i].jumlah_barang,
                                                                              total_harga_barang, pembelian[i].diskon_barang);
        }
        fprintf(file, "+======+====================+===========+===========+=============+\n");
        fprintf(file, "|Total Harga Barang        : Rp. %-33d|\n", total_harga_barang);
        fprintf(file, "|Total Jumlah Diskon       : Rp. %-33.2lf|\n", jumlah_diskon);
        fprintf(file, "|Total Harga Final         : Rp. %-33d|\n", total_harga_final);
        fprintf(file, "|Jumlah Harga yang Dibayar : Rp. %-33d|\n", bayar);
        fprintf(file, "|Kembalian                 : Rp. %-33d|\n", kembalian);
        fprintf(file, "|                                                                 |\n");
        fprintf(file, "|                                                                 |\n");
        fprintf(file, "|Waktu/Hari                : %d-%02d-%02d %02d:%02d:%02d                  |\n", tm.tm_year + 1900, tm.tm_mon + 1, tm.tm_mday, tm.tm_hour, tm.tm_min, tm.tm_sec); // Menampilkan waktu saat ini
        fprintf(file, "+=================================================================+\n");

        fclose(file);// Menutup file setelah selesai penulisan
        printf("Struk pembayaran telah disimpan dalam file %s.\n", nama_file);
    } else {
        printf("Gagal menyimpan struk pembayaran.\n");// Menampilkan pesan kesalahan jika gagal menyimpan file
    }
}
