# Push to New Repository on Github

## Create new Repo
1. Buka akun github kalian dan klik button `+` lalu `New Repository`
2. Isikan nama repository kalian
3. Pilih `Public` pada section di bawahnya
4. Langsung klik button `Create Repository` tanpa mencentang pilihan yang ada di atasnya
5. Repository Kosong Baru berhasil dibuat

## Push project to Repo
1. Setelah membuat repo baru, kalian akan disajikan halaman `Quict Setup`
2. Copy link repo github kalian yang tertera pada halamn tersebut
3. Buka terminal/cmd/powershell
4. Masuk ke folder project yang ingin kalian push
5. Setelah berada pada folder project lakukan inisialisasi git repository dengan command:
```bash
$ git init
```
6. Setelah itu set up project untuk push ke repo github yang baru kalian buat dengan command:
```bash
$ git remote add origin https://github.com/musfiratiratih/test-repository.git
```
7. Setelah push dengan command:
```bash
$ git add .
$ git commit -m "Initial Commit"
$ git push -u origin master
```
8. Pesan "Initial Commit" bisa diubah sesuai dengan kebutuhan (Ex. "Update file index.js")
9. Jika kalian melakukan update pada project kalian, kalian dapat langsung push mengikuti langkah pada nomor 7. Sesuaikan pesannya sesuai perubahan kode kalian.
