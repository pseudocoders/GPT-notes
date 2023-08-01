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

## Docker LAMP

* Reference: [github](https://github.com/sprintcube/docker-compose-lamp)

```
git clone https://github.com/sprintcube/docker-compose-lamp.git
cd docker-compose-lamp/
cp sample.env .env
// modify sample.env as needed
docker-compose up -d
// visit localhost
docker-compose stop
docker-compose start
docker-compose down
```

Be carefull with docker-compose down command, it will remove all volumes attached to your containers. For example in a database container, when you execute that command, it will destroy all persistent data.

