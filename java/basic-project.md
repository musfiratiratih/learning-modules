# Membuat Projek Sederhana

Modul ini akan memandu anda dalam membuat projek sederhana menggunkan bahasa pemrograman java. Projek yang akan dibuat pada modul ini adalah aplikasi pemesanan tiket.

## Inisialisasi Projek

Silahkan membuat projek baru dengan nama `ticket-app`. Dapat membuatnya melalui NetBean maupun IDE favorit kalian yang lain.

## Main.java

Untuk pembuatan aplikasi ini kita akan fokus pada file `Main.java` saja. Berikut adalah awalan dari file `Main.java`

```java
public class Main
{
    public static void main(String[] args)
    {
        //
    }
}
```

## Insialisasi variable yang diperlukan

Dalam projek ini kita membutuhkan input dari user menggunakan `Scanner`, data tiket yang tersedian dan harganya, serta variabel untuk menerima input dari user.

```java
public static void main(String[] args)
{
    // Inisialisasi input
    Scanner input = new Scanner(System.in);

    // Inisialisasi data tiket
    // Jumlah data tiket dan harga harus sama
    // Dalam hal ini jumlah tiket dan harga ada 3
    String[] availableTickets = {"Full Akses", "Museum and Zoo", "Theme Park"};
    int[] ticketPrice = {98, 78, 73};

    // Inisialisasi variable untuk menerima input user
    int choice = 0, paid = 0;
    String name;
}
```

## Pesan pembuka

Setelah melakukan inisialisasi, kita bisa langsung membuat pesan pembuka yang akan ditampilkan pada terminal.

```java
public static void main(String[] args)
{
    ...
    // Menampilkan pesan pada terminal
    System.out.println("=======================================================");
    System.out.println("Selamat Datang di Wahana Paket");
    System.out.println("=======================================================");
    System.out.println("");
}
```

## Input Nama

Selanjutnya meminta user untuk meng-input-kan nama mereka

```java
public static void main(String[] args)
{
    ...

    // Meminta input nama dari user
    System.out.println("=======================================================");
    System.out.println("Silahkan masukan nama anda: ");
    name = input.nextLine(); 
    // Penggunaan nextLine() untuk mendapatkan input 1 bari dari user
}
```

## Menampilkan Daftar Tiket dan Harga

Selanjutnya kita menggunakan perulangan `for` untuk menampilkan jumlah tiket yang tersedia. Pastikan jumlah data pada array `availableTickets` dan `ticketPrice` sama.

```java
public static void main(String[] args)
{
    ...

    System.out.println("=======================================================");
    System.out.println("Berikut adalah tiket yang tersedia untuk anda.");

    // Menampilkan seluruh data tiket beserta harganya
    for(int i=0; i<availableTickets.length; i++){
        System.out.println("(" + i + ") " + availableTickets[i] + " => " + ticketPrice[i]);
    }
}
```

## User memilih Tiket

Selanjutnya kita meminta untuk user memilih tiket berdasarkan index array-nya.

```java
public static void main(String[] args)
{
    ...

    // Meminta input pilihan tiket dari user
    System.out.println("=======================================================");
    System.out.println("Silahkan pilih nomor tiket yang anda inginkan.");
    System.out.print("=> ");
    choice = input.nextInt();
    System.out.println("Anda memilih tiket nomor " + choice );
}
```

## User melakukan Pembayaran

Selanjutnya kita meminta user untuk menginput pembayarannya

```java
public static void main(String[] args)
{
    ...

    // Meminta input pembayaran dari user
    System.out.println("=======================================================");
    System.out.println("Silahkan lakukan pembayaran sebesar $" + ticketPrice[choice]);
    System.out.print("=> ");
    paid = input.nextInt();
}
```

## Validasi Pembayaran

Selanjutnya kita melakukan validasi dari pembayaran user, apakan pas, lebih atau kurang.

```java
public static void main(Stringp[] args)
{
    ...

    // Pengkondisian pembayaran untuk mengecek apakah pembayaran berhasil atau tidak
    if(paid > ticketPrice[choice]){
        System.out.println("Ini kembaliannya sebesar $" + (paid - ticketPrice[choice]));
    }else if(paid == ticketPrice[choice]){
        System.out.println("Uang anda pas.");
    }else{
        System.out.println("Uang anda kurang " + (ticketPrice[choice] - paid));
    }
}
```

## Pesan Penutup

Selanjutnya kita bisa menampilkan pesan penutup untuk user. Disini sangat disarankan untuk menutup Scanner dengan fungsi `.close()`. Hal ini bertujuan untuk mengurangi penggunaan memory pada sistem.

```java
public static void main(String[] args)
{
    ...

    // Pesan penutup
    System.out.println("=======================================================");
    System.out.println("Terimakasih atas kunjungannya " + name);
    System.out.println("Silahkan datang kembali. :D");
    System.out.println("=======================================================");

    // Menutup scanner untuk pembebasan ruang memori
    input.close();
}
```

## Hasil Akhir

Pada tahap ini, kita bisa melakukan test pada aplikasi. Berikut hasil akhir yang diberktan di terminal

```
=======================================================
Selamat Datang di Wahana Paket
=======================================================

=======================================================
Silahkan masukan nama anda:
Ahmad Budi
=======================================================
Berikut adalah tiket yang tersedia untuk anda.
(0) Full Akses => 98
(1) Museum and Zoo => 78
(2) Theme Park => 73
=======================================================
Silahkan pilih nomor tiket yang anda inginkan.
=> 1
Anda memilih tiket nomor 1
=======================================================
Silahkan lakukan pembayaran sebesar $78
=> 80
Ini kembaliannya sebesar $2
=======================================================
Terimakasih atas kunjungannya Ahmad Budi
Silahkan datang kembali. :D
=======================================================
```