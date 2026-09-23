# URL Shortener

A Spring Boot-based URL shortener service that converts long URLs into short codes, redirects users to the original URL, and exposes statistics for each short link.

## Features

- Create short URLs from any valid long URL
- Redirect short links to their original destination
- Track click counts per short code
- View short URL statistics
- Swagger/OpenAPI documentation
- PostgreSQL persistence

## Tech Stack

- Java 25
- Spring Boot 4.1.1
- Spring Web MVC
- Spring Data JPA
- PostgreSQL
- Spring Validation
- Springdoc OpenAPI

## Project Structure

```text
src/
├── main/
│   ├── java/com/example/url_shortner/
│   │   ├── controller/
│   │   │   ├── RedirectController.java
│   │   │   └── UrlShortenerController.java
│   │   ├── dto/
│   │   │   ├── ShortenRequest.java
│   │   │   └── ShortenResponse.java
│   │   ├── exception/
│   │   │   ├── GlobalExceptionHandler.java
│   │   │   └── UrlNotFoundException.java
│   │   ├── model/
│   │   │   └── UrlMapping.java
│   │   ├── repository/
│   │   │   └── UrlMappingRepository.java
│   │   ├── service/
│   │   │   └── UrlShortenerService.java
│   │   └── UrlShortnerApplication.java
│   └── resources/
│       └── application.properties
└── test/java/com/example/url_shortner/
    └── UrlShortnerApplicationTests.java
```

## Prerequisites

Before running the project, make sure you have:

- Java 25 or later
- Maven 3.9+
- PostgreSQL installed and running
- A database named `urlshortner` created locally

## Configuration

Update the database settings in `src/main/resources/application.properties`:

```properties
spring.application.name=Url_Shortner
spring.datasource.url=jdbc:postgresql://localhost:5432/urlshortner
spring.datasource.username=postgres
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

springdoc.swagger-ui.path=/swagger-ui.html
```

You can also set a custom base URL for generated links:

```properties
app.base-url=http://localhost:8080
```

## Running the Application

From the project root, run:

```bash
./mvnw spring-boot:run
```

On Linux/macOS, if needed:

```bash
chmod +x mvnw
./mvnw spring-boot:run
```

The app will start on:

```text
http://localhost:8080
```

## API Endpoints

### Create a short URL

Request:

```http
POST /api/shorten
Content-Type: application/json
```

Body:

```json
{
  "originalUrl": "https://example.com/very/long/url"
}
```

Response:

```json
{
  "shortCode": "AbC1234",
  "shortUrl": "http://localhost:8080/AbC1234",
  "originalUrl": "https://example.com/very/long/url"
}
```

### Redirect to original URL

```http
GET /{shortCode}
```

Example:

```http
GET /AbC1234
```

This responds with an HTTP 302 redirect to the original URL.

### Get URL statistics

```http
GET /api/stats/{shortCode}
```

Example response:

```json
{
  "id": 1,
  "shortCode": "AbC1234",
  "originalUrl": "https://example.com/very/long/url",
  "createdAt": "2026-09-23T10:00:00",
  "clickCount": 25
}
```

## Swagger UI

Once the application is running, you can access the API documentation here:

```text
http://localhost:8080/swagger-ui.html
```

## License

This project is currently provided as a learning/demo application without a formal license.
