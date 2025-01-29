## What is Saleor?

Saleor is a headless e-commerce platform built with Python (Django) and GraphQL. It is designed for flexibility, scalability, and customization, making it suitable for B2B, B2C, and marketplace applications.

## Key Features

- **Headless Architecture** -: The frontend is decoupled from the backend, allowing integration with various frontends like React, Vue.js, or mobile apps.
- **GraphQL API** -: Uses GraphQL for efficient data fetching and flexible queries.
- **Modular and Extensible** -: Supports plugins and customizations for payment gateways, shipping, and pricing logic.
- **Multi-Channel Sales** -: Manage multiple stores, currencies, and languages from a single dashboard.
- **B2B and B2C Suppor** -: Features for both business customers (e.g., custom pricing, bulk orders) and direct consumers.
- **Docker Support** -: Easy deployment using Docker containers.

## Technology Stack 

- **Backend**: Python (Django), GraphQL
- **Frontend**: React (Saleor Storefront), TypeScript
- **Database**: PostgreSQL
- **Deployment**: Docker, Kubernetes

## How to set up Saleor using Docker?

- **Step 1** -: Clone `saleor-platform` repository using command -:
  
  ```
  git clone https://github.com/saleor/saleor-platform.git
  ```
- **Step 2** -: Go to cloned repository using `cd saleor-platform`.
- **Step 3** -: Apply Django migrations:
  ```
  docker compose run --rm api python3 manage.py migrate
  ```
- **Step 4** -: Populate the database with example data and create the admin user:
  ```
  docker compose run --rm api python3 manage.py populatedb --createsuperuser
  ```
- **Step 5** -: Run the application:
  ```
  docker compose up
  ```
- **Step 6** -: Clone *Saleor Core* using command -:
  ```
  git clone https://github.com/saleor/saleor.git
  ```
- **Step 7** -: Go to cloned repository using `cd saleor`
- **Step 8** -: Create a docker container using -: `docker build saleor:latest`
- **Step 9** -: Run Docker using command -:
  ```
  docker run --env-file {path of backend.env} --env-file {path of common.env} -p 8000:8000  --network=saleor-platform_saleor-backend-tier  saleor:latest
  ```
  Your **Saleor-Platform** is running on `http://127.0.0.1:8000/`.

  ## Problem Statement

  Modify Saleor to enable personalized pricing for B2B customers. The system should allow sellers to assign custom prices to specific customers at the product level. When a logged-in B2B customer browses the
  store, they should automatically see their exclusive prices instead of the default product prices.

  ## Approach

  1. **User Role Identification**
     - Modify the user model to include a `user_type` field (B2C or B2B).
     - Set this field during **signup** based on a key provided by the user.
     - Store this information in the database.

2. **Custom Pricing Storage**
   - Create a new model (`CustomerPrice`) to store product-specific prices for B2B users.
   - Structure: `customer_id`, `product_id`, `custom_price`.

3. **Pricing Logic Modification**
   - Modify GraphQL resolvers to check the logged-in user’s `user_type`.
   - If `B2B`, fetch custom price from `CustomerPrice`.
   - If no custom price exists, fallback to **default price**.

4. **Testing**
   - Test user signups, role-based pricing, and default price fallback.
  
**Note** -: Use Saleor's GraphQL Playground to test the results.
