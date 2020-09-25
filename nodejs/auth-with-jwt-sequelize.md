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
|- routes
   |- home.js
|- index.js
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
|- routes
   |- auth.js
   |- home.js
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

## Penggunaan JSON Web Token untuk Restrict Endpoint

Dalam dunia kerja, banyak penyedia layanan yang menggunakan API sebagai produk mereka. Dan harus ada restriction terhadap beberapa Endpoint untuk keperluan commercial/private app/dsb.

- Install package `jsonwebtoken`

```
$ npm install jsonwebtoken
```

- Modifikasi `routes/auth.js` seperti di bawah ini

```js
const express = require('express')
const { sign } = require('jsonwebtoken') // <--
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

    token = sign(credentials, 'verysecretkey') // <--
    
    return res.send({ // <--
        token: token,
        data: credentials
    })
})

module.exports = router
```

- Buat file baru pada root directory dengan nama `verify-token.js`

```
|- routes/
|- index.js
|- verify-token.js
```

- Tuliskan potongan kode berikut (boleh tanpa komentar)

```js
const { verify } = require("jsonwebtoken")

module.exports = function (req, res, next) {
    // mendapatkan token dari header
    const authHeader = req.headers['authorization']

    // memecah Bearer token
    const token = authHeader && authHeader.split(' ')[1]
    
    // cek jika token tidak ada
    if (token == null) return res.sendStatus(401)

    verify(token, 'verysecretkey', function (err, user) {
        console.log(err)
        if (err) return res.sendStatus(403)

        // menyimpan data user ke request
        req.user = user

        // lanjut eksekusi route yang sebenarnya
        next()
    })
}
```

- Modifikasi `routes/home.js`

```js
const express = require('express')
const verifyToken = require('./../verify-token') // <--
const router = express.Router()

router.get('/', function (req, res) {
    res.send('Selamat datang di Perpustakaan')
})

router.get('/home', verifyToken, function (req, res) { // <--
    res.send({
        user: req.user
    })
})

module.exports = router
```

- Lakukan ujicoba pada postman dengan mengakses `/home`
- Tambahkan Auth dengan cara menuju pada tab `Auth` lalu pilih tipe autentikasi `Bearer Token`
- Masukan token hasil dari login ke field token
- Jika token sesuai maka akan mengirim response data user
- Jika token tidak sesuai maka mengirim response `Forbidden`
- Jika token tidak ada maka mengirim response `Unauthorize`


## Controller

Penggunaan controller ditujukan agar code lebih mudah di baca dan maintain. Controller sendiri berisi logic utama dari sebuah end point.

- Buat folder baru pada root directory dengan nama `controllers`

```
|- controllers/
|- routes/
|- index.js
|- verify-token.js
```

### Home Controller

- Buat file baru pada folder `controllers` dengan nama `home.js`

```
|- controllers/
   |- home.js
```

- Buat method yang siap di export pada `home.js` dengan name `homeController`

```js
module.exports = {
    index(req, res) {
        return res.send('Selamat datang di Perpustakaan')
    }
}
```

- Modifikasi `routes/home.js` untuk menggunakan controller

```js
const express = require('express')

const verifyToken = require('./../verify-token')
const controller = require('./../controllers/home') // <--

const router = express.Router()

router.get('/', controller.index) // <--

router.get('/home', verifyToken, function (req, res) {
    res.send({
        user: req.user
    })
})

module.exports = router
```

- Lakukan ujicoba terlebih dahulu pada postman
- Lakukan hal yang sama untuk route `/home`
- Modifikasi `/controllers/home.js`

```js
module.exports = {
    index(req, res) {
        return res.send('Selamat datang di Perpustakaan')
    },

    home(req, res) { // <--
        return res.send({
            user: req.user
        })
    }
}
```

- Modifikasi `/routes/home.js`

```js
const express = require('express')

const verifyToken = require('./../verify-token')
const controller = require('./../controllers/home')

const router = express.Router()

router.get('/', controller.index)

router.get('/home', verifyToken, controller.home) // <--

module.exports = router
```

- Ujicoba route `http://localhost:3000/home`
- Lanjutkan dengan dengan file route lainnya
- File baru pada folder `controllers` dengan nama `auth.js`

```
|- controllers/
   |- home.js
   |- auth.js
```

- Pindah logic yang ada di `/routes/auth.js` ke `/controllers/auth.js` seperti dibawah

```js
const { sign } = require("jsonwebtoken")

module.exports = {
    login(req, res) {
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
    
        token = sign(credentials, 'verysecretkey')
        
        return res.send({
            token: token,
            data: credentials
        })
    }
}
```

- Hapus logic pada file `/routes/auth.js`

```js
const express = require('express')

const controller = require('./../controllers/auth')

const router = express.Router()

router.post('/login', controller.login)

module.exports = router
```

## Registrasi User

- Buat route baru pada file `/routes/auth.js` dengan endpoint `/register` dan arahkan ke `controler.register`

```js
const express = require('express')

const controller = require('./../controllers/auth')

const router = express.Router()

router.post('/login', controller.login)
router.post('/register', controller.register)

module.exports = router
```

- Tambah method controller baru pada file `/controllers/auth.js` dengan nama `register` lalu tuliskan kode berikut

```js
const { sign } = require("jsonwebtoken")

module.exports = {
    login(req, res) {
        ...
    },

    register(req, res) {
        let user = {
            name: req.body.name,
            username: req.body.username,
            password: req.body.password,
            address: req.body.address
        }

        return res.send({
            data: user
        })
    }
}
```

- Ujicoba endpoint melalui postman

## Menyimpan User ke DB dengan MySql Sequelize

### Konfigurasi MySql dan Sequelize

Pada modul ini akan menggunakan database `mysql` dengan package `mysql2`. Pastikan MySql sudah terinstall dan siapkan username dan password. Default dari username MySql pada XAMPP adalah

| username | password |
|---|---|
| root |  |

- Install package yang dibutuhkan

```
$ npm install mysql2 sequelize
```

- Install sequelize-cli secara global agar mempermudah inisialisasi sequelize

```
$ npm install -g sequelize-cli
```

- Inisialisasi sequelize

```
$ sequelize init
```

- Perhatikan perubahan yang terjadi pada project

```
|- config/
   |- config.json
|- controllers/
|- migrations/
|- models/
   |- index.js
|- routes/
|- seeders/
|- index.js
|- verify-token.js
```

- Buka file `/config/config.json` dan sesuaikan dengan koneksi ke database kalian
- Contoh modifikasi file `/config/config.json`

```json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "node_perpustakaan",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "node_perpustakaan",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "node_perpustakaan",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

- Buat database dengan nama sesuai dengan pada file `/config/config.json`
- Selanjutnya membuat model User dengan command berikut

```
$ sequelize model:create --name User --attributes name:string,username:string,password:string,address:text
```

- Command tersebut akan membuat dua file baru pada folder `/models` dan `/migrations`
- Modifikasi file `/migrations/<timestamp>-create-user.js` untuk mendefiniskan kolom unik dan kolom yang tidak boleh `null`

```js
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('Users', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      name: {
        allowNull: false, // <--
        type: Sequelize.STRING
      },
      username: {
        allowNull: false, // <--
        type: Sequelize.STRING,
        unique: true // <--
      },
      password: {
        allowNull: false, // <--
        type: Sequelize.STRING
      },
      address: {
        allowNull: false, // <--
        type: Sequelize.TEXT
      },
      createdAt: {
        allowNull: false,
        type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('Users');
  }
};
```

- Setelah itu lakukan migrasi untuk membuat table user pada database

```
$ sequelize db:migrate
```

- Setelah command berhasil, pastikan table `Users` sudah terbuat di database
- Pada tahap ini proses konfigurasi sudah selesai

### Menyimpan User ke Database

- Modifikasi file `/controllers/auth.js` pada method `register` untuk menyimpan user ke database

```js
const { sign } = require("jsonwebtoken")
const models = require('./../models') // <--

module.exports = {
    login(req, res) {
        ...
    },

    async register(req, res) { // <--
        let user = await models.User.create({ // <--
            name: req.body.name,
            username: req.body.username,
            password: req.body.password,
            address: req.body.address
        })

        return res.send({
            data: user
        })
    }
}
```

- Lakukan ujicoba pada postman, lalu cek data pada table User.

## Login berdasarkan User di Database

- Modifikasi file `/controllers/auth.js` pada method `auth` untuk mendapatkan user dari database dan melakukan validasi token

```js
const { sign } = require("jsonwebtoken")
const models = require('./../models')

module.exports = {
    async login(req, res) { // <--
        let credentials = {
            username: req.body.username,
            password: req.body.password
        }
    
        if (!credentials.username || !credentials.password) {
            return res.send('Username dan/atau Password harus diisi!')
        }
    
        let user = await models.User.findOne({ // <--
            where: credentials
        })

        if (!user) return res.send('Username dan/atau Password tidak sesuai') // <--
    
        token = sign(credentials, 'verysecretkey')
        
        return res.send({
            token: token,
            data: credentials
        })
    },

    async register(req, res) {
        ...
    }
}
```

- Lakukan ujicoba di postman dan route yang membutuhkan token 
