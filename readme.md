# Getting Started with Docker

![](images/image1.jpg)

Documentation based on Pluralsight course [Getting Started with Docker](https://app.pluralsight.com/library/courses/getting-started-docker)

## :gear: Technology
- Docker

## :notebook: Setup Docker

- Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Create a [Docker Account](https://hub.docker.com/signup)

## :video_game: Play with Docker
Docker provides a playground if you don't want to run this on your local machine:
- Login to [Play with Docker](https://labs.play-with-docker.com/)
- Add a new instance

## :rocket: Deploying a Containerized Application

- Copy the course authors [repository](https://github.com/nigelpoulton/gsd)
- Navigate into the `first-container` folder

| Command                                                   | Description |
|-----------------------------------------------------------|-------------|
| `docker image build -t qagaryparker/gsd:ctr --no-cache .` | build image |

> `qagaryparker` is the Docker Hub ID, `gsd` is the repository name, `ctr` is the image name/tag

| Command                                  | Description            |
|------------------------------------------|------------------------|
| `docker image ls`                        | list docker images     |
| `docker image push qagaryparker/gsd:ctr` | push image to registry |

> You will be able to view this on your [Docker Hub Account](https://hub.docker.com/repository/docker)

## :runner: Running a Containerized Application

| Command                                                                | Description                     |
|------------------------------------------------------------------------|---------------------------------|
| `docker container run -d --name web -p 8080:8080 qagaryparker/gsd:ctr` | to run image in container       |
| `docker container ls`                                                  | list running containers         |
| `docker image rm qagaryparker/gsd:ctr`                                 | remove image from local machine |

> Local Containers using this image must be stopped and deleted before removing the image

| Command                                                                | Description                             |
|------------------------------------------------------------------------|-----------------------------------------|
| `docker container run -d --name web -p 8000:8080 qagaryparker/gsd:ctr` | to run image in container (second time) |

> As there is no local instance, this image is pulled from Docker Hub - you can view the web app on `localhost:8000`

## :blue_book: Managing a Containerized Application

| Command                                          | Description                                  |
|--------------------------------------------------|----------------------------------------------|
| `docker container stop web`                      | stop running container                       |
| `docker container ls -a`                         | list all containers (even those not running) |
| `docker container start web`                     | start container                              |
| `docker container rm web`                        | remove container                             |
| `docker container run -it --name test alpine sh` | run container                                |

> `-it` is interactive terminal, `alpine` is the image, `sh` is the main application running inside the container

- `exit` exits the `sh` shell process and kills the container
- `ctrl + p + q` detaches from the running container and frees up the terminal
- `docker container rm test -f` to force remove the container

## :books: Multi-container Apps with Docker Compose

Navigate into the `multi-container` folder

| Command                | Description                                                |
|------------------------|------------------------------------------------------------|
| `docker-compose up -d` | build, create, start and attach to container for a service |

> You can view the web app on `localhost:5000`

| Command               | Description                                                             |
|-----------------------|-------------------------------------------------------------------------|
| `docker-compose down` | stop and remove containers, networks, volumes and images create by `up` |

## :honeybee: Docker Swarm

> 'You should maintain an odd number of managers in the swarm to support manager node failures. Having an odd number of managers ensures that during a network partition, there is a higher chance that the quorum remains available to process requests if the network is partitioned into two sets.'

- For more information see the Docker [documentation](https://docs.docker.com/engine/swarm/admin_guide/#add-manager-nodes-for-fault-tolerance)
---
- Go to [Play with Docker](https://labs.play-with-docker.com/)
- Select the template for 3 managers and 2 workers

> Follow the steps below if you are not using a template

#### Manager 1

| Command                                           | Description                 |
|---------------------------------------------------|-----------------------------|
| `docker swarm init --advertise-addr=192.168.0.20` | initalise the swarm         |
| `docker swarm join-token manager`                 | Generate manager join token |

> This will generate a command - copy and run this on the nodes you want to be managers

| Command                          | Description                |
|----------------------------------|----------------------------|
| `docker swarm join-token worker` | Generate worker join token |

> This will generate a command - copy and run this on the nodes you want to be workers

| Command                                                                              | Description                                      |
|--------------------------------------------------------------------------------------|--------------------------------------------------|
| `docker node ls`                                                                     | list active managers and workers                 |
| `docker service create --name web -p 8080:8080 --replicas 3 qagaryparker/gsd:docker` | create 3 replicas of the same image or container |
| `docker service ls`                                                                  | lsit replicated services                         |
| `docker service ps web`                                                              | list replicas and where they are running         |
| `docker service scale web=10`                                                        | scale services                                   |
| `docker container rm 5c7tjba3hhd8`                                                   | remove specific container                        |

> After removing this container, Docker will add a new container to match the scaled value of 10

---

Building Swarm from docker-compose:

- Navigate to the `swarm-stack` folder

| Command                                                | Description                               |
|--------------------------------------------------------|-------------------------------------------|
| `docker image build -t qagaryparker/gsd:swarm-stack .` | build swarm-stack' image                  |
| `docker image push qagaryparker/gsd:swarm-stack`       | push to Docker Hub registry               |
| `docker stack deploy -c docker-compose.yml counter`    | deploy stack from a compose file on swarm |
| `docker stack ls`                                      | list stacks                               |
| `docker stack services counter`                        | list stack details                        |
| `docker stack ps counter`                              | list containers in stack                  |
| `docker stack rm counter`                              | remove stack                              |