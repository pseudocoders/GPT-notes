# Docker for development

Here's a step-by-step guide to installing Docker on Linux Mint via `apt`:

Step 1: Update your system
Open a terminal and update your package list to ensure you have the latest package information:

```bash
sudo apt update
```

Install dependencies
Docker requires a few packages to be installed. Make sure you have them on your system:

```bash
sudo apt-get install ca-certificates curl gnupg lsb-release
```

Install Docker

```bash
sudo apt install docker
sudo apt install docker-compose
```

Start and enable Docker
Once Docker is installed, you can start the Docker service and enable it to start on system boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Verify Docker installation
You can verify the Docker installation by running a simple test:

```bash
sudo docker --version
sudo docker run hello-world
```

If everything is set up correctly, you should see the Docker version information and a "Hello from Docker!" message.

## Docker ce vs Docker

"Docker CE" (Community Edition) and "Docker" (sometimes referred to as "Docker EE" or "Docker Enterprise Edition") are two different variants of the Docker platform, each with distinct features and target audiences.

1. **Docker CE (Community Edition):**

   Docker CE is the free and open-source version of Docker. It's aimed at individual developers, small teams, and hobbyists who want to use Docker for creating, deploying, and managing containerized applications. Docker CE provides essential features for containerization, such as container runtime, image management, networking, and orchestration tools like Docker Compose.

   Docker CE comes in two editions:
   - **Docker CE Desktop**: This is a desktop application that provides an easy-to-use graphical interface for managing containers and images. It's available for Windows and macOS operating systems.
   - **Docker CE Engine**: This is the command-line tool and runtime for creating and managing containers on Linux-based systems.

   Docker CE is suitable for development environments, personal projects, and small-scale deployments. It's regularly updated with new features and improvements.

3. **Docker (Docker EE / Docker Enterprise Edition):**
   Docker Enterprise Edition (formerly known as Docker Datacenter) is a commercial platform offered by Docker, Inc. It's designed for larger enterprises and organizations that require additional features, support, and security for their container environments. Docker EE includes all the features of Docker CE and extends them with enterprise-focused capabilities.

   Some key features of Docker EE include:
   - **Advanced Security**: Enhanced security features like image signing, vulnerability scanning, and role-based access control (RBAC).
   - **Image Management**: Tools for managing and distributing container images across an organization.
   - **Orchestration**: Advanced orchestration features for managing clusters of containers at scale, such as Docker Swarm or Kubernetes integration.
   - **Certified Infrastructure**: Integration with certified infrastructure partners for optimized performance.
   - **Enterprise Support**: Official support from Docker, Inc., including training, certification, and technical assistance.

   Docker EE is suitable for large-scale deployments in enterprise environments where security, scalability, and support are critical factors.

In summary, the main difference between Docker CE and Docker EE lies in their target audiences and feature sets. Docker CE is suitable for individual developers and small teams, offering essential containerization features. Docker EE is a commercial platform tailored for enterprises, offering advanced security, orchestration, and support options. It's important to choose the variant that best aligns with your project's requirements and scale. Keep in mind that the container ecosystem is rapidly evolving, so it's a good idea to check for the latest updates and offerings from Docker, Inc.

## Docker LAMP

* Reference: [github: sprintcube/docker-compose-lamp](https://github.com/sprintcube/docker-compose-lamp)

```
git clone https://github.com/sprintcube/docker-compose-lamp.git
cd docker-compose-lamp/
// save a copy of sample.env
cp sample.env .env
// modify sample.env as needed
docker-compose up -d
// visit localhost
docker-compose stop
docker-compose start
docker-compose down
```

Be carefull with docker-compose down command, it will remove all volumes attached to your containers. For example in a database container, when you execute that command, it will destroy all persistent data.

### sample.env

```
# Please Note:
# In PHP Versions <= 7.4 MySQL8 is not supported due to lacking pdo support

# To determine the name of your containers
COMPOSE_PROJECT_NAME=lamp

# Possible values: php54, php56, php71, php72, php73, php74, php8, php81
PHPVERSION=php8
DOCUMENT_ROOT=./www
APACHE_DOCUMENT_ROOT=/var/www/html
VHOSTS_DIR=./config/vhosts
APACHE_LOG_DIR=./logs/apache2
PHP_INI=./config/php/php.ini
SSL_DIR=./config/ssl

# PHPMyAdmin
UPLOAD_LIMIT=512M
MEMORY_LIMIT=512M

# Xdebug
XDEBUG_LOG_DIR=./logs/xdebug
XDEBUG_PORT=9003
#XDEBUG_PORT=9000

# Possible values: mysql57, mysql8, mariadb103, mariadb104, mariadb105, mariadb106
#
# For Apple Silicon User: 
# Please select Mariadb as Database. Oracle doesn't build their SQL Containers for the arm Architecure

DATABASE=mysql8
MYSQL_INITDB_DIR=./config/initdb
MYSQL_DATA_DIR=./data/mysql
MYSQL_LOG_DIR=./logs/mysql

# If you already have the port 80 in use, you can change it (for example if you have Apache)
HOST_MACHINE_UNSECURE_HOST_PORT=8080

# If you already have the port 443 in use, you can change it (for example if you have Apache)
HOST_MACHINE_SECURE_HOST_PORT=8443

# If you already have the port 3306 in use, you can change it (for example if you have MySQL)
HOST_MACHINE_MYSQL_PORT=3306

# If you already have the port 8080 in use, you can change it (for example if you have PMA)
HOST_MACHINE_PMA_PORT=8081
HOST_MACHINE_PMA_SECURE_PORT=8444

# If you already has the port 6379 in use, you can change it (for example if you have Redis)
HOST_MACHINE_REDIS_PORT=6379

# MySQL root user password
MYSQL_ROOT_PASSWORD=tiger

# Database settings: Username, password and database name
#
# If you need to give the docker user access to more databases than the "docker" db 
# you can grant the privileges with phpmyadmin to the user.
MYSQL_USER=docker
MYSQL_PASSWORD=docker
MYSQL_DATABASE=docker
```

### Links

* http://localhost/
* virtual hosts
  * http://app.localhost/
  * http://dash.localhost/
  * http://daw.localhost/
* http://dash.localhost/phpinfo.php
* phpMyAdmin
  * http://localhost:8080/
* test DB Connection
  * http://dash.localhost/test_db.php
  * http://dash.localhost/test_db_pdo.php

