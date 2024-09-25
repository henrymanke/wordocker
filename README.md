# WordPress with MySQL in Docker

## Table of Contents
- [Project Description](#project-description)
- [Quickstart](#quickstart)
- [Usage](#usage)
- [Healthchecks](#healthchecks)
- [Testing and Troubleshooting](#testing-and-troubleshooting)
- [Security Considerations](#security-considerations)

## Project Description
This repository contains a Docker Compose setup for running a WordPress instance alongside a MySQL (MariaDB) database. The goal is to simplify the deployment of a WordPress development environment using Docker. It includes pre-configured services for WordPress and MySQL, health checks, and persistent storage for both services.

## Quickstart

### Prerequisites
Ensure you have the following installed:
- Docker
- Docker Compose

### Steps:
1. Clone the repository:
   ```bash
   git clone https://github.com/henrymanke/wordocker.git
   cd wordocker
   ```

2. Set up environment variables by creating a `.env` file:
   ```bash
   cp example.env .env
   ```

3. Start the services using Docker Compose:
   ```bash
   docker-compose up -d
   ```

4. Open your browser and access WordPress:
   - Go to `http://localhost:8080` or the port configured in the `.env` file.

## Usage

### Customizing the Environment Variables:
To tailor the setup to your needs, modify the following environment variables in the `.env` file:

```plaintext
MYSQL_ROOT_PASSWORD=rootpassword       # The root password for MySQL.
MYSQL_DATABASE=wordpress               # The MySQL database name.
MYSQL_USER=wp_user                     # The MySQL user.
MYSQL_PASSWORD=wp_password             # The MySQL user password.
WORDPRESS_PORT=8080                    # The port where WordPress will be accessible.
WORDPRESS_TABLE_PREFIX=wp_             # The table prefix for WordPress.
```

After making any changes, restart the services:
```bash
docker-compose down
docker-compose up -d
```

### Persisting Data:
- **MySQL Data**: Stored in a volume `db_data`.
- **WordPress Data**: Stored in a volume `wordpress_data`.

This setup ensures your data persists even after restarting or stopping the containers.

## Healthchecks

- **MySQL Healthcheck**: Monitors if MySQL is up and running using the following command:
   ```bash
   mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} -e 'SELECT 1'
   ```
- **WordPress Healthcheck**: Validates that WordPress is responding correctly:
   ```bash
   curl -f http://localhost:${WORDPRESS_PORT} || exit 1
   ```

If any service fails the health check, Docker will restart the container.

## Testing and Troubleshooting

### Testing the Setup
Before submitting or deploying, ensure the following:
- WordPress is accessible at `http://localhost:${WORDPRESS_PORT}`.
- You can log in with your admin credentials and navigate through WordPress.
- After restarting the setup, all your data remains intact.

### Troubleshooting:
- To inspect logs:
   ```bash
   docker-compose logs
   ```
- If a container crashes, it will automatically restart due to the `restart: always` policy.

## Security Considerations

- **Environment Variables**: Avoid storing sensitive information such as SSH keys, passwords, or IP addresses directly in the repository. Use environment variables via `.env` files.
- **Naming Conventions**: For environment variables, use the `UPPER_CASE_WITH_UNDERSCORE` format. Reference them using `${VARIABLE_NAME}` to avoid errors in shell scripts.

## Additional Notes
- Ensure that all default values are sensible and secure.
- Regularly review Docker and WordPress security practices to keep your setup safe from vulnerabilities.