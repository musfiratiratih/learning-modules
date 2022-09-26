# TODO APP
Contoh aplikasi sederhana untuk personal task management

## Stack
- Node Express v4.^
- MySQL / MariaDB

## Guide
1. Buat folder yang akan digunakan untuk menampun semua code pada project ini. Contoh: "simple-todo-app"
2. Jalankan command `npm init -y` didalam folder "simple-todo-app"
3. Npm akan membuat file `package.json`
4. Edit file `package.json` dengan menambahkan key `"type": "module"` setelah key `"main: "index.js"`, dan ganti isi `script` seperti berikut.
```json
{
  "name": "simple-todo-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "dev": "nodemon ."
  },
  "author": "",
  "license": "ISC"
}

```
5. `"type": "module"` akan menginisisi project ini menggunakan [js module](https://id.javascript.info/modules-intro).
6. `"dev": "nodemon ."` akan membuat shortcut untuk menjalankan program menggunakan [nodemon](https://www.npmjs.com/package/nodemon) dengan script `npm run dev`
7. Selanjutnya install semua dependencies yang dibutuhkan.
```bash
npm install express express-session express-flash ejs mysql2 dayjs bcrypt
```
```bash
npm install -D nodemon
```
8. `express` adalah [framework](https://expressjs.com/) nodejs yang digunakan di sini. Berfungsi untuk membantu / mempercepat proses development, karena fungsi-fungsi standart sudah tersedia dan siap digunakan.
9. `express-session` adalah [library](https://www.npmjs.com/package/express-session) yang digunakan untuk menyiapkan session storage diserver.
10. `express-flash` adalah [library](https://www.npmjs.com/package/express-flash) yang digunakan untuk mengirim data pada session yang bersifat sementara. pada project ini digunakan untuk menampilkan `info` sukses ada gagal saat menjalankan suatu aksi
11. `ejs` adalah [view engine](https://ejs.co/) yang memudahkan kita untuk membuat halaman html yang dinamis berisi data / konten yang kita siapkan di server
12. `mysql2` adalah [driver](https://www.npmjs.com/package/mysql2) mysql terbaru yang digunakan untuk menguhungkan nodejs dengan database mysql
13. `dayjs` adalah [library](https://day.js.org/) untuk melakukan manipulasi tanggal dan waktu
14. `bcrypt` adalah [library](https://www.npmjs.com/package/bcrypt) untuk melakukan hashing menggunakan [bcrypt](https://en.wikipedia.org/wiki/Bcrypt)
15. `nodemon` adalah [library](https://www.npmjs.com/package/nodemon) yang digunakan untuk menajalankan dev server
16. Selanjutkan kita siapkan entry point dari project ini pada file file `index.js`
```js
import express, { urlencoded } from "express";
import flash from 'express-flash';
import session from 'express-session';
import { authController } from "./controllers/auth.controller.js";
import { tasksController } from "./controllers/tasks.controller.js";
import { authMiddleware } from "./middlewares/auth.middleware.js";

// inisiasi aplikasi express
const app = express();

// digunakan untuk set view engine menggunakan ejs
app.set("view engine", "ejs");

// digunakan untuk menyiapkan session storage
app.use(session({
	secret: 'f*&23b87&^S%sdfs',
	saveUninitialized: false,
	resave: false,
}))

// digunakan untuk menyiapkan flash message
app.use(flash())

// digunakan untuk menangkap form request yang diinput oleh user
app.use(urlencoded({ extended: true }));

// registrasi route-route yang digunakan pada aplikasi
app.get("/", (req, res) => {
	// render file ejs pada folder views/index.ejs
	return res.render('index');
});

// memulai aplikasi pada port 3000
app.listen(3000, () => console.log("app running on http://localhost:3000"));
```
17. Selanjutkan kita siapkan file html pada folder `views` dengan nama `index.ejs`
<script src="https://gist.github.com/musfiratiratih/1440a67b69ef4d98da2241388dec59c5.js"></script>
