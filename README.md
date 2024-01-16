![MBA USB/ESALQ](https://i.imgur.com/MgViuM3.jpg)

## Understanding Docker's Command Line Interface (Docker CLI)

### Root folder for all codes: docker-class

Type in the terminal:

```bash

# Check if Docker is properly installed
docker

# Check the current Docker version
docker -v

# Collect basic information about Docker
docker info

# Pull the first image from the registry (Docker Hub)
docker pull hello-world

# List images on the machine
docker images

# Run the first Docker container from the pulled image
docker run hello-world

# List all containers and their basic information
docker ps -a

# List only active containers and their basic information
docker ps

```

## Creating an Image via Dockerfile and Running a Container Based on that Image

Type in the terminal:

```bash
# Enter the folder 1-create-image-script
cd 1-create-image-script

# Generate an image from the Dockerfile with the name image-script
docker build . -t image-script

# Run a container from the image with the name image-script
docker run image-script

# Delete the container that was just created using its ID
# Check the container ID
docker ps -a

# Remove the container using its ID since it doesn't have a name
docker rm `CONTAINER_ID`

# Check the container status after removing the container with the image image-script
docker ps -a

# Run the image-script container and remove it after executing all lines in Dockerfile
docker run --rm image-script

# Check the container status after removing the container with the image image-script
docker ps -a
```

Edit the file

```python
# Change line 2 of the script.py file
print("This script has been modified")
```

Type in the terminal

```bash
# Run the script from the image-script with the --rm argument
docker run --rm image-script

# Generate an imagem from the Dockerfile with the name image-script-altered
docker build . -t image-script-altered

# Run the script from the image-script-altered with the --rm argument
docker run --rm image-script-altered
```

## Creating a Container that Doesn't Stop After Running all Lines in Dockerfile

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 2-create-nonstop-image
cd 2-create-nonstop-image

# Generate the nonstop image with the name image-nonstop with a version tag
docker build . -t image-nonstop:1.0

# Run the container image-nonstop with the name image-nonstop
docker run -d --name image-nonstop image-nonstop:1.0

# Check running containers on the machine
docker ps -a

# Access the docker container terminal via command line
docker exec -it image-nonstop bash

# Access the Python kernel inside the container
python

# Test Python inside the container
>>> print("Python inside the container")

# Exit the Python kernel
ctrl+z

# Check folders and files in the container using the container terminal
ls

# Create a file inside the container
touch test

# Stop the container
docker stop image-nonstop

# Remove the container
docker rm image-nonstop

# The container can also be removed directly using the -f flag
# docker rm -f image-nonstop

# Run the container again to check if the created file is still there
docker run -d --name image-nonstop image-nonstop:1.0

# Access the docker container terminal via command line
docker exec -it image-nonstop bash

# Check folders and files in the container using the container terminal
ls

```

## Creating Shared Volumes host:container

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 3-containers-and-volumes
cd 3-containers-and-volumes

# Generate the containers-and-volumes image with the name containers-and-volumes version 1.0
docker build . -t containers-and-volumes:1.0

# Run the container containers-and-volumes with the name containers-and-volumes and the host:container volume
docker run -d --name containers-and-volumes -v ./volume:/app containers-and-volumes:1.0

# Check running containers on the machine
docker ps -a

# Access the docker container terminal via command line
docker exec -it containers-and-volumes bash

# Check files inside the container
ls

# Create files test1.txt and test2.txt outside the container, in the volume folder

# Create the test3.txt file inside the container
touch test4.txt

# Exit the container
exit

# Check and inspect the container using the inspect command
docker inspect containers-and-volumes

```

## Basic Operations such as stopping, removing, starting, and restarting containers via Docker CLI

```bash

# Stop the container with the name image-nonstop
docker stop image-nonstop

# Stop the container with the name containers-and-volumes
docker stop containers-and-volumes

# Check container status
docker ps -a

# Start the container with the name image-nonstop again
docker start image-nonstop

# Start the container with the name containers-and-volumes again
docker start containers-and-volumes

# Check container status
docker ps -a

# Stop all running containers
docker stop $(docker ps -a -q)

# Check container status
docker ps -a

# Remove all stopped containers
docker rm $(docker ps -a -q)

# Check container status
docker ps -a

```

## Basic Operations such as stopping, removing, starting, and restarting containers via VS Code Docker Extension

```bash
# Start two containers from the same image
docker run -d image-nonstop:1.0
docker run -d image-nonstop:1.0

# Stop container image-nonstop
# Stop container containers-and-volumes
# Start container image-nonstop
# Start container containers-and-volumes
# Access the terminal of container image-nonstop
# Exit the terminal of container image-nonstop
# Access the terminal of container containers-and-volumes

```

## Port Mapping between Host and Container

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 4-container-api
cd 4-container-api

# Generate the container-api image with the name container-api with a version tag 1.0
docker build . -t container-api:1.0

# Run the container-api container with the name container-api sharing the application source directory
docker run -d --name container-api -v ./app:/api/app container-api:1.0

# Try to access the application running inside the container in the browser through port 8000
# http://localhost:8000

# Remove the container via Docker extension

# Add the machine:container port mapping to access the container
docker run -d -p 8000:8000 --name container-api -v ./app:/api/app container-api:1.0

# Response to the request
# JSON -> {message: "I am inside the container..."}

# Change the request text to "The application has been changed..." in the main.py file

# Access the browser again on port 8000 and receive the response from the request
# JSON -> {"message": "The application has been changed..."}

# Remove the container container-api via Docker extension

# Changing the port mapping so that port 9000 redirects requests to port 8000 of the container
docker run -d -p 9000:8000 --name container-api -v ./app:/api/app container-api:1.0

# Access port 9000 through the browser and be redirected to port 8000 of the container
# Receive the response from the application request
# JSON -> {"message": "The application has been changed..."}

# Remove the container container-api via Docker extension

# Changing the port mapping so that port 8000 redirects requests to port 9000 of the container
docker run -d -p 8000:9000 --name container-api -v ./app:/api/app container-api:1.0

# Check port mapping with the inspect command
docker inspect container-api

# The application no longer has access to the container's server port via port mapping

# Remove the container container-api via Docker extension

```

## Orchestrating Multiple Containers using Docker Compose

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 5-intro-docker-compose
cd 5-intro-docker-compose

# Build images for all services in the docker-compose.yaml file
docker-compose build

# Check the images created after the build
docker images

# Run containers for all services in the docker-compose.yaml file in detached mode
docker-compose up -d

# Check the API routes
# / -> {"message": "I am inside the container..."}
# /environment_variable -> {"USERNAME": "DOCKER"}

# Bring down the set of containers using the down command
docker-compose down

# Check the current status of containers
docker ps -a

```

## Developing a Full Stack Application (front-end and back-end) using containers that communicate on the same network

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 6-docker-compose-environment
cd 6-docker-compose-environment

# Build images for all services in the docker-compose.yaml file
docker-compose build

# Run containers for all services in the docker-compose.yaml file in detached mode
docker-compose up -d

# Remove all containers, volumes, and networks using the down command with the -v tag
docker-compose down -v


```

## Pushing an Image to Docker Hub (Registry)

Type in the terminal

```bash

# Access the directory before the current one
cd ..

# Enter the folder 7-push-image-registry
cd 7-push-image-registry

# Build the image with instructions from the Dockerfile
docker build . -t odd-even-game:1.0

# Test the locally created image
docker run -i odd-even-game:1.0

# Push the image to your private Docker Hub repository with your Docker Hub username
docker push `DOCKER_HUB_USERNAME`/odd-even-game:1.0 # Example from the lesson: helderprado/odd-even-game:1.0

# Build the image again with the default tag to push to your private Docker Hub repository
docker build . -t `DOCKER_HUB_USERNAME`/odd-even-game:1.0

# Push the image to your private Docker Hub repository with your Docker Hub username
docker push `DOCKER_HUB_USERNAME`/odd-even-game:1.0

# Check the image on Docker Hub (Registry)
# https://hub.docker.com

# Remove the local image with the same name
docker rmi `DOCKER_HUB_USERNAME`/odd-even-game:1.0

# Check the images available locally in Docker
docker images

# Pull the image just stored on Docker Hub to the local image repository
docker pull `DOCKER_HUB_USERNAME`/odd-even-game:1.0

# Test the newly pulled image for the local docker repository
docker run -i `DOCKER_HUB_USERNAME`/odd-even-game:1.0


```
