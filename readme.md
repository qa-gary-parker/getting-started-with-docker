# Getting Started with Docker

- Documentation based on Pluralsight course [Getting Started with Docker](https://app.pluralsight.com/library/courses/getting-started-docker)

## :gear: Technology
- Docker

## :notebook: Setup Docker

- Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Create a [Docker Account](https://hub.docker.com/signup)

## :runner: Play with Docker
Docker provides a playground if you don't want to run this on your local machine:
- Login to [Play with Docker](https://labs.play-with-docker.com/)
- Add a new instance

## :rocket: Deploying a containerized App

- Copy the course authors [repository](https://github.com/nigelpoulton/gsd)
- Navigate into the `first-container` folder
- `docker image build -t qagaryparker/gsd:ctr --no-cache .` to build an image

> Note: `qagaryparker` is the Docker Hub ID, `gsd` is the repository name, `ctr` is the image name/tag

- `docker image ls` to list your docker images

- `docker image push qagaryparker/gsd:ctr` to push the image to a registry

> Note: you will be able to view this on your [Docker Hub Account](https://hub.docker.com/repository/docker)

- `docker container run -d --name web -p 8080:8080 qagaryparker/gsd:ctr` to run the image in a container

- `docker container ls` to view running containers



