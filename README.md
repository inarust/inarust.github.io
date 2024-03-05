# Indonesia Rust | Komunitas Pecinta ~Inara Rusli~ Rust

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
3. Buat proyek baru dengan cargo dan masuk ke direktori tersebut
   ```sh
   cargo new main
   cd main
   code .
   ```
4. Jalankan program atau build dengan perintah :
   ```sh
   cargo run
   cargo build
   ```

## Web Service dengan Axum

Edit file Cargo.toml tambahkan pada bagian setelah [dependencies]
```toml
axum = {version = "0.6.20", features = ["headers"]}
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0.68"
tokio = { version = "1.0", features = ["full"] }
```

setelah itu build

```sh
cargo build
```

### Hello word axum

```rs
use axum::{routing::get, Router};

#[tokio::main]
async fn main() {
    let app = Router::new().route("/", get(|| async { "Hello, Rust!" }));

    println!("Running on http://localhost:3000");
    axum::Server::bind(&"0.0.0.0:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}
```

