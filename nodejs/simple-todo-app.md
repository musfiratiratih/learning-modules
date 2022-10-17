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
```html
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>To Do Apps | Express & MySql2</title>

		<script src="https://cdn.tailwindcss.com"></script>
	</head>
	<body class="bg-gray-200">
		<header class="mb-8 flex items-center justify-center py-4 bg-white w-full shadow-xl">
			<h1 class="text-xl font-bold uppercase">Todo App</h1>
		</header>

		<div class="mx-auto flex items-center justify-center max-w-md">
			<div class="flex flex-col items-stretch w-screen">
				<div class="mb-8">
					<p class="text-center text-gray-600">
						Hi, <strong>User</strong>! What do you want todo next?
					</p>
				</div>
				<div class="mb-8">
					<form action="/tasks" method="post">
						<div class="flex items-center">
							<input
								type="text"
								name="task"
								placeholder="Things need todo asap!"
								class="flex-1 px-4 py-2 w-full bg-white rounded-full text-sm focus:outline-none focus:shadow-lg"
							/>
							<button
								class="ml-4 px-4 py-2 rounded-full bg-sky-500 text-white font-bold text-sm uppercase focus:outline-none focus:shadow-lg"
							>
								Add Task
							</button>
						</div>
					</form>
				</div>
				<div class="mb-8">
					<form
						action="/tasks/1"
						method="post"
						class="block flex items-center pl-4 pr-3 py-2 bg-white shadow-lg rounded-full"
					>
						<div class="flex-1 mr-2 text-gray-800">Install expressjs</div>
						<button
							name="action"
							value="done"
							class="text-xs font-bold bg-gray-300 text-gray-500 rounded-full px-2 py-1"
						>
							Mark as Done
						</button>
					</form>
					<form
						action="/tasks/1"
						method="post"
						class="block flex items-center pl-4 pr-3 py-2 bg-white shadow-lg rounded-full"
					>
						<div class="flex-1 mr-2 text-gray-800">Install nodejs</div>
						<button name="action" value="undone" class="text-xs font-bold bg-sky-500 text-white rounded-full px-2 py-1">
							Done!
						</button>
						<button
							name="action"
							value="delete"
							class="ml-2 text-xs font-bold bg-red-500 text-white rounded-full px-2 py-1"
						>
							Delete
						</button>
					</form>
				</div>
				<div class="mb-8">
					<form action="/logout" method="post" class="block text-center">
						<button class="text-red-400 text-sm underline">Logout!</button>
					</form>
				</div>
			</div>
		</div>
	</body>
</html>
```
18. Dari sini, kita dapat mencoba menjalankan project dengan menjalankan `npm run dev`
19. Kemudian buka halaman http://localhost:3000 pada browser