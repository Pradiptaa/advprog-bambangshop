# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam pola desain Observer, seperti yang dijelaskan dalam buku Head First Design Pattern, interface Subscriber memungkinkan fleksibilitas karena objek yang berbeda dapat menjadi pelanggan selama mereka mengimplementasikan interface ini. Dalam kasus BambangShop, menggunakan interface masih diperlukan untuk menjaga desain yang fleksibel dan dapat diperluas, daripada hanya menggunakan satu model struct. 
2. karena id di Program dan url di Subscriber harus unik, menggunakan DashMap lebih efektif dibandingkan hanya menggunakan Vec, karena mempermudah pencarian dan akses data yang unik tanpa perlu iterasi manual. 
3. meskipun Rust menerapkan pembatasan kompiler yang ketat untuk memastikan keselamatan thread, penggunaan DashMap pada variabel static SUBSCRIBERS lebih tepat daripada mengimplementasikan pola Singleton karena DashMap sudah menyediakan fungsionalitas HashMap yang aman thread, yang cocok untuk kasus dengan akses konkuren tanpa memerlukan mekanisme locking manual yang kompleks.

#### Reflection Publisher-2
1. Memisahkan Service dan Repository dari Model dalam Model-View-Controller (MVC) berguna untuk mempertahankan prinsip pemrograman SOLID, khususnya Single Responsibility Principle. Model yang hanya fokus pada data dan logika bisnis menjadi lebih sederhana dan mudah diurus. Repository bertanggung jawab untuk semua interaksi dengan basis data, sedangkan Service mengelola logika bisnis yang kompleks antar model tanpa langsung berhubungan dengan detail penyimpanan data. Pendekatan ini mendukung pemisahan kekhawatiran yang lebih baik dan memfasilitasi testing yang lebih mudah serta pemeliharaan kode.
2. Jika hanya menggunakan Model dalam desain software, akan terjadi peningkatan kompleksitas kode karena setiap Model (Program, Subscriber, Notification) perlu menangani lebih dari satu kekhawatiran seperti logika bisnis dan interaksi dengan penyimpanan data. Hal ini bisa menyebabkan kode yang sulit untuk diuji dan dipelihara karena setiap model menjadi terlalu berat dan tidak fleksibel. 
3. Postman adalah alat yang sangat berguna untuk menguji API dalam pengembangan software. Alat ini memungkinkan saya untuk mengirim permintaan HTTP ke API, melihat respons, dan bahkan menjalankan serangkaian tes otomatis terhadap respons tersebut. Fitur-fitur seperti Collections, yang memungkinkan saya menyimpan dan mengelola permintaan yang sering digunakan, Environment untuk mengelola variabel konfigurasi yang berbeda antara lingkungan pengembangan dan produksi, serta fitur Mock Servers dan Monitors yang membantu dalam pengujian dan pemantauan API secara real-time, sangat membantu untuk kebutuhan testing. 

#### Reflection Publisher-3
1. Tutorial ini menggunakan varian Push model dari Observer Pattern. Dalam model ini, publisher bertanggung jawab untuk mengirimkan (push) data atau pemberitahuan secara langsung ke semua subscribers tanpa perlu meminta atau menarik (pull) informasi dari publisher. Hal ini memungkinkan publisher untuk secara aktif menginformasikan perubahan kepada semua subscriber yang terdaftar.
2. Menggunakan varian Pull model dari Observer Pattern dalam tutorial ini memiliki beberapa kelebihan dan kekurangan. Kelebihan dari Pull model adalah pengurangan kemungkinan overload pada publisher karena subscriber yang mengambil data sesuai kebutuhan mereka, yang bisa mengoptimalkan penggunaan sumber daya dan bandwidth. Namun, kekurangannya adalah respons terhadap perubahan mungkin tidak segera karena tergantung pada kapan subscriber memutuskan untuk menarik informasi, yang bisa menyebabkan penundaan dalam pembaruan status dan kurangnya sinkronisasi real-time antara publisher dan subscriber.
3. Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, program mungkin mengalami penurunan kinerja secara signifikan terutama saat ada banyak pelanggan yang perlu diberi notifikasi. Tanpa multi-threading, proses pengiriman notifikasi akan dilakukan secara berurutan, yang berarti notifikasi ke pelanggan berikutnya hanya akan dikirim setelah notifikasi sebelumnya selesai diproses. Ini bisa menyebabkan penundaan yang signifikan dalam pengiriman notifikasi, dan dalam skenario dengan banyak pelanggan, ini bisa mengakibatkan waktu tunggu yang lama bagi pelanggan untuk menerima pembaruan mereka. Kondisi ini juga dapat menyebabkan program menjadi tidak responsif jika ada tugas berat lain yang perlu dijalankan secara bersamaan.
