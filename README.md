Notification Service

Overview:
A simple Spring Boot microservice that demonstrates sending and receiving messages via RabbitMQ.

Built with Spring Boot 3.5.15 and Java 17.
Uses spring-boot-starter-amqp to produce/consume messages.
On startup the service sends a test message to the queue and consumes messages from the same queue.
Key behaviors:

Queue name: notification.queue
At startup a message ("Hello from spring boot ! This is my first event!") is sent to the queue.
A consumer listens on the queue and prints received messages to stdout.
Prerequisites:

Java 17 installed and JAVA_HOME set.
Maven 3.x installed.
Running RabbitMQ broker (default config uses localhost:5672 with guest/guest).
Configuration:

Edit the RabbitMQ connection properties in src/main/resources/application.properties:
spring.rabbitmq.host
spring.rabbitmq.port
spring.rabbitmq.username
spring.rabbitmq.password
Build:

From the project root run:
mvn clean package
Run (development):

To run directly via Maven:
mvn spring-boot:run
Or run the packaged jar:
java -jar target/notification-service-0.0.1-SNAPSHOT.jar
Tests:

Run unit tests with:
mvn test
Project structure (high level):

src/main/java — application code
com.example.notification_service.NotificationServiceApplication — Spring Boot entry point
com.example.notification_service.config.RabbitMQConfig — queue configuration (QUEUE_NAME = notification.queue)
com.example.notification_service.runner.MessageRunner — sends startup message using RabbitTemplate
com.example.notification_service.consumer.NotificationConsumer — listens for messages with @RabbitListener
src/main/resources/application.properties — RabbitMQ and application settings
pom.xml — Maven build and dependencies (spring-boot-starter-amqp, spring-boot-starter-web, test deps)
How it works:

At application startup MessageRunner (CommandLineRunner) uses RabbitTemplate to send a message to notification.queue.
NotificationConsumer has a @RabbitListener bound to notification.queue and prints any incoming string messages.
RabbitMQConfig declares the durable queue used by both sender and listener.
Customization:

Change the queue name by editing the QUEUE_NAME constant in the RabbitMQConfig class.
Adjust RabbitMQ host/port/credentials in application.properties.
Replace the simple stdout logging with a real notification delivery implementation inside NotificationConsumer.consumeMessage.
Troubleshooting:

If no messages are delivered, confirm RabbitMQ is reachable at the configured host/port and credentials are correct.
Check application logs for startup errors or connection failures.
If using Docker for RabbitMQ, map port 5672 and use the same credentials or update application.properties.
Development tips:

Use an AMQP management UI (RabbitMQ Management Plugin) to inspect queues and messages.
For local testing, start RabbitMQ via Docker:
docker run -d --name rabbit -p 5672:5672 -p 15672:15672 rabbitmq:3-management
Contributing:

Fork the repository, create a feature branch, and submit a pull request.
Ensure mvn test passes before opening a PR.
