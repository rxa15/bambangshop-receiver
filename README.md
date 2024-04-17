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
-   ✅ Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   ✅ Commit: `Create Notification model struct.`
    -   ✅ Commit: `Create SubscriberRequest model struct.`
    -   ✅ Commit: `Create Notification database and Notification repository struct skeleton.`
    -   ✅ Commit: `Implement add function in Notification repository.`
    -   ✅ Commit: `Implement list_all_as_string function in Notification repository.`
    -   ✅ Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   ✅ Commit: `Create Notification service struct skeleton.`
    -   ✅ Commit: `Implement subscribe function in Notification service.`
    -   ✅ Commit: `Implement subscribe function in Notification controller.`
    -   ✅ Commit: `Implement unsubscribe function in Notification service.`
    -   ✅ Commit: `Implement unsubscribe function in Notification controller.`
    -   ✅ Commit: `Implement receive_notification function in Notification service.`
    -   ✅ Commit: `Implement receive function in Notification controller.`
    -   ✅ Commit: `Implement list_messages function in Notification service.`
    -   ✅ Commit: `Implement list function in Notification controller.`
    -   ✅ Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

`RwLock<>` and `Mutex<>` are Rust principles used to synchronize data accessed by multiple threads. `RwLock<>` permits multiple threads to read data simultaneously, while `Mutex<>` restricts access to a single thread. In this tutorial, `RwLock<>` is used to allow concurrent read access by multiple threads to Vec Notifications while ensuring exclusive writing. This setup is particularly effective for data structures with higher read than write frequency. Unlike `Mutex<>`, which allows only one thread access at a time, `RwLock<>` accommodates multiple readers or a single writer. Hence, `RwLock<>` is preferable for situations with high read frequency and low write frequency, while Mutex<> excels in scenarios with balanced read and write frequencies.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

In Rust, static variables are by default immutable, meaning their content cannot be mutated once initialized. The `lazy_static` external library in Rust provides a workaround for initializing static variables lazily at runtime, but even with `lazy_static`, once initialized, the content of a static variable remains immutable by default.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

Yes, I have explored some parts from src/lib.rs, for instance the `Default` implementation for `AppConfig`. In the implementation, the default() function defines the default behavior for creating an instance of the AppConfig struct. It returns an instance of AppConfig with predefined default values for its fields:

instance_root_url: Initialized with the string "http://localhost:8001".
publisher_root_url: Initialized with the string "http://localhost:8000".
instance_name: Initialized with the string "BambangShop Receiver".

If an instance of AppConfig is created without explicitly specifying values for its fields, this implementation of Default will be invoked to provide default values for each field. This simplifies the process of creating instances of AppConfig by providing sensible default values that can be overridden if needed.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

The Observer pattern streamlines the process of adding new subscribers to the notification system by offering a flexible and easily scalable architecture. In this model, each subscriber serves as an observer, receiving notifications from the main application, or subject, without necessitating alterations to the system's core functions. It will still be easy enough to spawn more than one instance of Main app because the Observer pattern makes it easy for us to add more subscribers.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

While I have not yet delved into creating my own tests or refining my Postman collection, my familiarity with Postman since last semester has laid a solid groundwork. Nonetheless, I believe that doing so would significantly enhance our ability to monitor the program's progress, particularly for collaborative group projects. Testing would enable each group member to verify whether the program has delivered the expected responses accurately.