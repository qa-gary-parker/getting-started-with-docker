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

> `qagaryparker` is the Docker Hub ID, `gsd` is the repository name, `ctr` is the image name/tag

- `docker image ls` to list your docker images
- `docker image push qagaryparker/gsd:ctr` to push the image to a registry

> You will be able to view this on your [Docker Hub Account](https://hub.docker.com/repository/docker)

## :runner: Running a Containerized App

- `docker container run -d --name web -p 8080:8080 qagaryparker/gsd:ctr` to run the image in a container
- `docker container ls` to list running containers
- `docker image rm qagaryparker/gsd:ctr` to remove the image from your local machine

> Local Containers using this image must be stopped and deleted before removing the image

- `docker container run -d --name web -p 8000:8080 qagaryparker/gsd:ctr`

> As there is no local instance, this image is pulled from Docker Hub - you can view the web app on `localhost:8000`

## :blue_book: Managing a Containerized App

- `docker container stop web` to stop the running container
- `docker container ls -a` to list all containers (even those not running)
- `docker container start web` to start the container
- `docker container rm web` to remove the container
- `docker container run -it --name test alpine sh`

> `-it` is interactive terminal, `alpine` is the image, `sh` is the main app. running inside the container

- `exit` exits the `sh` shell process and kills the container
- `ctrl + p + q` detaches from the running container and frees up the terminal
- `docker container rm test -f` to force remove the container

## :books: Multi-container Apps with Docker Compose

- Navigate into the `multi-container` folder
- `docker-compose up -d` to build, create, start and attach to container for a service

> You can view the web app on `localhost:5000`

- `docker-compose down` to stop and remove containers, networks, volumes and images create by `up`

## :honeybee: Docker Swarm

> 'You should maintain an odd number of managers in the swarm to support manager node failures. Having an odd number of managers ensures that during a network partition, there is a higher chance that the quorum remains available to process requests if the network is partitioned into two sets.'

- For more information see the Docker [documentation](https://docs.docker.com/engine/swarm/admin_guide/#add-manager-nodes-for-fault-tolerance)
---
- Go to [Play with Docker](https://labs.play-with-docker.com/)
- Select the template for 3 managers and 2 workers

> Follow the steps below if you are not using a template

#### Manager 1
- `docker swarm init --advertise-addr=192.168.0.20` to initalise the swarm
- `docker swarm join-token manager`

> This will generate a command - copy and run this on the nodes you want to be managers

- `docker swarm join-token worker`

> This will generate a command - copy and run this on the nodes you want to be workers

- `docker node ls` to view active managers and workers
- `docker service create --name web -p 8080:8080 --replicas 3 qagaryparker/gsd:docker` to create 3 replicas of the same image or container
- `docker service ls` to view replicated services
- `docker service ps web` to view each replica and where it is running
- `docker service scale web=10` to scale the services
- `docker container rm 5c7tjba3hhd8` to remove a specific container

> After removing this container, Docker will add a new container to match the scaled value of 10
---
> Steps to build Swarm from docker-compose file
- Navigate to the `swarm-stack` folder
- `docker image build -t qagaryparker/gsd:swarm-stack .` to build the 'swarm-stack' image
- `docker image push qagaryparker/gsd:swarm-stack` to push to a Docker Hub registry
- `docker stack deploy -c docker-compose.yml counter` to deploy a stack from a compose file on the swarm
- `docker stack ls` to list stacks
- `docker stack services counter` to view stack details
- `docker stack ps counter` to view containers in the stack
- `docker stack rm counter` to remove stack

|                                                        |                                             |
|--------------------------------------------------------|---------------------------------------------|
| `docker image build -t qagaryparker/gsd:swarm-stack .` | build the 'swarm-stack' image               |
| `docker image push qagaryparker/gsd:swarm-stack`       | push to a Docker Hub registry               |
| `docker stack deploy -c docker-compose.yml counter`    | deploy stack from compose file to the swarm |
| `docker stack ls`                                      | list stacks                                 |
| `docker stack services counter`                        | view stack details                          |
| `docker stack ps counter`                              | view containers in stack                    |
| `docker stack rm counter`                              | remove stack                                |