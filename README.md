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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam kasus BambangShop, kita tidak membutuhkan _interface_ atau _trait_ untuk `Subscriber` karena hanya ada satu _observer_ yang terlibat, yaitu `Subscriber` itu sendiri. Biasanya,  _interface_ atau _trait_ digunakan ketika kita berencana untuk memiliki beberapa jenis _observer_ yang berbeda,  yang mana bisa dikelola dalam kelas yang terpisah. Namun, dalam kasus ini, satu _struct_ Model sudah cukup untuk menangani kebutuhan kita. Meskipun begitu, jika kita mempertimbangkan _best practice_, mendefinisikan `Subscriber` sebagai _interface_ akan memberikan  fleksibilitas lebih di masa depan, memastikan bahwa kode ini lebih _scalabel_ serta menjaga penerapan prinsip SOLID. Dengan menggunakan _interface_, kita dapat menambahkan _subscriber_ baru tanpa mengubah kode yang ada dan menjaga ketergantungan sistem agar lebih mudah dipelihara.

> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Dalam kasus ini, penggunaan DashMap diperlukan dan lebih baik dibandingkan dengan Vec untuk menyimpan data `Subscriber` dengan id dan url yang unik, karena DashMap menawarkan efisiensi yang lebih tinggi. Jika menggunakan Vec, maka setiap operasi lookup/pencarian memiliki _time complexity_ sebesar O(n), sedangkan DashMap memungkinkan pencarian dengan kompleksitas waktu O(1), yang artinya waktu pencarian tetap konstan meskipun jumlah `Subscriber` meningkat. Dengan demikian, DashMap tidak hanya mempercepat proses pencarian, tetapi juga membuat pengelolaan data `Subscriber` lebih efektif dan _scalable_, menjadikannya pilihan yang lebih baik dalam kasus ini.

> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Dalam kasus ini, meskipun pola Singleton memastikan hanya ada satu instance objek yang berjalan, hal ini tidak cukup untuk menangani masalah akses bersamaan yang aman dalam konteks _multithreading_. Pola Singleton dapat memastikan bahwa hanya ada satu instance dari `HashMap` yang digunakan, namun tidak menjamin akses yang aman oleh banyak _thread_ secara bersamaan, memungkinkan terjadinya permasalahan _race condition_. Dengan demikian, penggunaan DashMap menjadi penting, karena DashMap mendukung akses bersamaan, memastikan bahwa data dapat diakses oleh beberapa _thread_ secara aman. Oleh karena itu, dalam kasus ini, DashMap dan pola Singleton berjalan secara beriringan, karena pola Singleton memastikan bahwa hanya ada satu instance yang berjalan dan DashMap memastikan _concurrency_ yang aman, sehingga mereka tidak dapat dipisahkan satu sama lain.

#### Reflection Publisher-2

> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Pemisahan Service dan Repository dari Model dalam pola Model-View-Controller (MVC) penting untuk mengikuti prinsip-prinsip design seperti _Single Responsibility Principle_ (SRP). Model dalam MVC hanya bertugas sebagai representasi data dari basis data, sementara Repository bertanggung jawab untuk akses dan penyimpanan data, dan Service menangani logika yang ada. Dengan memisahkan Service dan Repository dari sebuah Model, kode kita akan menjadi lebih mudah dipahami dan mudah dipelihara karena setiap komponen memiliki tanggung jawab yang jelas dan terpisah. Pemisahan ini juga mempermudah pengujian setiap komponen secara terpisah, memungkinkan pengembangan yang lebih fleksibel dan _scalable_ tanpa mengganggu bagian lain dalam kode. Selain itu, perubahan pada satu bagian tidak mempengaruhi bagian lainnya, sehingga kode menjadi lebih mudah dimodifikasi. 

> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika kita hanya menggunakan Model tanpa pembagian seperti Service dan Repository, kita akan menghadapi masalah pada depedensi antara komponen-komponen dalam sistem. Model akan bertanggung jawab untuk menangani baik penyimpnan data maupun logika, yang melanggar prinsip Single Responsibility Principle (SRP). Hal ini menyebabkan kode menjadi sulit untuk dipelihara, karena setiap perubahan pada satu bagian dapat mempengaruhi bagian lain secara signifikan, yang terkadang meningkatkan kompleksitas kode secara keseluruhan serta memperlambat pengembangan lebih lanjut.

> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Saya sudah melakukan eksplorasi terhadap aplikasi Postman ini. Postman merupakan suatu alat yang sangat berguna untuk menguji endpoint aplikasi web menggunakan permintaan HTTP. Dengan Postman, kita dapat menguji endpoint tanpa perlu menulis kode, yang sangat memudahkan dalam menguji fungsionalitas aplikasi secara cepat. Beberapa fitur yang sangat membantu saya selama ini adalah fitur untuk menyimpan _request_ dan _response_, mengontrol _header_ dan _body request_, serta mengelola _cookie_ yang sangat berguna ketika melakukan pengujian terhadap endpoint aplikasi web yang saya miliki. Fitur dokumentasi API juga sangat penting karena memudahkan saya untuk berbagi dokumentasi API dengan teman-teman yang lain. Alat ini akan sangat bermanfaat dalam proyek perangkat lunak saya di masa depan, karena memungkinkan saya melakukan pengujian dengan efisien dan memungkinkan kolaborasi dengan teman-teman saya yang lain.

#### Reflection Publisher-3

> Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada tutorial ini, kita menggunakan variasi Observer Pattern dengan model Push. Dalam model ini, setiap kali terjadi perubahan data, seperti `create`, `update`, atau `delete`, perubahan tersebut akan memicu pemanggilan `NotificationService`. Service ini bertugas untuk mengiterasi seluruh `subscriber` dan mengirimkan pembaruan terbaru tanpa memerlukan permintaan dari `subscriber`. Dengan demikian, `publisher` secara aktif mengirimkan notifikasi kepada `subscriber`, sesuai dengan implementasi yang ada pada fungsi `notify` di `NotificationService`.

> What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan model Pull dalam tutorial ini, terdapat beberapa keuntungan dan kerugian yang perlu dipertimbangkan. Keuntungannya adalah efisiensi dalam penggunakan _resource_ karena data hanya dikirimkan ketika `subscriber` membutuhkannya. Selain itu, `subscriber` memiliki kontrol penuh untuk memilih data yang relevan dan kapan data tersebut diambil. Model ini juga menyederhanakan kode karena mengurangi kompleksitas pada _observable_. Namun, kerugiannya adalah `subscriber` harus terus-menerus memeriska perubahan data pada `publisher`, yang dapat menyebabkan tingginya angka penggunaan CPU dan meningkatkan latensi dalam proses notifikasi.

> Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika kita memutuskan untuk tidak menggunakan _multithreading_ dalam proses notifikasi, maka program akan menghadapi masalah antrian panjang karena `NotificationService` harus memberi tahu setiap `subscriber` secara berurutan. Hal ini dapat menyebabkan _bottleneck_, memperlambat pengiriman notifikasi, dan meningkatkan latensi dalam proses notifikasi. Proses akan terhenti untuk `subscriber` lain ketika mengirimkan notifikasi ke salah satu `subscriber`, yang membuat program menjadi tidak efisien. Masalah ini semakin parah seiring dengan bertambahnya jumlah `subscriber` karena proses akan terus menerus menunggu setiap `subscriber` untuk menerima notifikasi sebelum melanjutkan ke `subscriber` berikutnya, sehingga mengurangi efisiensi dan skalabilitas program.