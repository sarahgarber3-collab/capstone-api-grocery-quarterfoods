# [Grocery Store] – E-Commerce API

A Spring Boot REST API backend for an online grocery store, built as a capstone
project. The API handles product categories, product browsing/search, and a
persistent shopping cart for logged-in users, backed by a MySQL database.

## Features

- **Categories** – Browse, search, and (as an admin) create/update/delete
  product categories
- **Products** – Browse and search products by category, price range, and
  sub-category
- **Shopping Cart** – Logged-in users can add products to their cart, view
  their cart, and clear it. Cart contents persist across logins.
- **Authentication** – JWT-based registration and login, with role-based
  access control (ADMIN vs. USER)

## Tech Stack

- Java / Spring Boot
- MySQL
- Spring Security (JWT authentication, role-based authorization)
- Insomnia (API testing)


## Interesting Code: Add to Cart Logic

One piece of logic I'm proud of is in `ShoppingCartService.addToCart()`. Rather
than always inserting a new row, it checks whether the product is already in
the user's cart and either inserts a new row (quantity 1) or increments the
existing quantity:


## Setup

1. Run the appropriate `.sql` script in MySQL Workbench to create the database
2. Update `application.properties` with your database name/credentials
3. Run the Spring Boot application
4. Use Insomnia (or the included frontend) to interact with the API

