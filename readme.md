# Getting Started with Docker

- Documentation based on Pluralsight course [Getting Started with Docker](https://app.pluralsight.com/library/courses/getting-started-docker)

## :gear: Technology
- Docker

## :notebook: Setup Docker

- Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Create a [Docker Account](https://hub.docker.com/signup)

## :video_game: Play with Docker
Docker provides a playground if you don't want to run this on your local machine:
- Login to [Play with Docker](https://labs.play-with-docker.com/)
- Add a new instance

## :rocket: Deploying a Containerized App

- Copy the course authors [repository](https://github.com/nigelpoulton/gsd)
- Navigate into the `first-container` folder
- `docker image build -t qagaryparker/gsd:ctr --no-cache .` to build an image

> Note: `qagaryparker` is the Docker Hub ID, `gsd` is the repository name, `ctr` is the image name/tag

- `docker image ls` to list your docker images

- `docker image push qagaryparker/gsd:ctr` to push the image to a registry

> Note: you will be able to view this on your [Docker Hub Account](https://hub.docker.com/repository/docker)

## :runner: Running a Containerized App

- `docker container run -d --name web -p 8080:8080 qagaryparker/gsd:ctr` to run the image in a container

- `docker container ls` to list running containers

- `docker image rm qagaryparker/gsd:ctr` to remove the image from your local machine

> Note: Local Containers using this image must be stopped and deleted before removing the image

- `docker container run -d --name web -p 8000:8080 qagaryparker/gsd:ctr`

> Note: As there is no local instance, this image is pulled from Docker Hub

## :blue_book: Managing a Containerized App

- `docker container stop web` to stop the running container

- `docker container ls -a` to list all containers (even those not running)

- `docker container start web` to start the container

- `docker container rm web` to remove the container

- `docker container run -it --name test alpine sh`

>Note: `-it` is interactive terminal, `alpine` is the image, `sh` is the main app. running inside the container

- `exit` exits the `sh` shell process and kills the container

- `ctrl + p + q` detaches from the running container and frees up the terminal

- `docker container rm test -f` to force remove the container