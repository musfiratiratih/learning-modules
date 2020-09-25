# Modul Auth dengan JWT dan Sequelize
Modul ini menggunakan VSCode sebagai code editor utama. Hal-hal yang harus sudah ada pada system adalah:
1. Nodejs (cek dengan `node -v` dan `npm -v`)
2. Mysql (XAMPP, Laragon, WAMP, Native, etc)

## Inisialisasi Project & Aplikasi
- Buatlah folder project baru dengan nama `perpustakaan`
- Buka folder project di VSCode
- Inisialiasi project melalui terminal pada VSCode
```
$ npm init -y
```
- Gunakan `-y` untuk skip confirmasi pada saat inisialisasi project
- Install Express
```
$ npm install express
```
- Buat file `index.js` pada root directory
- Tuliskan code berikut untuk inisialisasi aplikasi
```js
const express = require('express')
const app = express()

app.listent(3000, function (){
  console.log('aplikasi berjalan pada port 3000')
})
```
- Jalankan index.js dengan command
```
$ node index
```
- Jika memberikan output "aplikasi berjalan pada port 3000" maka aplikasi sudah siap

## Menggunakn Nodemon
Penggunaan nodemon bisa meringankan pekerjaan untuk build aplikasi.
- Install nodemon secara global dengan comman
```
$ npm install -g nodemon
```
- Ujicoba nodemon pada project `perpustakaan`
```
$ nodemon
```
Pada step ini, setiap kali ada perubahan pada project `perpustakaan` makan nodemon akan otomatis rebuild aplikasi

## Membuat Route Pertama
- Buat folder `routes` dan file `home.js`
```
perpustakaan /
|-routes
  |-home.js
|-index.js
```
- Tambahkan potongan kode berikut pada `routes/routes.js`
```js
const express = require('express')
const router = express.Router()

router.get('/', function (req, res) {
  res.send('Selamat datang di Perpustakaan')
})

module.exports = router
```
- Tambahkan potngan kode berikut pada `/index.js`
```js
const express = require('express')
const app = express()

const homeRoutes = require('./routes/home')

app.use('/', homeRoutes)

app.listen(3000, function () {
  console.log('Aplikasi berjalan pada port 3000')
})
```
- Uji coba request `http://localhost:3000/` pada postman
