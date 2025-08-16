<p align="center">
  <img src="./assets/avatar.jpeg" alt="Project Mongodb Docker Avatar" width="200">
</p>

# Ready-to-use MongoDB with Docker Compose

This project provides a robust, configurable, and Ready-to-use infrastructure for running a MongoDB database using Docker and Docker Compose. It is designed for security, persistence, and ease of use.

## Features

-   **Dockerized:** Runs MongoDB in an isolated container for consistency across all environments.
-   **Fully Configurable:** Uses a `.env` file for easy configuration of ports, users, and paths.
-   **Persistent Storage:** Maps host directories for data and logs, ensuring your data survives container restarts.
-   **Security by Default:** Enforces authentication and creates a dedicated, non-root user for your application.
-   **Automated Initialization:** Uses a startup script (`init-mongo.js`) to automatically create your application's database and user on the first run.
-   **Centralized Configuration:** Manages MongoDB server settings through a dedicated `mongod.conf` file.
-   **Health Checks:** Includes a Docker health check to ensure the container is running and the database is responsive.

## Prerequisites

Before you begin, ensure you have the following installed on your system:
-   [Docker](https://docs.docker.com/get-docker/)
-   [Docker Compose](https://docs.docker.com/compose/install/)

## Project Structure

```
.
├── .env.example                  # Template for environment variables
├── .gitignore                    # Specifies intentionally untracked files
├── docker-compose.yml.example    # Template for Docker Compose configuration
├── init-scripts/
│   ├── .gitignore                # Ignores custom init scripts
│   └── init-mongo.js.example     # Template for DB initialization script
├── logs/
│   └── .gitignore                # Keeps the logs directory but ignores log files
├── mongod.conf.example           # Template for MongoDB configuration file
└── README.md                     # This file
```

## Quickstart Guide

Follow these steps to get your MongoDB instance up and running.

### Step 1: Create Local Configuration Files

This project uses `.example` files as templates. You need to create your own local versions.

```bash
# Create your environment file
cp .env.example .env

# Create your Docker Compose file
cp docker-compose.yml.example docker-compose.yml

# Create your MongoDB config file
cp mongod.conf.example mongod.conf

# Create your database initialization script
cp init-scripts/init-mongo.js.example init-scripts/init-mongo.js
```

### Step 2: Customize Your Configuration

1.  **`.env`**: Open the `.env` file and set strong, unique passwords for `MONGO_INITDB_ROOT_PASSWORD`. You can also customize ports, container names, and paths if needed.

2.  **`init-scripts/init-mongo.js`**: Open this file and change the `APP_DATABASE`, `APP_USER`, and `APP_PASSWORD` constants to match your application's requirements. This script will create a dedicated user for your app.

### Step 3: Set Directory Permissions (Crucial Step)

The MongoDB container runs its process under a specific user (`mongodb`, UID `999`), not as `root`. To allow this user to write to the data and log directories on your host machine, you **must** change the ownership of these directories.

**Run these commands from the project's root directory:**

```bash
# Ensure the data and logs directories exist
mkdir -p ./data ./logs

# Change the ownership of the directories to the mongodb user's ID (999)
# You need sudo because you are changing ownership to a different user.
sudo chown -R 999:999 ./data ./logs
```
> **Why is this necessary?** This command gives the MongoDB process inside the container permission to write database files and logs to the `./data` and `./logs` folders on your computer. Without this, you will get a "Permission denied" or "FileNotOpen" error on startup.

### Step 4: Launch MongoDB

Now you are ready to start the container.

```bash
# Start the container in detached mode (-d)
docker compose up -d
```

### Step 5: Verify the Setup

1.  **Check the container status:**
    ```bash
    docker compose ps
    ```
    You should see the `mongo` container running and the status should be `healthy`.

2.  **Check the logs:**
    ```bash
    docker compose logs -f
    ```
    Look for a "waiting for connections" message. You should also see output from the `init-mongo.js` script if this is the first time you are running the container.

## Connecting to the Database

You can connect to your database using any MongoDB client or from your application.

-   **Host:** `localhost`
-   **Port:** `27017` (or whatever you set for `MONGO_EXTERNAL_PORT`)

#### Connection String for Application User

Use this string to connect your application. This user only has access to the `my_app_db` database.

```
mongodb://app_user:app_user_secret_password@localhost:27017/my_app_db?authSource=admin
```
*(Remember to replace the user, password, and database with the values from your `init-mongo.js` file.)*

#### Connection String for Root User

Use this for administrative tasks with a tool like MongoDB Compass or the `mongosh` CLI.

```
mongodb://root:your-very-strong-and-secret-password@localhost:27017/admin
```

## Managing the Container

Here are some common commands for managing your MongoDB service.

-   **Start the service in the background:**
    ```bash
    docker compose up -d
    ```

-   **Stop the service:**
    ```bash
    docker compose down
    ```

-   **Stop and remove data volumes (for a clean reset):**
    ```bash
    docker compose down -v
    ```

-   **View real-time logs:**
    ```bash
    docker compose logs -f
    ```

-   **Access the container's shell:**
    ```bash
    docker compose exec mongo bash
    ```

## Security Considerations

-   **Passwords:** Always use strong, unique passwords in your `.env` file, especially for the root user.
-   **Network Exposure:** By default, the database port is exposed on `localhost`. Do not expose your database directly to the public internet without configuring a firewall and other security measures.
-   **Principle of Least Privilege:** The `init-mongo.js` script creates an application user with limited permissions (`readWrite` on a single database). This is a critical security practice. Avoid using the root user for your application's day-to-day operations.