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
3. Buat proyek baru dengan cargo dan masuk ke direktori tersebut, init untuk eksisting direktori dan new untuk buat direktori baru
   ```sh
   cargo init inarust
   cargo new inarust
   cd inarust
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

pada file rs di folder src
```rs
use axum::{
    body::Body,
    http::StatusCode,
    response::{IntoResponse, Response},
    routing::{get, post,delete},
    extract::{Path, Query},
    Json, Router,
};
use serde::{Serialize,Deserialize};

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

// A struct for query parameters
#[derive(Deserialize)]
struct Page {
    number: u32,
}

// A handler to demonstrate path and query extractors
async fn show_item(Path(id): Path<u32>, Query(page): Query<Page>) -> String {
    format!("Item {} on page {}", id, page.number)
}

// A struct for the JSON body
#[derive(Deserialize)]
struct Item {
    title: String,
}

// A handler to demonstrate the JSON body extractor
async fn add_item(Json(item): Json<Item>) -> String {
    format!("Added item: {}", item.title)
}

#[derive(Serialize)]
struct Userx {
    id: u64,
    name: String,
}

// Define a handler that performs an operation and may return an error
async fn delete_user(Path(user_id): Path<u64>) -> Result<Json<Userx>, impl IntoResponse> {
    match perform_delete_user(user_id).await {
        Ok(_) => Ok(Json(Userx {
            id: user_id,
            name: "Deleted User".into(),
        })),
        Err(e) => Err((
            StatusCode::INTERNAL_SERVER_ERROR,
            format!("Failed to delete user: {}", e),
        )),
    }
}

// Hypothetical async function to delete a user by ID
async fn perform_delete_user(user_id: u64) -> Result<(), String> {
    // Simulate an error for demonstration
    if user_id == 1 {
        Err("User cannot be deleted.".to_string())
    } else {
        // Logic to delete a user...
        Ok(())
    }
}

#[tokio::main]
async fn main() {
    // Define Routes
    let app = Router::new()
        .route("/", get(|| async { "Hello, Rust!" }))
        .route("/create-user", post(create_user))
        .route("/users", get(list_users))
        .route("/item/:id", get(show_item))
        .route("/add-item", post(add_item))
        .route("/delete-user/:user_id", delete(delete_user));

    println!("Running on http://localhost:3000");
    // Start Server
    axum::Server::bind(&"127.0.0.1:3000".parse().unwrap())
        .serve(app.into_make_service())
        .await
        .unwrap();
}
```
kita coba test dengan menggunakan curl, bisa juga menggunakan postman
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

