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

## Docker daemon & Docker client

Docker uses a client-server architecture, where the client interacts with a server component called the Docker daemon. This architecture allows users to interact with Docker through a command-line interface (CLI) or other tools while isolating the complex operations performed by the daemon.

1. **Docker Daemon:**
   The Docker daemon (also known as `dockerd`) is a background process running on the host system. It's responsible for managing Docker containers, images, networks, volumes, and other resources. The daemon handles the low-level operations of building, running, and managing containers. It also communicates with the underlying Linux kernel's features like cgroups and namespaces to ensure proper container isolation and resource utilization.

   The Docker daemon listens for Docker API requests and responds to client requests, performing the necessary actions to manage containers and resources. It manages the lifecycle of containers, from starting and stopping them to managing their resource allocation.

2. **Docker Client:**

   The Docker client is the command-line tool (usually named `docker`) that allows users to interact with the Docker daemon. It provides a user-friendly interface for issuing commands to build, run, manage, and inspect containers and images. The Docker client translates user commands into API requests that are sent to the Docker daemon over the Docker API.

   Users interact with the Docker client to perform actions such as creating containers, pulling images, starting and stopping containers, viewing logs, managing networks, and more. The Docker client abstracts away the complexities of interacting directly with the Docker daemon's API, making it easy for users to manage containers without needing to understand the underlying technical details.

In summary, the Docker client and Docker daemon work together to enable the creation, management, and operation of containers. The client serves as the user interface, while the daemon handles the behind-the-scenes tasks necessary for running and managing containers efficiently and securely. This separation of concerns allows developers and operators to interact with containers and images in a standardized way, regardless of the underlying system complexities.

## Docker image

A Docker image is a lightweight, standalone, and executable software package that contains everything needed to run a piece of software, including the code, runtime, system libraries, environment variables, and configuration files. It's a snapshot of a filesystem along with instructions for how to use and run the software contained within it.

Docker images serve as the basis for creating Docker containers, which are instances of running applications. When you launch a container from an image, you create an isolated environment where the application runs with its own filesystem, networking, and other resources, while sharing the host system's kernel.

Key characteristics of Docker images:

1. **Layered Architecture**: Images are created using a layered file system. Each instruction in a Dockerfile (a special text file that defines how an image is built) adds a new layer on top of the previous one. This layered approach allows for efficient storage and sharing of common components across multiple images. Layers are read-only, which contributes to the immutability and efficiency of images.

2. **Immutable**: Once an image is created, its contents cannot be modified. If changes are required, a new image is created based on the updated Dockerfile. This immutability ensures consistency and reliability in different environments.

3. **Versioned**: Images can have versions or tags associated with them. This allows users to specify which version of an image they want to use when creating a container. Versioning is crucial for managing software releases and updates.

4. **Portable**: Docker images encapsulate the application and its dependencies, making them highly portable across different systems and environments. This eliminates the "it works on my machine" problem by ensuring consistency in various deployment scenarios.

5. **Reproducible**: By using Dockerfiles to define how an image is built, you can ensure that the image creation process is reproducible. This is particularly useful for collaboration and continuous integration/continuous deployment (CI/CD) pipelines.

Docker images can be shared and distributed through Docker registries, both public (like Docker Hub) and private. Docker Hub is a centralized repository where developers can publish and pull images, while private registries allow organizations to manage their own image distribution within their infrastructure.

In summary, a Docker image is a self-contained package that encapsulates an application and its dependencies, providing a consistent and efficient way to deploy and manage software across various environments.

## Docker registries

A Docker registry is a centralized repository that stores and distributes Docker images. It serves as a place where developers and organizations can publish, share, and manage Docker images. Docker registries play a crucial role in the containerization process by providing a way to store and retrieve images, making it easy to distribute applications and services in a consistent and efficient manner.

Key features of Docker registries:

1. **Image Storage**: Docker registries store Docker images as a collection of layers, where each layer represents a specific filesystem change. This layered architecture enables efficient storage and sharing of images, as common layers can be reused across multiple images.

2. **Public and Private Registries**: There are both public and private Docker registries available. Public registries, such as Docker Hub, allow anyone to publish and access images. Private registries are used by organizations to control access to their images and manage distribution within their infrastructure. Private registries are particularly useful for securing proprietary or sensitive software.

3. **Image Sharing**: Registries make it easy to share Docker images with colleagues, collaborators, or the broader community. Developers can upload their images to a registry, and others can then download and use those images to create containers.

4. **Caching**: Docker registries also act as caching mechanisms. When an image is pulled from a registry to a local system, Docker caches the layers locally. This caching reduces the time and bandwidth needed to download the same image on multiple systems.

5. **Image Versioning**: Docker images in a registry can have different versions or tags associated with them. This allows users to specify which version of an image they want to use when creating a container. Versioning is essential for managing software releases and updates.

6. **Authentication and Security**: Private registries offer authentication and access control mechanisms. This ensures that only authorized users can push and pull images from the registry, enhancing security.

7. **Replication**: Some Docker registries support replication, allowing images to be mirrored across multiple instances. This is useful for distributing images across different geographical locations or ensuring high availability.

Commonly used Docker registries include:

- **Docker Hub**: A popular public registry hosted by Docker, Inc. It provides a vast collection of public images that anyone can use. Docker Hub also supports private repositories.

- **Amazon Elastic Container Registry (ECR)**: A fully managed Docker registry provided by Amazon Web Services (AWS) that is tightly integrated with their cloud services.

- **Google Container Registry**: A managed Docker registry provided by Google Cloud Platform (GCP) that allows users to store, manage, and access Docker images.

- **Azure Container Registry**: A private Docker registry provided by Microsoft Azure that enables users to store and manage Docker images in Azure.

In summary, a Docker registry is a critical component of the Docker ecosystem, facilitating the storage, distribution, and sharing of Docker images across different environments and systems.

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

