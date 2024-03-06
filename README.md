# Indonesia Rust | Komunitas Pecinta Rust
Komunitas Pecinta <del>Inara Rusli</del> Rust
```rs
fn main() {
    println!("Panduan Pemrograman Rust Bahasa Indonesia");
}
```

1. [Download dan Install rustup](https://www.rust-lang.org/tools/install)  
2. Cek Instalasi dan update
   ```sh
   rustup --version
   rustc --version
   cargo --version
   rustup update
   ```
3. Buat proyek baru dengan cargo dan masuk ke direktori tersebut, init untuk eksisting direktori dan new untuk buat direktori baru
   ```sh
   cargo init inarust
   cargo new inarust
   cd inarust
   code .
   ```
4. Jalankan program atau build dengan perintah :
   ```sh
   cargo build
   cargo run
   ```
5. Untuk membersihkan dan update dependensi dengan perintah :
   ```sh
   cargo clean
   cargo update
   ```

## Web Service dengan Axum

Gunakan [skeleton kami](https://github.com/inarust/inarust) untuk mempermudah hidup anda.
Cukup setting saja database dan port dan tinggal kita coba test dengan menggunakan curl, bisa juga menggunakan postman
```sh
curl http://localhost:3000/
curl http://localhost:3000/users
curl -X POST http://localhost:3000/create-user
curl "http://localhost:3000/item/42?number=2"
curl -X POST http://localhost:3000/add-item \
    -H "Content-Type: application/json" \
    -d '{"title": "Some random item"}'
curl -X DELETE http://localhost:3000/delete-user/1
curl -X DELETE http://localhost:3000/delete-user/2

```

