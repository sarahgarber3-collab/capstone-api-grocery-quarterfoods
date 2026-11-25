# Grocery Store E-Commerce API

A Spring Boot REST API for a grocery storefront, built as a Year Up capstone project. It supports product and category browsing, JWT-based authentication and authorization, persistent user shopping carts, and an OpenAPI/Swagger interface.

## Features

- Register and log in users with JWT authentication
- Support role-based access for `ROLE_USER` and `ROLE_ADMIN`
- Browse, filter, and search products by category, price range, and subcategory
- Create, update, and delete products as an administrator
- View and search categories
- Add products to an authenticated user's persistent shopping cart
- View or clear the current user's shopping cart
- Explore the API through Swagger UI

> Some profile, category-management, profile, cart-update, and checkout endpoints shown in `openapi.yaml` are still marked as TODOs. The implementation status should be checked in the controller classes before relying on an endpoint.

## Tech Stack

- Java 17
- Spring Boot 4.0.2
- Spring Web MVC
- Spring Security with JJWT
- Spring Data JPA
- MySQL
- H2 for test data
- SpringDoc OpenAPI / Swagger UI
- Maven

## Project Structure

```text
src/main/java/org/yearup/
├── ECommerceApplication.java       # Spring Boot entry point
├── controllers/                    # REST endpoints
├── service/                        # Application and cart business logic
├── repository/                     # Spring Data JPA repositories
├── models/                         # API and persistence models
└── security/                       # JWT and Spring Security configuration

src/main/resources/
├── application.properties          # Database, JWT, and Swagger settings
├── static/openapi.yaml             # API specification served by Swagger UI
└── banner*.txt                     # Store-specific startup banners

database/
└── create_database_grocerystore.sql  # Grocery database schema and seed data

src/test/java/                      # JUnit and Spring Data JPA tests
openapi.yaml                        # Standalone API specification
pom.xml                             # Maven build and dependencies
```

## API Overview

| Area | Base path | Access |
|---|---|---|
| Authentication | `/register`, `/login` | Public |
| Products | `/products` | Browse publicly; admin for mutations |
| Categories | `/categories` | Browse and administration endpoints |
| Shopping cart | `/cart` | Authenticated users |
| API documentation | `/swagger-ui.html` | Swagger UI |

Example product filters:

```text
GET /products
GET /products?cat=1
GET /products?minPrice=3&maxPrice=10
GET /products?subCategory=Organic
GET /products/{id}
```

For complete request and response details, use the Swagger UI or read [`openapi.yaml`](openapi.yaml).

## Prerequisites

- JDK 17 or newer
- Maven 3.9+ or use the included Maven wrapper
- MySQL 8+
- Optional: an API client such as Insomnia or Postman

## Database Setup

The grocery schema and seed data are in [`database/create_database_grocerystore.sql`](database/create_database_grocerystore.sql).

Run the script in MySQL Workbench or from the MySQL CLI:

```bash
mysql -u root -p < database/create_database_grocerystore.sql
```

The script creates the `grocerystore` database and inserts sample users, categories, products, and cart items.

For a sample seeded user, try:

- Username: `user`
- Username: `admin`

Sample password data and the `hashed_password` values are stored in the SQL script. Do not use this seeded configuration outside local development.

## Configuration

The application reads database credentials and the database name from environment variables, with local defaults in [`application.properties`](src/main/resources/application.properties).

```properties
DB_NAME=grocerystore
DB_USERNAME=root
DB_PASSWORD=<your-mysql-password>
```

Set them in your shell or IDE before starting the application:

```bash
export DB_NAME=grocerystore
export DB_USERNAME=root
export DB_PASSWORD='your-mysql-password'
```

On Windows PowerShell:

```powershell
$env:DB_NAME="grocerystore"
$env:DB_USERNAME="root"
$env:DB_PASSWORD="your-mysql-password"
```

The default server address is `http://localhost:8080`.

## Run the Application

macOS/Linux:

```bash
./mvnw spring-boot:run
```

Windows:

```powershell
mvnw.cmd spring-boot:run
```

Swagger UI will be available at:

```text
http://localhost:8080/swagger-ui.html
```

## Authentication

1. Send `POST /register` or `POST /login`.
2. Copy the JWT from the login response.
3. Send it in protected requests:

```http
Authorization: Bearer <jwt-token>
```

JWT expiration is configured by `jwt.token-timeout-seconds` in `application.properties`.

## Run Tests

```bash
./mvnw test
```

Or on Windows:

```powershell
mvnw.cmd test
```

Tests use the H2 test database and repository test seed data where configured.

## Implementation Notes

- `ShoppingCartService.addToCart()` inserts a cart row when a product is not already present. Otherwise, it increments the existing quantity.
- Cart records are keyed by the combination of user ID and product ID through a unique database constraint.
- `ShoppingCartController` resolves the authenticated principal to a database user before reading or modifying a cart.
- The application also contains SQL scripts and banner resources for other store themes, but the default and grocery-focused setup uses `grocerystore`.

## Development Status

This repository is a student capstone and is under active development. Confirm the implementation in the controllers and services before documenting or calling an endpoint marked `@todo` in `openapi.yaml`.
