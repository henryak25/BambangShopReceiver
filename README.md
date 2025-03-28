# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create SubscriberRequest model struct.`
    -   [v] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [v] Commit: `Implement add function in Notification repository.`
    -   [v] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [v] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Commit: `Implement receive_notification function in Notification service.`
    -   [v] Commit: `Implement receive function in Notification controller.`
    -   [v] Commit: `Implement list_messages function in Notification service.`
    -   [v] Commit: `Implement list function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?<br>
    Karena RWLock memungkinkan multiple thread untuk membaca data secara bersamaan, sekaligus hanya membiarkan satu thread yang boleh melakukan write (saat write tidak boleh ada yang read). Sedangkan Mutex memastikan hanya satu thread yang bisa mengakses data, baik itu read atau write.

<br><br>

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?<br>
    Rust dirancang untuk menjamin keamanan memori dan thread safety tanpa bergantung pada garbage collector seperti di java, sehingga harus lebih ketat dalam mengontrol akses ke data yang dibagikan antar-thread. Dengan begitu Rust tidak mengizinkan kita memutasi isi dari static variable karena hal tersebut tidak thread safe. lazy_static membuat variabel yang diinisialisasi secara lazy (saat pertama kali diakses) dan menjamin thread safety, dimana hal tersebut bekerja seperti Singleton pattern.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.<br>
Ya, saya telah menjelajahi bagian src/lib.rs. File ini berisi berbagai komponen penting yang digunakan oleh bagian lain dalam aplikasi, seperti konfigurasi aplikasi, respons kesalahan, dan root URL. Salah satu bagian yang menarik dalam lib.rs adalah penggunaan lazy_static!, yang memungkinkan inisialisasi statis untuk objek yang biasanya memerlukan alokasi dinamis. Contohnya, REQWEST_CLIENT adalah instance dari Client yang dibuat menggunakan ClientBuilder::new().build().unwrap(), sehingga dapat digunakan secara global tanpa perlu membuat instance baru setiap kali dibutuhkan. Begitu juga dengan APP_CONFIG, yang dibuat menggunakan AppConfig::generate(), memastikan bahwa konfigurasi aplikasi hanya diinisialisasi sekali dan dapat diakses dari berbagai bagian program tanpa harus memuat ulang atau membuat instance baru.
```rust
lazy_static! {
    pub static ref REQWEST_CLIENT: Client = ClientBuilder::new().build().unwrap();
    pub static ref APP_CONFIG: AppConfig = AppConfig::generate();
}
```

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?<br>
Penerapan Observer pattern dalam sistem ini sangat membantu dalam menambahkan subscriber baru karena sistem dirancang dengan prinsip open-closed, sehingga kita dapat menambahkan Observer baru tanpa harus mengubah kode yang sudah ada. Setiap subscriber cukup didaftarkan ke publisher, dan mereka akan menerima notifikasi secara otomatis. Jika kita ingin menjalankan lebih dari satu instance Main App, hal ini tetap memungkinkan, tetapi setiap instance aplikasi utama harus memiliki daftar subscriber-nya sendiri. Untuk menambahkan subscriber ke aplikasi yang berbeda, kita dapat melakukannya dengan memanggil API yang sesuai, sehingga setiap instance tetap dapat mengelola subscriber mereka secara independen.

<br><br>

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).<br>
Ya, saya telah mencoba melakukan testing dan menambahkan dokumentasi pada Postman collection. Fitur ini sangat berguna dalam menguji apakah aplikasi berjalan sesuai harapan, baik untuk proyek tutorial maupun proyek kelompok. Dengan melakukan pengujian, kita dapat memastikan bahwa setiap permintaan yang dikirim ke API memberikan respons yang benar dan sesuai dengan data yang ada di aplikasi. Selain itu, Postman collection memudahkan anggota tim untuk memahami cara kerja API dan bagaimana setiap endpoint berfungsi, sehingga kolaborasi menjadi lebih efisien.







