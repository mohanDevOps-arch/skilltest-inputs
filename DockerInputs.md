# Docker Tasks

## Task 1: Build and Run a Dockerized Flask Application

### Inputs Required:

1. **Flask Application Code:**

   - Create a Python file `app.py` with the following code:
     ```python
     from flask import Flask
     app = Flask(__name__)

     @app.route('/')
     def home():
         return "Welcome to Dockerized Flask Application!"

     if __name__ == "__main__":
         app.run(host="0.0.0.0", port=5000)
     ```

2. **Dockerfile:**

   - Write a `Dockerfile` to containerize the Flask application:
     ```Dockerfile
     # Use the official Python image as a base
     FROM python:3.9-slim

     # Set the working directory in the container
     WORKDIR /app

     # Copy the current directory contents into the container
     COPY . /app

     # Install Flask
     RUN pip install flask

     # Expose the application on port 5000
     EXPOSE 5000

     # Command to run the application
     CMD ["python", "app.py"]
     ```

3. **Commands:**

   - Build the Docker image using the command:
     ```bash
     docker build -t flask-app .
     ```
   - Run the container exposing port 5000:
     ```bash
     docker run -p 5000:5000 flask-app
     ```

4. **Testing Instructions:**

   - Open your web browser and verify the application at `http://localhost:5000`.

---

## Task 2: Multi-Container Application with Docker Compose

### Inputs Required:

1. **Flask Application Code:**

   - Create a Python file `app.py` that increments a counter using Redis:
     ```python
     from flask import Flask
     from redis import Redis

     app = Flask(__name__)
     redis = Redis(host='redis', port=6379)

     @app.route('/')
     def home():
         count = redis.incr('hits')
         return f"Welcome! This page has been visited {count} times."

     if __name__ == "__main__":
         app.run(host="0.0.0.0", port=5000)
     ```

2. **Dockerfile:**

   - Create a Dockerfile to containerize the Flask application:
     ```Dockerfile
     FROM python:3.9-slim
     WORKDIR /app
     COPY . /app
     RUN pip install flask redis
     EXPOSE 5000
     CMD ["python", "app.py"]
     ```

3. **Docker Compose Configuration:**

   - Create a `docker-compose.yml` file:
     ```yaml
     version: '3'
     services:
       web:
         build: .
         ports:
           - "5000:5000"
         depends_on:
           - redis
       redis:
         image: redis
     ```

4. **Commands:**

   - Start the containers using:
     ```bash
     docker-compose up
     ```

5. **Testing Instructions:**

   - Access the application at `http://localhost:5000` and verify the counter increments.

---

## Task 3: Persistent Data Storage with Docker Volumes

### Inputs Required:

1. **MySQL Container:**

   - Run a MySQL container with environment variables for database creation:
     ```bash
     docker run -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=test_db -d mysql:latest
     ```

2. **Volume Creation:**

   - Create a named Docker volume:
     ```bash
     docker volume create mysql-data
     ```

3. **Commands:**

   - Attach the volume to the MySQL container:
     ```bash
     docker run -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=test_db -v mysql-data:/var/lib/mysql -d mysql:latest
     ```

4. **Testing Instructions:**

   - Access the MySQL database, add some data, stop the container, and verify data persistence after restarting.

---

## Task 4: Custom Bridge Network Creation

### Inputs Required:

1. **Custom Network:**

   - Create a custom bridge network:
     ```bash
     docker network create --driver bridge custom-network
     ```

2. **Containers:**

   - Run a Flask app and Redis container on the custom network:
     ```bash
     docker run --network custom-network -d --name flask-app flask-app
     docker run --network custom-network -d --name redis redis
     ```

3. **Flask App Configuration:**

   - Configure the Flask app to interact with Redis using the Redis container name (`redis`).

4. **Testing Instructions:**

   - Verify communication between containers by checking Redis functionality.

---

## Task 5: Network Isolation Implementation

### Inputs Required:

1. **Networks:**

   - Create two separate bridge networks:
     ```bash
     docker network create --driver bridge network-1
     docker network create --driver bridge network-2
     ```

2. **Containers:**

   - Run containers on separate networks:
     ```bash
     docker run --network network-1 -d --name container-1 nginx
     docker run --network network-2 -d --name container-2 nginx
     ```

3. **Testing Instructions:**

   - Attempt to ping one container from another and confirm isolation.

---

## Task 6: Multi-Stage Builds for Node.js

### Inputs Required:

1. **Node.js Application Code:**

   - Create a file `server.js`:
     ```javascript
     const express = require('express');
     const app = express();

     app.get('/', (req, res) => {
         res.send('Hello, World!');
     });

     app.listen(3000, () => {
         console.log('App running on http://localhost:3000');
     });
     ```

2. **Dockerfile:**

   - Write a multi-stage Dockerfile:
     ```Dockerfile
     # Stage 1: Build
     FROM node:16 AS build
     WORKDIR /app
     COPY . .
     RUN npm install

     # Stage 2: Production
     FROM node:16-slim
     WORKDIR /app
     COPY --from=build /app .
     CMD ["node", "server.js"]
     ```

3. **Commands:**

   - Build and run the container:
     ```bash
     docker build -t node-app .
     docker run -p 3000:3000 node-app
     ```

4. **Testing Instructions:**

   - Verify the application is running at `http://localhost:3000`.

---

## Task 7: Multi-Stage Builds for Python

### Inputs Required:

1. **Flask Application Code:**

   - Write a `app.py`:
     ```python
     from flask import Flask
     app = Flask(__name__)

     @app.route('/')
     def home():
         return "Hello, Docker"

     if __name__ == "__main__":
         app.run(host="0.0.0.0", port=5000)
     ```

2. **Dockerfile:**

   - Write a multi-stage Dockerfile:
     ```Dockerfile
     # Stage 1: Build
     FROM python:3.9 AS build
     WORKDIR /app
     COPY . .
     RUN pip install flask

     # Stage 2: Production
     FROM python:3.9-slim
     WORKDIR /app
     COPY --from=build /app .
     CMD ["python", "app.py"]
     ```

3. **Commands:**

   - Build and run the container:
     ```bash
     docker build -t flask-app .
     docker run -p 5000:5000 flask-app
     ```

4. **Testing Instructions:**

   - Verify the application is running at `http://localhost:5000`.

---

## Task 8: PostgreSQL Database Volume Management

### Inputs Required:

1. **PostgreSQL Container:**

   - Run a PostgreSQL container:
     ```bash
     docker run -e POSTGRES_PASSWORD=root -e POSTGRES_DB=test_db -d postgres:latest
     ```

2. **Volume Creation:**

   - Create a named Docker volume:
     ```bash
     docker volume create postgres-data
     ```

3. **Commands:**

   - Attach the volume to the container:
     ```bash
     docker run -e POSTGRES_PASSWORD=root -e POSTGRES_DB=test_db -v postgres-data:/var/lib/postgresql/data -d postgres:latest
     ```

4. **Testing Instructions:**

   - Access the PostgreSQL database, add data, stop the container, and verify persistence after restarting.

---

## Task 9: Shared Volume Configuration

### Inputs Required:

1. **Volume Creation:**

   - Create a shared volume:
     ```bash
     docker volume create shared-volume
     ```

2. **Containers:**

   - Run containers with the shared volume:
     ```bash
     docker run -v shared-volume:/data -d --name flask-app flask-app
     docker run -v shared-volume:/data -d --name redis redis
     ```

3. **Testing Instructions:**

   - Add data to `/data` in one container and verify access from the other.

---

### Note for Students:

- Refer to Docker documentation for further learning.
- Explore advanced configurations to deepen your understanding.

