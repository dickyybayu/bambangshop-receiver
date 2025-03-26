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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

    In this tutorial, we used RwLock<> to synchronize access to the Vec of notifications because it allows multiple readers to access the data simultaneously while ensuring that only one writer can modify it at a time. This is particularly useful in a scenario where multiple parts of the system need to read the list of notifications concurrently, but modifications (such as adding a new notification) are less frequent. Using RwLock<> helps improve performance by allowing multiple read operations to happen in parallel, which would not be possible with Mutex<>. A Mutex<>, on the other hand, only allows either a single read or write operation at a time, meaning that every read operation would block other readers, leading to unnecessary contention and reduced efficiency. Since the notifications list is primarily read rather than frequently modified, RwLock<> is a more suitable choice.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

    We also used the lazy_static external library to define Vec and DashMap as static variables because Rust enforces strict ownership and borrowing rules to ensure memory safety and prevent data races. Unlike Java, where static variables can be mutated directly within static functions, Rust does not allow direct mutation of static variables because they exist globally and could lead to undefined behavior due to concurrent access. Rust requires explicit synchronization mechanisms, such as RwLock<> or Mutex<>, to safely mutate shared static data, ensuring that changes happen in a controlled manner. This design choice enforces thread safety at the language level, preventing common concurrency issues such as race conditions and deadlocks.

#### Reflection Subscriber-2

1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

    I have only explored lib.rs and learned that it plays a crucial role in managing global configurations, HTTP client setup, and error handling. It defines REQWEST_CLIENT as a global reqwest::Client to optimize HTTP requests and APP_CONFIG to manage application settings dynamically using lazy_static! and dotenvy. Additionally, lib.rs provides a structured approach to error handling with Result<T, E> and ErrorResponse, ensuring consistency in error messages. However, I have not yet explored other parts of the code beyond what was covered in the tutorial.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

    The Observer pattern simplifies adding more subscribers because it decouples the publisher (Main app) from the subscribers (Receiver instances). Each new subscriber simply registers itself, and the system ensures they receive notifications when an event occurs. Since the notification logic is abstracted in the NotificationService, adding new subscribers does not require modifying the publisher’s code.

    However, if we spawn multiple instances of the Main app, things become more complex. Since each instance manages its own in-memory subscriber list, there is no built-in synchronization across instances. This means that a subscriber registered in one instance might not be recognized by another. To make the system scalable, we would need a shared storage mechanism, such as a database or distributed cache, to ensure all instances have access to the same subscriber list.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

    I have created multiple instances of the Receiver app, each listening on different ports (8001, 8002, and 8003) by modifying the .env file to change the ROCKET_PORT and APP_INSTANCE_NAME variables. After running cargo run in separate terminals for each instance, I also ran one instance of the Main app. Using the provided Postman collection, I tested the subscription process by subscribing different Receiver instances to different product types. After executing the create Product, publish Product, and delete Product endpoints in the Main app, I verified that the correct notifications were received by each Receiver instance. This process confirmed that the Observer pattern effectively allows multiple subscribers to receive updates dynamically. Additionally, by testing different product types, I ensured that notifications were routed correctly to the appropriate Receiver instances. Postman was useful in simplifying the testing process by allowing me to easily send requests and verify responses.