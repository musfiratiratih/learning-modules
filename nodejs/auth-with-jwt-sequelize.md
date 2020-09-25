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

app.listen(3000, function () {
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
const router = require('./routes/home') // <--
const app = express()

app.use('/', router) // <--

app.listen(3000, function () {
    console.log('aplikasi berjalan pada port 3000')
})
```

- Uji coba request `http://localhost:3000/` pada postman
- Balikan yang diberikan adalah seperti berikut

```json
Selamat datang di Perpustakaan
```

## Membuat Route untuk Login

- Buat file `auth.js` pada folder `routes`

```
|-routes
  |-auth.js
  |-home.js
```

- Inisialisasi router express lalu export `router`

```js
const express = require('express')
const router = express.Router()

module.exports = router
```

- Tambahkan route `/login` dengan method post

```js
router.get('/login', function (req, res) {

})
```

- Pada route `/login` simpan request dari user berupa `username` dan `password` dalam variable `credentials`

```js
let credentials = {
    username: req.body.username,
    password: req.body.password
}
```

- Jika `username` atau `password` tidak diisi (null) maka kirim response dengan pesan "Username dan/atau Password harus diisi!"

```js
if (!credentials.username || $credentials.password) {
    return res.send('Username dan/atau Password harus diisi!')
}
```

- Untuk saat ini, lakukan pengecekan jika username dan password adalah "admin"

```js
if (credentials.username != 'admin' || credential.password != 'admin') {
    return res.send('Username dan/atau Password tidak sesuai')
}

return res.send('Selamat datang kembali Administrator')
```

- Berikut adalah potongan kode lengkap untuk file `auth.js`

```js
const express = require('express')
const router = express.Router()

router.post('/login', function (req, res) {
    let credentials = {
        username: req.body.username,
        password: req.body.password
    }

    if (!credentials.username || !credentials.password) {
        return res.send('Username dan/atau Password harus diisi!')
    }

    if (credentials.username != 'admin' || credentials.password != 'admin') {
        return res.send('Username dan/atau Password tidak sesuai')
    }
    
    return res.send('Selamat datang kembali Administrator')
})

module.exports = router
```

- Selanjutnya modifikasi index.js seperti di bawah ini

```js
const express = require('express')
const homeRouter = require('./routes/home') // <--
const authRouter = require('./routes/auth') // <--

const app = express()
app.use(express.json()) // <--

app.use('/', homeRouter) // <--
app.use('/', authRouter) // <--

app.listen(3000, function () {
    console.log('aplikasi berjalan pada port 3000')
})
```

- Route `/login` dapat diakses di postman dengan menggunakan method `POST`
- karena kita hanya menggunakan `express.json()` yang berfungsi untuk mendapatkan request body dari client maka isikan request pada tab `Body -> Raw` lalu pilih tipenya `JSON`

```json
{
    "username": "admin",
    "password": "admin"
}
```

- Silahkan lakuan ujicoba saat `username` kosong atau bukan "admin"
