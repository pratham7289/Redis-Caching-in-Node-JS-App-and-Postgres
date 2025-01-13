# PostgreSQL, Redis, and Node.js Integration

This project demonstrates the integration of PostgreSQL, Redis, and Node.js to create a scalable and efficient web application. The application uses PostgreSQL as the primary database, Redis as a caching layer, and Node.js as the backend server.

## Table of Contents
1. [PostgreSQL Database Setup](#postgresql-database-setup)
2. [Redis Setup](#redis-setup)
3. [Node.js Application Setup](#nodejs-application-setup)
4. [Testing the Application](#testing-the-application)
5. [Redis Integration and Cache Workflow](#redis-integration-and-cache-workflow)
6. [Summary of Workflow](#summary-of-workflow)

## PostgreSQL Database Setup

### Step 1: PostgreSQL Database and Table Creation
- Create a PostgreSQL database named `user_db`.
- Create a table named `users` with the following columns:
  - `id`: A unique identifier for each user.
  - `name`: The name of the user.
  - `email`: The user's email address.
  - `age`: The user's age.

### Step 2: PostgreSQL Configuration for Remote Connections
- Update the `postgresql.conf` file to listen on all network interfaces (`0.0.0.0`).
- Modify the `pg_hba.conf` file to allow connections from the Node.js server's IP (e.g., `192.168.48.132/32`) using the `md5` authentication method.

### Step 3: Inserting Sample User Data
- Insert sample users into the `users` table to populate the database.

## Redis Setup

### Step 4: Redis Installation and Configuration
- Install Redis on Server 2.
- Configure Redis to allow connections from the Node.js application:
  - Set the `bind` configuration to the appropriate network interfaces.
  - Set the `requirepass` configuration to secure Redis with a password.
- Restart the Redis service to apply the configuration changes.

## Node.js Application Setup

### Step 5: Node.js and Library Installation
- Install Node.js on Server 2.
- Install the required libraries:
  - `express`: A web framework for handling HTTP requests.
  - `pg`: A PostgreSQL client library.
  - `redis`: A Redis client library.

### Step 6: Creating the `app.js` File
- Create the `app.js` file with the following routes:
  - `GET /users`: Retrieves all users from PostgreSQL and returns them as a JSON response.
  - `POST /users`: Adds new users to the PostgreSQL database via a POST request.
  - `GET /users/:id`: Checks Redis for cached user data:
    - If the data exists in Redis (cache hit), it is returned immediately.
    - If the data is not found in Redis (cache miss), the route queries PostgreSQL, retrieves the data, caches it in Redis, and returns it.

### Step 7: Starting the Node.js Server
- Start the Node.js server using the command `node app.js`.
- The server listens on port `5000` (or `0.0.0.0:5000` to accept external connections).

## Testing the Application

### Step 8: Testing the `/users` Endpoint
- Test the `/users` endpoint using a web browser or `curl`.
- Verify that the endpoint returns a list of users stored in PostgreSQL.

## Redis Integration and Cache Workflow

### 1. Redis Integration in the Application
- Configure the Node.js application to connect to the Redis server using the correct host and port.

### 2. Cache Lookup (Cache Hit / Cache Miss)
- When a request for user data is received, the application first checks Redis for the cached data:
  - **Cache Hit**: If the data is found in Redis, it is returned immediately.
  - **Cache Miss**: If the data is not found in Redis, the application queries PostgreSQL, retrieves the data, caches it in Redis, and returns it.

### 3. Monitoring Cache Behavior
- Log or return the source of the data (`cache` or `database`) in the response.
- Track and display the time it took to retrieve the data.

### 4. System Performance
- **Reduced Load on PostgreSQL**: Caching frequently accessed data reduces the need to query PostgreSQL.
- **Faster Data Retrieval**: Cached data is served instantly, improving response times.

## Summary of Workflow
The Redis integration into the Node.js application optimizes data retrieval by caching frequently accessed user data. Cache hits provide fast responses, while cache misses ensure that data is retrieved from PostgreSQL and then cached for future use. Monitoring and logging both the source of data and response times allow for performance evaluation and optimization, while the caching layer reduces database load and improves the overall system performance.
