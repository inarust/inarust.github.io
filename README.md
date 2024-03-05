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

Setiap ada package baru yang di tambahkan di dependensi selalu jalankan cargo build

```sh
cargo build
```

### Hello word website

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

### Routing dan handling

```rs
use axum::{
    body::Body,
    http::StatusCode,
    response::{IntoResponse, Response},
    routing::{get, post},
    Json, Router,
};
use serde::Serialize;

#[derive(Serialize)]
struct User {
    id: u64,
    name: String,
    email: String,
}

// Handler for /create-user
async fn create_user() -> impl IntoResponse {
    Response::builder()
        .status(StatusCode::CREATED)
        .body(Body::from("User created successfully"))
        .unwrap()
}
// Handler for /users
async fn list_users() -> Json<Vec<User>> {
    let users = vec![
        User {
            id: 1,
            name: "Elijah".to_string(),
            email: "elijah@example.com".to_string(),
        },
        User {
            id: 2,
            name: "John".to_string(),
            email: "john@doe.com".to_string(),
        },
    ];
    Json(users)
}

#[tokio::main]
async fn main() {
    // Define Routes
    let app = Router::new()
        .route("/", get(|| async { "Hello, Rust!" }))
        .route("/create-user", post(create_user))
        .route("/users", get(list_users));

    println!("Running on http://localhost:3000");
    // Start Server
    axum::Server::bind(&"127.0.0.1:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}
```

